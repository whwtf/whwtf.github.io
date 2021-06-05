---
title: 在树莓派上操作 rgbLED 显示不同颜色
categories:
  - Linux
  - SomePi
tags:
  - RaspberryPi
  - rgbLED
date: 2021-04-15 11:52:17
---

常见的 rgbLED 有四根引脚，一个 GND，另外三个分别是红绿蓝三色控制脚。通常由输入的电压不同来显示不同的颜色。但是树莓派的 GPIO 输出的是数字信号，这里可以用编程的方式来模拟电压的变化。  
树莓派上用 PWM（脉宽调制）方法来实现，这是一种对模拟信号电平进行数字编码的方法。PWM 可以简单理解为，通过可控频率的高低电平切换来实现模拟电压变化的方法。

```
import RPi.GPIO as GPIO
import time

R, G, B = 20, 16, 21

GPIO.setwarnings(False)
GPIO.setmode(GPIO.BCM)

GPIO.setup(R, GPIO.OUT)
GPIO.setup(G, GPIO.OUT)
GPIO.setup(B, GPIO.OUT)

pwmR = GPIO.PWM(R, 70)    
pwmG = GPIO.PWM(G, 70)
pwmB = GPIO.PWM(B, 70)

pwmR.start(0)
pwmG.start(0)
pwmB.start(0)

try:
    t = 0.5
    while True:
        
        
        
        pwmR.ChangeDutyCycle(100)
        pwmG.ChangeDutyCycle(0)
        pwmB.ChangeDutyCycle(0)
        time.sleep(t)
         
        
        pwmR.ChangeDutyCycle(0)
        pwmG.ChangeDutyCycle(100)
        pwmB.ChangeDutyCycle(0)
        time.sleep(t)
         
        
        pwmR.ChangeDutyCycle(0)
        pwmG.ChangeDutyCycle(0)
        pwmB.ChangeDutyCycle(100)
        time.sleep(t)
         
        
        pwmR.ChangeDutyCycle(100)
        pwmG.ChangeDutyCycle(100)
        pwmB.ChangeDutyCycle(0)
        time.sleep(t)
         
        
        pwmR.ChangeDutyCycle(100)
        pwmG.ChangeDutyCycle(0)
        pwmB.ChangeDutyCycle(100)
        time.sleep(t)
         
        
        pwmR.ChangeDutyCycle(0)
        pwmG.ChangeDutyCycle(100)
        pwmB.ChangeDutyCycle(100)
        time.sleep(t)
         
        
        pwmR.ChangeDutyCycle(100)
        pwmG.ChangeDutyCycle(100)
        pwmB.ChangeDutyCycle(100)
        time.sleep(t)

        
        for r in range(0, 101, 20):
            pwmR.ChangeDutyCycle(r)
            for g in range(0, 101, 20):
                pwmG.ChangeDutyCycle(g)
                for b in range(0, 101, 20):
                    pwmB.ChangeDutyCycle(b)
                    time.sleep(0.05)

except KeyboardInterrupt:
    pass

pwmR.stop()
pwmG.stop()
pwmB.stop()

GPIO.cleanup()
```
