---
title: "Deploying image classification (mxnet) using Mxnet Model Server (mms)"
excerpt: "How to deploying image classification (mxnet) using Mxnet Model Server (mms)."
categories:
  - tutorial
tags:
  - mms
published: true
comments: true
toc: true
support: true
order: 9
author: vugia.truong
---

# Summary

In this post, i will summarize steps required when deploying a simple image classification (mxnet) using Mxnet Model Server (mms).

# Detail

Like other deeplearning framework, mxnet also provides tons a pretrained model and tools cover nearly all of machine learning task like image classification, object detection, segmentation, ... These pretrained models and tools are inside the [gluon-cv](https://gluon-cv.mxnet.io/) package.

For similarity, i'll use the pretrained Resnet50 model. For other model please see [model_zoo](https://gluon-cv.mxnet.io/model_zoo)

* [source code : https://github.com/gachiemchiep/mms_example](https://github.com/gachiemchiep/mms_example). 

## Run the image classification service

* Download source code

```bash
$ git clone https://github.com/gachiemchiep/mms_example
```

* Re-create anaconda environment and activate it

```bash
$ cd mmx_example
# This will create environment called DL with python=3.7
$ conda env create -f environment.yml
# Activate anaconda
$ conda activate DL
```

* Create mms model archive file

```bash
$ mkdir mms_example/mms_example/services
$ cd mms_example/mms_example
$ model-archiver --model-name mxnet_resnet50 --model-path mxnet_resnet50 --handler img_classifier_service:handle --export-path ../model-archives/ --force
# A new file called mxnet_resnet50.mar should be created inside model-archives directory
```

* Start mms server

```bash
$ cd mms_example/mms_example
$ mxnet-model-server --start
```

* Test

```bash
$ cd mms_example/mms_example/resources
$ curl -X POST http://127.0.0.1:8080/predictions/mxnet_resnet50 -T chair.jpg 
```

* Output should be as follow

```bash
[
  {
    "rocking chair": "0.4921491"
  },
  {
    "folding chair": "0.08000506"
  },
  {
    "wool": "0.027606748"
  },
  {
    "cowboy hat": "0.024664406"
  },
  {
    "velvet": "0.020736441"
  }
]
```

* Shutdown the model server

```bash
mxnet-model-server --stop
```

## Source code explain

At this state, our source will be as follow

```bash
mms_example/
├── environment.yml
├── LICENSE
├── mms_example
│   ├── common
│   ├── config.properties
│   ├── __init__.py
│   ├── logs                                        : log directory
│   │   ├── access_log.log
│   │   ├── mms_log.log
│   │   ├── mms_metrics.log
│   │   ├── model_log.log
│   │   └── model_metrics.log
│   ├── model-archives                  
│   │   └── mxnet_resnet50.mar                      : model archieve file which we created
│   ├── resources
│   ├── services
│   │   ├── __init__.py
│   │   └── mxnet_resnet50
│   │       ├── img_classifier_service.py           : service file
│   │       └── __init__.py
│   └── tests
└── README.md
```

The most important file is [img_classifier_service.py](https://github.com/gachiemchiep/mms_example/blob/master/mms_example/services/mxnet_resnet50/img_classifier_service.py), this include all of our logic from loading model, reading uploader data, inference, ... 

* Load model 

```python
class ResNet50Classifier():
    def __init__(self):
        self.ctx = None
        self.net = None
        self.initialized = False

    def initialize(self, context):
        """
        Load the model and mapping file to perform infernece.
        :param context: model relevant worker information
        :return:
        """

        properties = context.system_properties
        gpu_id = properties.get("gpu_id")
        self.ctx = mx.cpu() if gpu_id is None else mx.gpu(gpu_id)
        self.net = gluoncv.model_zoo.get_model("ResNet50_v1d", pretrained=True, ctx=self.ctx)
        self.initialized = True
```

* Read uploaded data and convert to mxnet's NDArray

```python
    def preprocess(self, data):
        """
        Scales, crops, and normalizes a PIL image for a mxnet model,
        :param data:
        :return: ndarray
        """

        img_arrs = []
        for idx in range(len(data)):
            img = data[idx].get("data")
            if img is None:
                img = data[idx].get("body")

            if img is None:
                img = data[idx].get("data")

            if img is None or len(img) == 0:
                self.error = "Empty image input"
                return None

            img_arr = mx.image.imdecode(img)
            img_arrs.append(img_arr)

        img_arrs = gluoncv.data.transforms.presets.imagenet.transform_eval(img_arrs)
        return img_arrs
```

* Do the inferencing

```python
    def inference(self, img, topk=5):
            """
            :param img:
            :param topk:
            :return:
            """
            pred = self.net(img.as_in_context(self.ctx))
            # map predicted values to probability by softmax
            probs = mx.nd.softmax(pred)[0].asnumpy()
            # find the 5 class indices with the highest score
            inds = mx.nd.topk(pred, k=topk)[0].astype('int').asnumpy().tolist()

            rets = []
            for i in range(topk):
                ret = dict()
                ret[self.net.classes[inds[i]]] = str(probs[inds[i]])
                rets.append(ret)

            return [rets]

```

* The final logic

```python
_service = ResNet50Classifier()


def handle(data, context):
    if not _service.initialized:
        _service.initialize(context)

    if data is None:
        return None

    data = _service.preprocess(data)
    data = _service.inference(data)
    data = _service.postprocess(data)

    return data

```

In the last section, we used the following command to create model-archive

```bash
$ model-archiver --model-name mxnet_resnet50 --model-path mxnet_resnet50 --handler img_classifier_service:handle --export-path ../model-archives/ --force
```

As we can see, the model-archive will use **handle** method as logic. This method is defined inside the   **img_classifier_service.py** file.

## Benchmark result

My computer has the following specs :

```bash
cpu:  
                    AMD Ryzen 5 2600 Six-Core Processor
graphics card:
                    nVidia GP106 [GeForce GTX 1060 6GB]
memory:
                    16GB
```

My computer only have GTX-1060 GPU card, the benchmarking result is as follow :

```bash
# benchmarking command
$ ab -k -l -n 10000 -c 100 -T "image/jpeg" -p kitten.jpg http://127.0.0.1:8080/predictions/mxnet_resnet50 
# This will send 10000 request with 100 concurrency. So please be patient. i would take around 5 minutes

# GPU usage: only 673 MB
(DL) gachiemchiep@gachiemchiep:services$ nvidia-smi 
Thu Jun 13 20:55:58 2019       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 418.67       Driver Version: 418.67       CUDA Version: 10.1     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce GTX 106...  On   | 00000000:1C:00.0  On |                  N/A |
| 40%   59C    P2    46W / 120W |   1702MiB /  6075MiB |     45%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|    0      1933      G   /usr/lib/xorg/Xorg                           699MiB |
|    0      3685      G   ...quest-channel-token=3700236306674113463   214MiB |
|    0      7697      G   ...p/pycharm-educational/12/jre64/bin/java     2MiB |
|    0      8227      C   ...chiep/opt/miniconda2/envs/DL/bin/python   673MiB |
|    0     22775      G   ...-token=CCBE267BA54B1DB7F35623396BB493CD    43MiB |



# Benchmark result
Benchmarking 127.0.0.1 (be patient)
Completed 1000 requests
Completed 2000 requests
Completed 3000 requests
Completed 4000 requests
Completed 5000 requests
Completed 6000 requests
Completed 7000 requests
Completed 8000 requests
Completed 9000 requests
Completed 10000 requests
Finished 10000 requests


Server Software:        
Server Hostname:        127.0.0.1
Server Port:            8080

Document Path:          /predictions/mxnet_resnet50
Document Length:        Variable

Concurrency Level:      100
Time taken for tests:   242.090 seconds
Complete requests:      10000
Failed requests:        0
Keep-Alive requests:    10000
Total transferred:      4320000 bytes
Total body sent:        1111520000
HTML transferred:       1970000 bytes
Requests per second:    41.31 [#/sec] (mean)
Time per request:       2420.901 [ms] (mean)
Time per request:       24.209 [ms] (mean, across all concurrent requests)
Transfer rate:          17.43 [Kbytes/sec] received
                        4483.74 kb/s sent
                        4501.16 kb/s total

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.6      0       6
Processing:    53 2409 154.0   2417    2679
Waiting:       53 2409 154.0   2417    2679
Total:         58 2409 153.6   2417    2679

Percentage of the requests served within a certain time (ms)
  50%   2417
  66%   2445
  75%   2462
  80%   2477
  90%   2519
  95%   2572
  98%   2618
  99%   2636
 100%   2679 (longest request)

```

* Note : by default configuration, mms only queue 100 jobs. So if you need more concurrency, increase the value of  job_queue_size inside [config.properties](https://github.com/gachiemchiep/mms_example/blob/master/mms_example/config.properties). With a value of job_queue_size=1000 , i can increase the -c to 200

```bash
# benchmarking command
$ ab -k -l -n 10000 -c 200 -T "image/jpeg" -p kitten.jpg http://127.0.0.1:8080/predictions/mxnet_resnet50 

# Benchmark result
Benchmarking 127.0.0.1 (be patient)
Completed 1000 requests
Completed 2000 requests
Completed 3000 requests
Completed 4000 requests
Completed 5000 requests
Completed 6000 requests
Completed 7000 requests
Completed 8000 requests
Completed 9000 requests
Completed 10000 requests
Finished 10000 requests


Server Software:        
Server Hostname:        127.0.0.1
Server Port:            8080

Document Path:          /predictions/mxnet_resnet50
Document Length:        Variable

Concurrency Level:      200
Time taken for tests:   241.262 seconds
Complete requests:      10000
Failed requests:        0
Keep-Alive requests:    10000
Total transferred:      4320000 bytes
Total body sent:        1111520000
HTML transferred:       1970000 bytes
Requests per second:    41.45 [#/sec] (mean)
Time per request:       4825.240 [ms] (mean)
Time per request:       24.126 [ms] (mean, across all concurrent requests)
Transfer rate:          17.49 [Kbytes/sec] received
                        4499.13 kb/s sent
                        4516.61 kb/s total

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    7  83.2      0    1023
Processing:  2037 4771 198.9   4794    6032
Waiting:     2037 4771 198.9   4794    6031
Total:       2043 4778 230.1   4794    7054

Percentage of the requests served within a certain time (ms)
  50%   4794
  66%   4830
  75%   4851
  80%   4864
  90%   4903
  95%   4936
  98%   4965
  99%   5006
 100%   7054 (longest request)

```

Quite a good benchmark resulti guess.

End
