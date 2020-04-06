---
title: "Accessing internal network by ssh tunnel"
excerpt: "How to access internal network by ssh tunnel."
categories: 
  - cheatsheet
tags: 
  - ssh
published: true
comments: true
toc: true
support: true
order: 9
author: vugia.truong
---

# Summary

Because of the corona virus pandemic, recently i have to work from home. 
My gpu servers is behind my company's internal network. 
So i have to create a gateway and open a ssh tunnel through that gateway. 
Later from outside, i will login into the gateway and use the opened ssh tunnel 
to access into the gpu servers

# Procedure

The procedure is as follow 

* The init state

```bash

                                            / Internal Network
                                           |  
                                           |  +------------+
                                           |--| GPU servers|
                                           |  +------------+
                                           |  
                                            \ 


```

* Create aws machine. Other type of cloud provider such as Azure, or Google Cloud will be the same. The result i choose
aws is because Amazon provide always free machine. So i can use it as gateway without worrying about credit or machine expiration.

When creating machine, please remember to download the private keypair and select machine type as Ubuntu.
 In our case it is **ssh_tunnel.pem** file.
On the aws console, it will look like this

![ssh_tunnel](/assets/images/cheatsheet/ssh_tunnel_001.png)

The from GPU server, check whether it can access to the created AWS instance

```bash
# Replace the * with corresponding value of your Public DNS (IPV4) 
ssh -i ~/.ssh/ssh_tunnel.pem ubuntu@ec2-*-*-*-*.us-east-2.compute.amazonaws.com
```

The graph will be as follow

```bash

                                            / Internal Network
                          +----------+     |  
                          |   AWS    |     |  +------------+
                          | instance |<----|--| GPU servers|
                          +----------+     |  +------------+
                                           |  
                                            \ 



                                            / Internal Network
                          +----------+     |  
+-------------+    ssh    |   GCE    | ssh |  +----------+
| Laptop dynIP|---------->| instance |-----|->| Machine A|
+-------------+           +----------+     |  +----------+
                                           |
                                            \
```

* From the created aws machine, create the ssh tunnel

```bash
# Replace the * with corresponding value of your Public DNS (IPV4) 
ssh -i ~/.ssh/ssh_tunnel.pem -o UserKnownHostsFile=/dev/null -o CheckHostIP=no -o StrictHostKeyChecking=no -f -N -R 2022:*:22 ubuntu@ec2-*-*-*-*.us-east-2.compute.amazonaws.com
```

The graph will be as follow

```bash

                                            / Internal Network
                          +----------+     |  
                          |   AWS    |     |  +------------+
                          | instance |<----|->| GPU servers|
                          +----------+     |  +------------+
                                           |  
                                            \ 
```

* We can access from outside now

```bash
# Access to the aws machine
ssh -i ~/.ssh/ssh_tunnel.pem ubuntu@ec2-*-*-*-*.us-east-2.compute.amazonaws.com
# Then access to gpu server
ssh -p 2022 gpu_server_user@localhost
```


```bash



                                            / Internal Network
                          +----------+     |  
+-------------+    ssh    |   AWS    | ssh |  +------------+
| Laptop dynIP|---------->| instance |-----|->| GPU servers|
+-------------+           +----------+     |  +------------+
                                           |
                                            \
```

# Reference

1. [gcloud ssh tunnel](https://stackoverflow.com/questions/58339624/using-google-gcloud-to-ssh-tunnel-into-linux-machine-inside-network)

End