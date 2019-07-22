---
title: RaspberryPi-01-GPIO
date: 2019-06-23 15:06:31
categories: Raspberry Pi
tags:
- Raspberry Pi
- GPIO

---

# Raspberry Pi GPIO

树莓派B预留了13x2个GPIO引脚，其中也包含了一些片内外设的接口(比如I2C，SPI和UART)。个引脚的分配如下:

![pinmap](https://cdn.sparkfun.com/assets/learn_tutorials/4/2/4/header_pinout.jpg)

有两种方式可以控制io口的电平，方式之一是通过Python，另一种方式是通过C。需要注意的是二者代码中IO引脚的白哦是方式有区别（上图中NAME列）。

# 例程

C语言版本

```c
#include <wiringPi.h>
#include <stdio.h>
#include <stdlib.h>

int led_pin = 6;

int main ()
{
    if (wiringPiSetup() == -1)
    {
        printf("Setup wiringPi failed!");
        return 1;
    }
    printf("linker_led pin : GPIO%d (wiringPi pin)\n",led_pin);
    pinMode(led_pin, OUTPUT); // set mode to output

    while(1) 
    {
        digitalWrite(led_pin, 1); // output a high level 
        delay(200);
        digitalWrite(led_pin, 0); // output a low level 
        delay(200);
    }

    return 0;
}
```

接下来编译运行，

```
gcc c_file_name.c -o out_file_name -lwiringPi
sudo ./out_file_name
```

Python版本

```python
import RPi.GPIO as GPIO
import time

led_pin = 25

GPIO.setwarnings(False)
GPIO.setmode(GPIO.BCM)
GPIO.setup(led_pin,GPIO.OUT)

print ("linker led pin 25 (BCM GPIO)")

while True:
    GPIO.output(led_pin,True)
    time.sleep(0.2)
    GPIO.output(led_pin,False)
    time.sleep(0.2)
```

# 参考

https://www.oschina.net/question/1425530_140979

