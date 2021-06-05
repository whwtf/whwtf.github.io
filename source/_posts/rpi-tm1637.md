---
title: 在树莓派上操作 TM1637 主控的四位数码管
categories:
  - Linux
  - SomePi
tags:
  - RaspberryPi
  - TM-1637
date: 2021-04-15 11:49:29
---

**TM1637** 驱动的数码管操作简单，只需要 DIO（串行数据）和 CLK（时钟控制）两个引脚就可以工作，3.3V 和 5V 电压都可以。  
在 [GitHub](https://github.com/) 上已经有先贤写好了控制模块，拿过来用就可以了，应用最多的就是 [tm1637.py](https://github.com/whwtf/python-learning/blob/master/tm1637.py)。

在使用的时候，当然首先就是：

```
import tm1637
```

唯一需要注意的是这个模块当中 **显示内容的传递是通过列表（list）来进行的** 。简单使用如下：

```
import RPi.GPIO as GPIO
import time
import tm1637

CLK = 21      
DIO  = 20     
GPIO.setmode(GPIO.BCM)
GPIO.setwarnings(False)    

d = tm1637.TM1637(CLK, DATA)    
d.showDoublePoint(1)    
d.showData([5,6,7,9])   

GPIO.cleanup()
```

其他进阶应用无非就是通过不通方法改变列表的内容来实现应用的目的，比方说一个 **数字时钟** ：

```
import tm1637
import RPi.GPIO as GPIO
import time

CLK = 21
DATA  = 20
GPIO.setmode(GPIO.BCM)
GPIO.setwarnings(False)

HHMMFORMAT = '%H:%M'    

digital1637 = tm1637.TM1637(CLK, DATA)
digital1637.showDoublePoint(1)

while(True):
    timenow = time.strftime('%Y-%m-%d %H:%M',time.localtime(time.time()))
    curTime = time.strftime(HHMMFORMAT, time.localtime(time.time()))
        if(curTime != lastTime):
            
            timer = time.localtime()
            number = [timer.tm_hour//10, timer.tm_hour%10, timer.tm_min//10, timer.tm_min%10]
            digital1637.showData(number)
            lastTime = curTime
        

    time.sleep(0.1)


GPIO.cleanup()
```
