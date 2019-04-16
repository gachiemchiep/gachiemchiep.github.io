---
title: "Quick glance at Ampere eMAG processer"
excerpt: "Quick glance at Ampere eMAG processer."
published: true
categories: 
  - arm
tags: 
  - server  
comments: true
toc: true
support: true
order: 9
author: vugia.truong
---


## Ampere eMAG processer

ARM is a family of bi-endian fixed-length instruction set architectures and extensions. The ARM architecture is widely used in the mobile and embedded markets. Today, ARM-based implementations are designed by a number of companies including [ARM Holdings](https://en.wikichip.org/wiki/arm_holdings), [Qualcomm](https://en.wikichip.org/wiki/qualcomm), [Apple](https://en.wikichip.org/wiki/apple), and [AppliedMicro](https://en.wikichip.org/wiki/apm).

eMAG is a family of 64-bit high-performance **ARM server microprocessors** designed **by Ampere Computing for the data center**. Based on [emag_wiki](https://en.wikichip.org/wiki/ampere_computing/emag), in March of 2019,
eMAG is still in its first generation. 

1st generation of eMAG is called **Ampere eMAG 8180 64-bit Arm processor**. The detail specification could be 
viewed at [specification](https://amperecomputing.com/wp-content/uploads/2018/09/eMAG8180_PB_v0.5_20180914-2.pdf) . 

![Ampere eMAG 8180 64-bit Arm processor feature](/assets/images/2019/eMAG-8180-spec.png)

In March 20, 2018, **Ampere computing** release their report at OCP Summit. 
In that report, they compared their server rack with intel-chip usage server rack. 
They archieved from 15%, 35%, 20% improve on Perf/Watt, Perf/Dollar, Perf/Rack compared with Intel Xeon Gold. Which is very impressive. The following figure is taken from 
[their report](https://www.opencompute.org/files/18150J-Ampere-PPT-OCPSummitKumar-final.pdf)

![Ampere eMAG Value Proposition with Cassandra](/assets/images/2019/eMAG_compare.png)

Despite the big improvement in Perf/Watt, Perf/Dollar and Perf/Rack, 
the cost of eMAG chip usage server rack is only about 1/2 or 1/3 compared to Intel
chip usage rack. [source: youtube](https://youtu.be/78XSWxjZXB4?t=162) .

The biggest disadvantage of **Ampere eMAG** is that they're still very young processor. 
Increasing Perf/Watt, Perf/Dollar and Perf/Rack is good but there're 2 very important factors that's still in a mist.

1. stabality (heat,...)
2. ecosystem

## Conclusion

For decades, Intel dominated the server/cloud machine marketplace.
Entirely server ecosystem are built for intel chipset. 
Switching from Intel into Arm-chipset is a big-risk move and will cost tons of money as well. This will make large corp hesiate and slowly but surely kill the **Ampere eMAG**

Let's not forget the trading war between China and America. 
Sooner or later, the America will ban all the selling of Intel chipset to 
China's corp (see some [articles](https://www.theregister.co.uk/2015/04/10/us_intel_china_ban/)). 
The china is in an urge to find a replacement for Intel processor. 
So that should be one of many reasons why Lenovo coperate with 
Ampere computing to develop and spear out the Ampere eMAG processor.