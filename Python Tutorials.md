# Python Tutorials

**Keyestudio IoT Smart Home Kit for ESP32**

## 1. Thonny Installation
    
    1.  ## Open the Thonny Package 
        
        Please refer to the folder shown below:

![](media/29fa7a9563dec2dddbfc7ed79d197380.png)

## 2. Thonny Interface

    Open the Thonny

![](media/d753621b40cd7405ce034e93e0f5678a.png)

Main interface functions:

![](media/d41b79772c9846fd8bf295c8451f8321.png)

![](media/3d04fe6893ca104e4e593a0786cb3799.png)

## 3. Select ESP32 Development Environment
    
    Click Python.exe，then select Configure interpreter

![](media/30d66dba96cfabbe2bd3b6c858564ef2.png)

Select MicroPython(ESP32) from the Interpreter interface

![](media/5f92c4dd24223cf49d6da075aa53307f.png)

## 4. Installing Firmware
    
    Download link：<https://micropython.org/download/esp32/>

Choose to download version V1.17

![](media/4f1f3b0568c3ae2ca3288431df340184.png)

Of course, we also provide the downloaded firmware, as shown below.

![](media/150da4adbcbf76f47793254a2ae56967.png)

Burn microPython firmware

Connect the smart home to your computer with a USB.

Click Install or update firmware

![](media/9b8470fee22b3a6aa4ac21d1e1d10eda.png)

Select Port

![](media/d3bff3f1b25076733717273e94616088.png)

Click Browser to find the the downloaded version V1.17 firmware

![](media/ad4cfc202f014101ddd9f5373773635f.png)

![](media/aef61c4411d53f83685ad570c7d3a075.png)

Click install

![](media/507ff0c04761a509f729a8c4e88e4b27.png)

Choose Port or WebREPL as the driver of ESP32 mainboard CH340(COM)

![](media/c1c4ae83635b2c0186b1985aeef844ae.png)

![](media/19514aef3fdd86fb2c033c6441d8ff6e.png)

The ESP32 environment has been installed.

Thonny interface

![](media/c42ed7549ff7ff1f7deabd64710cb98e.png)

## Projects

Please refer to the file below：

![](media/528951558fa53138c60b87e160a94d05.png)

### Project 1: Control LED

we will first learn how to control LED.

![](media/0cda68ae8719d9b6c1bb79d64160d31d.png)

1.  **Working Principle**

LED is also the light-emitting diode, which can be made into an
electronic module. It will shine if we control pins to output high
level, otherwise it will be off.  

2.  **[Parameter](C:/Users/NINGMEI/AppData/Local/youdao/dict/Application/8.10.7.0/resultui/html/index.html#/javascript:;)s**

|                 |          |
| --------------- | -------- |
| Working voltage | DC 3\~5V |
| Working current | \<20mA   |
| Power           | 0.1W     |

3.  **Control Pin**

|            |    |
| ---------- | -- |
| Yellow LED | 12 |

#### Project 1.1 LED Flashing

1.  **Description**

We can make the LED pin output high level and low level to make the LED
flash.  

2.  **Test Code**

<table>
<tbody>
<tr class="odd">
<td><p>from machine import Pin</p>
<p>import time</p>
<p>led = Pin(12, Pin.OUT)# Build an LED object, connect the external LED light to pin 0, and set pin 0 to output mode</p>
<p>while True:</p>
<p>led.value(1)# turn on led</p>
<p>time.sleep(1)# delay 1s</p>
<p>led.value(0)# turn off led</p>
<p>time.sleep(1)# delay 1s</p></td>
</tr>
</tbody>
</table>

1.  Open the sample code
    
    ![](media/39658e26aad2c9794bd3db9df3c70732.png)
    
    ![](media/166384572a1fa595858d933aea5af710.png)

2.  Operation and test result
    
    Click the button
    
    ![](media/c5e28dda04f63745f59ef351025e82e8.png)

We can see that the yellow LED is flashing .  

#### Project 1.2 Breathing LED

1.  **Description**

A“breathing LED”is a phenomenon where an LED's brightness smoothly
changes from dark to bright and back to dark, continuing to do so and
giving the illusion of an LED“breathing. However, how to control LED’s
brightness?

It makes sense to take advantage of PWM. Output the number of high level
and low level in unit time, the more time the high level occupies, the
larger the PWM value, the brighter the LED. 

![](media/704984700612966b997127cb9bde5c96.jpeg)

2.  **Test Code**

<table>
<tbody>
<tr class="odd">
<td><p>import time</p>
<p>from machine import Pin,PWM</p>
<p>#The way that the ESP32 PWM pins output is different from traditionally controllers.</p>
<p>#It can change frequency and duty cycle by configuring PWM’s parameters at the initialization stage.</p>
<p>#Define GPIO 0’s output frequency as 10000Hz and its duty cycle as 0, and assign them to PWM.</p>
<p>pwm =PWM(Pin(12,Pin.OUT),10000,0)</p>
<p>try:</p>
<p>while True:</p>
<p>#The range of duty cycle is 0-1023, so we use the first for loop to control PWM to change the duty</p>
<p>#cycle value,making PWM output 0% -100%; Use the second for loop to make PWM output 100%-0%.</p>
<p>for i in range(0,1023):</p>
<p>pwm.duty(i)</p>
<p>time.sleep_ms(1)</p>
<p>for i in range(0,1023):</p>
<p>pwm.duty(1023-i)</p>
<p>time.sleep_ms(1)</p>
<p>except:</p>
<p>#Each time PWM is used, the hardware Timer will be turned ON to cooperate it. Therefore, after each use of PWM,</p>
<p>#deinit() needs to be called to turned OFF the timer. Otherwise, the PWM may fail to work next time.</p>
<p>pwm.deinit()</p></td>
</tr>
</tbody>
</table>

3.  **Test Result**
    
    Click the button
    
    ![](media/609b283e0909b5e5c14809c4ccf892ed.png)

The LED gradually gets dimmer then brighter, cyclically, like human
breathe.

### Project 2: Table Lamp

1.  **Description**

The common table lamp uses LED lights and buttons, which can control the
light on and off pressing the button.

2.  **Button Principle**

The button module is a digital sensor, which can only read 0 or 1. When
the module is not pressed, it is in a high level state, that is, 1, when
pressed, it is a low level 0. 

![](media/41f565d4f355abb96e105119660e80ba.png)

3.  **Pins of the Button**

|          |    |
| -------- | -- |
| Button 1 | 16 |
| Button 2 | 27 |

#### Project 2.1 Read the Button

**1. Description**

We will work to read the status value of the button and display it on
the serial monitor, so as to see it intuitively.  

**2. Test Code**

<table>
<tbody>
<tr class="odd">
<td><p>button1 = Pin(16, Pin.IN, Pin.PULL_UP)</p>
<p>button2 = Pin(27, Pin.IN, Pin.PULL_UP)</p>
<p>while True:</p>
<p>btnVal1 = button1.value() # Reads the value of button 1</p>
<p>btnVal2 = button2.value()</p>
<p>print("button1 =",btnVal1) #Print it out in the shell</p>
<p>print("button2 =",btnVal2)</p>
<p>time.sleep(0.1) #delay 0.1s</p></td>
</tr>
</tbody>
</table>

**3. Test Result**

Click the run button, then you can see the status values of button1 and
button 2 printed in shell. Click the button of the smart home, and you
can see the change of the status values.

![](media/1b984da67c0e89a72a9601c39362567d.png)

#### Project 2.2. Table Lamp 

**1. Description**

For common simple table lamp, click the button it will be opened, click
it again, the lamp will be closed.  

2.  **Test Code**

Calculate the clicked button times and take the remainder of 2, you can
get 0 or 1 two state values.  

<table>
<tbody>
<tr class="odd">
<td><p>from machine import Pin</p>
<p>import time</p>
<p>button1 = Pin(16, Pin.IN, Pin.PULL_UP)</p>
<p>led = Pin(12, Pin.OUT)</p>
<p>count = 0</p>
<p>while True:</p>
<p>btnVal1 = button1.value() # Reads the value of button 1</p>
<p>#print("button1 =",btnVal1) #Print it out in the shell</p>
<p>if(btnVal1 == 0):</p>
<p>time.sleep(0.01)</p>
<p>while(btnVal1 == 0):</p>
<p>btnVal1 = button1.value()</p>
<p>if(btnVal1 == 1):</p>
<p>count = count + 1</p>
<p>print(count)</p>
<p>val = count % 2</p>
<p>if(val == 1):</p>
<p>led.value(1)</p>
<p>else:</p>
<p>led.value(0)</p>
<p>time.sleep(0.1) #delay 0.1s</p></td>
</tr>
</tbody>
</table>

3.  **Test Result**

The shell will print out the clicked button times, then click the button
once, the LED will be on, click it again, it will be off.  

![](media/1bc079eabd93cb2e8a8e15f0ab7f1367.png)

### Project 3: PIR Motion Sensor

**1. Description**

The PIR motion sensor has many application scenarios in daily life, such
as automatic induction lamp of stairs, automatic induction faucet of
washbasin, etc.  

It is also a digital sensor like buttons, which has two state

values 0 or 1.  And it will be sensed when people are moving.  

![](media/c1518252606b111bfa66878a2bfcc965.png)

2.  **Control Pin**

|                   |    |
| ----------------- | -- |
| PIR motion sensor | 14 |

#### Project 3.1 Read the PIR Motion Sensor

We will print out the value of the PIR motion sensor through the serial
monitor.

1.  **Test Code**

<table>
<tbody>
<tr class="odd">
<td><p>from machine import Pin</p>
<p>import time</p>
<p>PIR = Pin(14, Pin.IN)</p>
<p>while True:</p>
<p>value = PIR.value()</p>
<p>print(value, end = " ")</p>
<p>if value == 1:</p>
<p>print("Some body is in this area!")</p>
<p>else:</p>
<p>print("No one!")</p>
<p>time.sleep(0.1)</p></td>
</tr>
</tbody>
</table>

2.  **Test Result**

When you stand still in front of the sensor, the reading value is 0,
move a little, it will change to 1.

![](media/f8c6be9a6ad7a6423c1fa1456f771406.png)

#### Project 3.2 PIR Motion Sensor

If someone moves in front of the sensor, the LED will light up.  

1.  **Test Code**

<table>
<tbody>
<tr class="odd">
<td><p>from machine import Pin</p>
<p>import time</p>
<p>PIR = Pin(14, Pin.IN)</p>
<p>led = Pin(12, Pin.OUT)</p>
<p>while True:</p>
<p>value = PIR.value()</p>
<p>print(value)</p>
<p>if value == 1:</p>
<p>led.value(1)# turn on led</p>
<p>else:</p>
<p>led.value(0)</p>
<p>time.sleep(0.1)</p></td>
</tr>
</tbody>
</table>

2.  **Test Result**

Move your hand in front of the sensor,  the LED will turn on. After a
few seconds of immobility, the LED will turn off.  

### Project 4: Play Music

**1. Description**

There is a audio power amplifier element in the car expansion board,
which is as an external amplification equipment to play music.

In this project, we will work to play a  piece of music by using it. 

**2. Component Knowledge**

**Passive Buzzer:** The audio power amplifier (like the passive buzzer)
does not have internal oscillation. When controlling, we need to input
square waves of different frequencies to the positive pole of the
component and ground the negative pole to control the power amplifier to
chime sounds of different frequencies.

![](media/2e6fd6b7975ef84ab94eee896161347b.png)

3.  **Control Pin**

|                |    |
| -------------- | -- |
| Passive Buzzer | 25 |

#### Project 4.1 Play Happy Birthday

**1. Test Code**

<table>
<tbody>
<tr class="odd">
<td><p>from machine import Pin, PWM</p>
<p>from time import sleep</p>
<p>buzzer = PWM(Pin(25))</p>
<p>buzzer.duty(1000)</p>
<p># Happy birthday</p>
<p>buzzer.freq(294)</p>
<p>sleep(0.25)</p>
<p>buzzer.freq(440)</p>
<p>sleep(0.25)</p>
<p>buzzer.freq(392)</p>
<p>sleep(0.25)</p>
<p>buzzer.freq(532)</p>
<p>sleep(0.25)</p>
<p>buzzer.freq(494)</p>
<p>sleep(0.25)</p>
<p>buzzer.freq(392)</p>
<p>sleep(0.25)</p>
<p>buzzer.freq(440)</p>
<p>sleep(0.25)</p>
<p>buzzer.freq(392)</p>
<p>sleep(0.25)</p>
<p>buzzer.freq(587)</p>
<p>sleep(0.25)</p>
<p>buzzer.freq(532)</p>
<p>sleep(0.25)</p>
<p>buzzer.freq(392)</p>
<p>sleep(0.25)</p>
<p>buzzer.freq(784)</p>
<p>sleep(0.25)</p>
<p>buzzer.freq(659)</p>
<p>sleep(0.25)</p>
<p>buzzer.freq(532)</p>
<p>sleep(0.25)</p>
<p>buzzer.freq(494)</p>
<p>sleep(0.25)</p>
<p>buzzer.freq(440)</p>
<p>sleep(0.25)</p>
<p>buzzer.freq(698)</p>
<p>sleep(0.25)</p>
<p>buzzer.freq(659)</p>
<p>sleep(0.25)</p>
<p>buzzer.freq(532)</p>
<p>sleep(0.25)</p>
<p>buzzer.freq(587)</p>
<p>sleep(0.25)</p>
<p>buzzer.freq(532)</p>
<p>sleep(0.5)</p>
<p>buzzer.duty(0)</p></td>
</tr>
</tbody>
</table>

**2. Test Result**

The passive buzzer will play happy Birthday.

### Project 5: Automatic Doors and Windows

1.  **Description**

Automatic doors and windows need power device, which will become more
automatic with a 180 degree servo and some sensors. Adding a raindrop
sensor, you can achieve the effect of closing windows automatically when
raining. If adding a RFID, we can realize the effect of swiping to open
the door and so on.  

2.  **Component Knowledge**
    
    **Servo:** Servo is a position servo
    [driver](C:/Users/NINGMEI/AppData/Local/youdao/dict/Application/8.10.7.0/resultui/html/index.html#/javascript:;)
    device consists of a housing, a circuit board, a coreless motor, a
    gear and a position detector.

Its working principle is that the servo receives the signal sent by MCU
or receiver and produces a reference signal with a period of 20ms and
width of 1.5ms, then compares the acquired DC bias voltage to the
voltage of the potentiometer and obtain the voltage difference output.

The IC on the circuit board judges the direction of rotation, and then
drives the coreless motor to start rotation. The power is transmitted to
the swing arm through the reduction gear, and the signal is sent back by
the position detector to judge whether the positioning has been reached,
which is suitable for those control systems that require constant angle
change and can be maintained.  

When the motor speed is constant, the potentiometer is driven to rotate
through the cascade reduction gear, which leads that the voltage
difference is 0, and the motor stops rotating. Generally, the angle
range of servo rotation is 0° --180 °.

The pulse period of the control servo is 20ms, the pulse width is 0.5ms
\~ 2.5ms, and the corresponding position is -90°\~ +90°.  Here is an
example of a 180° servo:  

![](media/708316fde05c62113a3024e0efb0c237.jpeg)

In general, servo has three lines in brown, red and orange. The brown
wire is grounded, the red one is a positive pole line and the orange one
is a signal line.

![](media/35084ae289a08e35bdb8c89ceb134ba4.png)

![](media/6cbf6f177ea204f7632b872497fde010.png)

3.  **Pin**

|                         |    |
| ----------------------- | -- |
| The servo of the window | 5  |
| The servo of the door   | 13 |

#### Project 5.1 Control the Door 

**1. Test Code**

<table>
<tbody>
<tr class="odd">
<td><p>from machine import Pin, PWM</p>
<p>import time</p>
<p>pwm = PWM(Pin(13))</p>
<p>pwm.freq(50)</p>
<p>'''</p>
<p>Duty cycle corresponding to the Angle</p>
<p>0°----2.5%----25</p>
<p>45°----5%----51.2</p>
<p>90°----7.5%----77</p>
<p>135°----10%----102.4</p>
<p>180°----12.5%----128</p>
<p>'''</p>
<p>angle_0 = 25</p>
<p>angle_90 = 77</p>
<p>angle_180 = 128</p>
<p>while True:</p>
<p>pwm.duty(angle_0)</p>
<p>time.sleep(1)</p>
<p>pwm.duty(angle_90)</p>
<p>time.sleep(1)</p>
<p>pwm.duty(angle_180)</p>
<p>time.sleep(1)</p></td>
</tr>
</tbody>
</table>

**2. Test Result**

The servo of the door turns with the door, back and forth

#### Project 5.2 Close the Window

1.  **Description**
    
    We will work to use a servo and a raindrop sensor to make an device
    closing windows automatically when raining.  

2.  **Component Knowledge**

**Raindrop Sensor:** This is an analog input module, the greater the
area covered by water on the detection surface, the greater the value
returned (range 0\~4096). 

3.  **Test Code**

<table>
<tbody>
<tr class="odd">
<td><p># Import Pin, ADC and DAC modules.</p>
<p>from machine import ADC,Pin,DAC,PWM</p>
<p>import time</p>
<p>pwm = PWM(Pin(5))</p>
<p>pwm.freq(50)</p>
<p># Turn on and configure the ADC with the range of 0-3.3V</p>
<p>adc=ADC(Pin(34))</p>
<p>adc.atten(ADC.ATTN_11DB)</p>
<p>adc.width(ADC.WIDTH_12BIT)</p>
<p># Read ADC value once every 0.1seconds, convert ADC value to DAC value and output it,</p>
<p># and print these data to “Shell”.</p>
<p>try:</p>
<p>while True:</p>
<p>adcVal=adc.read()</p>
<p>dacVal=adcVal//16</p>
<p>voltage = adcVal / 4095.0 * 3.3</p>
<p>print("ADC Val:",adcVal,"DACVal:",dacVal,"Voltage:",voltage,"V")</p>
<p>if(voltage &gt; 0.6):</p>
<p>pwm.duty(46)</p>
<p>else:</p>
<p>pwm.duty(100)</p>
<p>time.sleep(0.1)</p>
<p>except:</p>
<p>pass</p></td>
</tr>
</tbody>
</table>

4.  **Test Result**

At first, the window opens automatically, and when you touch the
raindrop sensor with your hand (which has water on the skin), the window
will close.  

### Project 6: Atmosphere Lamp

1.  **Description**

The atmosphere lamp of smart home is 4 SK6812RGB LEDs. RGB LED belongs
to a simple luminous module, which can adjust the color to bring out the
lamp effect of different colors. Furthermore, it can be widely used in
buildings, bridges, roads, gardens, courtyards, floors and other fields
of decorative lighting and venue layout, Christmas, Halloween,
Valentine's Day, Easter, National Day as well as other festivals during
the atmosphere and other scenes.

In this experiment, we will make various lighting effects.  

2.  **Component Knowledge**
    
    From the schematic diagram, we can see that these four RGB LEDs are
    all connected in series. In fact, no matter how many they are, we
    can use a pin to control a RGB LED and let it display any color.
    Each RGBLED is an independent pixel, composed of R, G and B colors,
    which can achieve 256 levels of brightness display and complete the
    full true color display of 16777216 colors. 
    
    What’s more, the pixel point contains a data latch signal shaping
    amplifier drive circuit and a signal shaping circuit, which
    effectively ensures the color of the pixel point light is highly
    consistent.
    
    ![](media/86e292d0666046b72a1e0e68adfb17e8.png)
    
    ![](media/c0df93f61c6b9272f62b1847ccfbdb10.png)

3.  **Pin**

|        |    |
| ------ | -- |
| SK6812 | 26 |

#### Project 6.1 Control SK6812

We will control SK6812 to display various lighting effects.

**1. Test Code**

<table>
<tbody>
<tr class="odd">
<td><p>#Import Pin, neopiexl and time modules.</p>
<p>from machine import Pin</p>
<p>import neopixel</p>
<p>import time</p>
<p>#Define the number of pin and LEDs connected to neopixel.</p>
<p>pin = Pin(26, Pin.OUT)</p>
<p>np = neopixel.NeoPixel(pin, 4)</p>
<p>#brightness :0-255</p>
<p>brightness=100</p>
<p>colors=[[brightness,0,0], #red</p>
<p>[0,brightness,0], #green</p>
<p>[0,0,brightness], #blue</p>
<p>[brightness,brightness,brightness], #white</p>
<p>[0,0,0]] #close</p>
<p>#Nest two for loops to make the module repeatedly display five states of red, green, blue, white and OFF.</p>
<p>while True:</p>
<p>for i in range(0,5):</p>
<p>for j in range(0,4):</p>
<p>np[j]=colors[i]</p>
<p>np.write()</p>
<p>time.sleep_ms(50)</p>
<p>time.sleep_ms(500)</p>
<p>time.sleep_ms(500)</p></td>
</tr>
</tbody>
</table>

**2. Test Result**

The atmosphere lamps of the smart home will display red,greenish blue as
well as white.

#### Project 6.2 Button 

1.  **Description**

There are two switch buttons to change the color of the atmosphere lamp.

2.  **Test Code**

<table>
<tbody>
<tr class="odd">
<td><p>#Import Pin, neopiexl and time modules.</p>
<p>from machine import Pin</p>
<p>import neopixel</p>
<p>import time</p>
<p>button1 = Pin(16, Pin.IN, Pin.PULL_UP)</p>
<p>button2 = Pin(27, Pin.IN, Pin.PULL_UP)</p>
<p>count = 0</p>
<p>#Define the number of pin and LEDs connected to neopixel.</p>
<p>pin = Pin(26, Pin.OUT)</p>
<p>np = neopixel.NeoPixel(pin, 4)</p>
<p>#brightness :0-255</p>
<p>brightness=100</p>
<p>colors=[[0,0,0],</p>
<p>[brightness,0,0], #red</p>
<p>[0,brightness,0], #green</p>
<p>[0,0,brightness], #blue</p>
<p>[brightness,brightness,brightness] #white</p>
<p>] #close</p>
<p>def func_color(val):</p>
<p>for j in range(0,4):</p>
<p>np[j]=colors[val]</p>
<p>np.write()</p>
<p>time.sleep_ms(50)</p>
<p>#Nest two for loops to make the module repeatedly display five states of red, green, blue, white and OFF.</p>
<p>while True:</p>
<p>btnVal1 = button1.value() # Reads the value of button 1</p>
<p>#print("button1 =",btnVal1) #Print it out in the shell</p>
<p>if(btnVal1 == 0):</p>
<p>time.sleep(0.01)</p>
<p>while(btnVal1 == 0):</p>
<p>btnVal1 = button1.value()</p>
<p>if(btnVal1 == 1):</p>
<p>count = count - 1</p>
<p>print(count)</p>
<p>if(count &lt;= 0):</p>
<p>count = 0</p>
<p>btnVal2 = button2.value()</p>
<p>if(btnVal2 == 0):</p>
<p>time.sleep(0.01)</p>
<p>while(btnVal2 == 0):</p>
<p>btnVal2 = button2.value()</p>
<p>if(btnVal2 == 1):</p>
<p>count = count + 1</p>
<p>print(count)</p>
<p>if(count &gt;= 4):</p>
<p>count = 4</p>
<p>if(count == 0):</p>
<p>func_color(0)</p>
<p>elif(count == 1):</p>
<p>func_color(1)</p>
<p>elif(count == 2):</p>
<p>func_color(2)</p>
<p>elif(count == 3):</p>
<p>func_color(3)</p>
<p>elif(count == 4):</p>
<p>func_color(4)</p></td>
</tr>
</tbody>
</table>

3.  **Test Result**

We can switch the color of the atmosphere lamp by clicking buttons 1 and
2.

### Project 7: Fan

1.  **Description**

In this project, we will learn how to make a small fan.

2.  **Component Knowledge**

The small fan uses a 130 DC motor and safe fan blades.  You can use PWM
output to control the fan speed.  

![](media/33da52918e88862a94035d61a9050f2e.png)

3.  **Control Method**

Two pins are required to control the motor of the fan, one for INA and
two for INB.  The PWM value range is 0\~255. When the PWM output of the
two pins is different, the fan can rotate.  

|                   |                                                                                                                                |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| INA - INB \<= -45 | Rotate clockwise                                                                                                               |
| INA - INB \>= 45  | Rotate [anticlockwise](C:/Users/NINGMEI/AppData/Local/youdao/dict/Application/8.10.7.0/resultui/html/index.html#/javascript:;) |
| INA ==0, INB == 0 | Stop                                                                                                                           |

4.  **Control Pins**

|     |    |
| --- | -- |
| INA | 19 |
| INB | 18 |

#### Project 7.1 Control the Fan

We can control the
[anticlockwise](C:/Users/NINGMEI/AppData/Local/youdao/dict/Application/8.10.7.0/resultui/html/index.html#/javascript:;)
and clockwise rotation speed of the fan.

**1. Test Code**

<table>
<tbody>
<tr class="odd">
<td><p>from machine import Pin,PWM</p>
<p>import time</p>
<p>#Two pins of the motor</p>
<p>INA =PWM(Pin(19,Pin.OUT),10000,0)#INA corresponds to IN+</p>
<p>INB =PWM(Pin(18,Pin.OUT),10000,2)#INB corresponds to IN-</p>
<p>try:</p>
<p>while True:</p>
<p>#Counterclockwise 2s</p>
<p>INA.duty(0) #The range of duty cycle is 0-1023</p>
<p>INB.duty(700)</p>
<p>time.sleep(2)</p>
<p>#stop 1s</p>
<p>INA.duty(0)</p>
<p>INB.duty(0)</p>
<p>time.sleep(1)</p>
<p>#Turn clockwise for 2s</p>
<p>INA.duty(600)</p>
<p>INB.duty(0)</p>
<p>time.sleep(2)</p>
<p>#stop 1s</p>
<p>INA.duty(0)</p>
<p>INB.duty(0)</p>
<p>time.sleep(1)</p>
<p>except:</p>
<p>INA.duty(0)</p>
<p>INB.duty(0)</p>
<p>INA.deinit()</p>
<p>INB.deinit()</p></td>
</tr>
</tbody>
</table>

**2. Test Result**

The fan will rotate clockwise and
[anticlockwise](C:/Users/NINGMEI/AppData/Local/youdao/dict/Application/8.10.7.0/resultui/html/index.html#/javascript:;)
at different

speeds.

#### Project 7.2 Switch On or Off the Fan 

One button switches the fan on and the other button controls the speed
of the fan.

**1. Test Code**

<table>
<tbody>
<tr class="odd">
<td><p>from machine import Pin,PWM</p>
<p>import time</p>
<p>#Two pins of the motor</p>
<p>INA =PWM(Pin(19,Pin.OUT),10000,0)#INA corresponds to IN+</p>
<p>INB =PWM(Pin(18,Pin.OUT),10000,2)#INB corresponds to IN-</p>
<p>button1 = Pin(16, Pin.IN, Pin.PULL_UP)</p>
<p>count = 0</p>
<p>try:</p>
<p>while True:</p>
<p>btnVal1 = button1.value() # Reads the value of button 1</p>
<p>if(btnVal1 == 0):</p>
<p>time.sleep(0.01)</p>
<p>while(btnVal1 == 0):</p>
<p>btnVal1 = button1.value()</p>
<p>if(btnVal1 == 1):</p>
<p>count = count + 1</p>
<p>print(count)</p>
<p>val = count % 2</p>
<p>if(val == 1):</p>
<p>INA.duty(0) #The range of duty cycle is 0-1023</p>
<p>INB.duty(700)</p>
<p>else:</p>
<p>INA.duty(0)</p>
<p>INB.duty(0)</p>
<p>except:</p>
<p>INA.duty(0)</p>
<p>INB.duty(0)</p>
<p>INA.deinit()</p>
<p>INB.deinit()</p></td>
</tr>
</tbody>
</table>

**2. Test Result**

Click button 1, the fan starts to rotate, click button 2, the

speed can be adjusted(there are three different speeds), press the
button 1 again, the fan stops. 

### Project 8: LCD1602 Display

1.  **Description**

As we all know, screen is one of the best ways for people to interact
with electronic devices.  

2.  **Component Knowledge**

1602 is a line that can display 16 characters. There are two lines,
which use IIC communication protocol.  

![](media/066e093f1711ada67d3309ddc9bdc66e.png)

3.  **Control Pins**

|     |     |
| --- | --- |
| SDA | SDA |
| SCL | SCL |

#### Project 8.1 Display [Character](C:/Users/NINGMEI/AppData/Local/youdao/dict/Application/8.10.7.0/resultui/html/index.html#/javascript:;)s

1.  **Description**
    
    We will use library file i2c\_lcd.py and lcd\_api.py, which should
    be saved in the ESP32 memory.

2.  **Operations**
    
    Open the i2c\_lcd.py and lcd\_api.py
    
    ![](media/b5d74645d450d329aded48064bd599c8.png)
    
    Select File \> save as \> MicroPython device
    
    ![](media/0b0e9857fb550f54a436bd92c64b00c7.png)
    
    ![](media/64b696646e34078f43d764690c3e3a48.png)
    
    The saved name id i2c\_lcd.py and lcd\_api.py
    
    ![](media/c7a374c92ed24402abd0d8c479a7d132.png)

3.  **Test Code**

<table>
<tbody>
<tr class="odd">
<td><p>from time import sleep_ms, ticks_ms</p>
<p>from machine import I2C, Pin</p>
<p>from i2c_lcd import I2cLcd</p>
<p>DEFAULT_I2C_ADDR = 0x27</p>
<p>i2c = I2C(scl=Pin(22), sda=Pin(21), freq=400000)</p>
<p>lcd = I2cLcd(i2c, DEFAULT_I2C_ADDR, 2, 16)</p>
<p>lcd.move_to(1, 0)</p>
<p>lcd.putstr('Hello')</p>
<p>lcd.move_to(1, 1)</p>
<p>lcd.putstr('keyestudio')</p>
<p># The following line of code should be tested</p>
<p># using the REPL:</p>
<p># 1. To print a string to the LCD:</p>
<p># lcd.putstr('Hello world')</p>
<p># 2. To clear the display:</p>
<p>#lcd.clear()</p>
<p># 3. To control the cursor position:</p>
<p># lcd.move_to(2, 1)</p>
<p># 4. To show the cursor:</p>
<p># lcd.show_cursor()</p>
<p># 5. To hide the cursor:</p>
<p>#lcd.hide_cursor()</p>
<p># 6. To set the cursor to blink:</p>
<p>#lcd.blink_cursor_on()</p>
<p># 7. To stop the cursor on blinking:</p>
<p>#lcd.blink_cursor_off()</p>
<p># 8. To hide the currently displayed character:</p>
<p>#lcd.display_off()</p>
<p># 9. To show the currently hidden character:</p>
<p>#lcd.display_on()</p>
<p># 10. To turn off the backlight:</p>
<p>#lcd.backlight_off()</p>
<p># 11. To turn ON the backlight:</p>
<p>#lcd.backlight_on()</p>
<p># 12. To print a single character:</p>
<p>#lcd.putchar('x')</p>
<p># 13. To print a custom character:</p>
<p>#happy_face = bytearray([0x00, 0x0A, 0x00, 0x04, 0x00, 0x11, 0x0E, 0x00])</p>
<p>#lcd.custom_char(0, happy_face)</p>
<p>#lcd.putchar(chr(0))</p></td>
</tr>
</tbody>
</table>

4.  **Test Result**

The first line of the LCD1602 shows hello and the second line shows
keyestudio.  

#### Project 8.2 Dangerous Gas Alarm 

**1. Description**

When a gas sensor detects a high concentration of dangerous gas, the
buzzer will sound an alarm and the display will show dangerous.

**2. Component Knowledge**

**MQ2 Smoke Sensor**: It is a gas leak monitoring device for homes and
factories, which is suitable for liquefied gas, benzene, alkyl, alcohol,
hydrogen as well as smoke detection.  Our sensor leads to digital pin D
and analog output pin A, which is connected to D as a digital sensor in
this project .  

![](media/4550c4935e6c08e595a1e8707b54b551.png)

**3. Control Pin**

|            |    |
| ---------- | -- |
| Gas Sensor | 23 |

**4. Test Code**

<table>
<tbody>
<tr class="odd">
<td><p>from time import sleep_ms, ticks_ms</p>
<p>from machine import I2C, Pin</p>
<p>from i2c_lcd import I2cLcd</p>
<p>DEFAULT_I2C_ADDR = 0x27</p>
<p>i2c = I2C(scl=Pin(22), sda=Pin(21), freq=400000)</p>
<p>lcd = I2cLcd(i2c, DEFAULT_I2C_ADDR, 2, 16)</p>
<p>from machine import Pin</p>
<p>import time</p>
<p>gas = Pin(23, Pin.IN, Pin.PULL_UP)</p>
<p>while True:</p>
<p>gasVal = gas.value() # Reads the value of button 1</p>
<p>print("gas =",gasVal) #Print it out in the shell</p>
<p>lcd.move_to(1, 1)</p>
<p>lcd.putstr('val: {}'.format(gasVal))</p>
<p>if(gasVal == 1):</p>
<p>#lcd.clear()</p>
<p>lcd.move_to(1, 0)</p>
<p>lcd.putstr('Safety ')</p>
<p>else:</p>
<p>lcd.move_to(1, 0)</p>
<p>lcd.putstr('dangerous')</p>
<p>time.sleep(0.1) #delay 0.1s</p></td>
</tr>
</tbody>
</table>

5.  **Test Result**

The screen displays "safety" in normal state. However, when the gas
sensor detects some dangerous gases, such as carbon monoxide, at a
certain concentration, the buzzer will sound an alarm and the screen
displays "dangerous".  

### Project 9: Temperature and Humidity Sensor

1.  **Component Knowledge**

Its communication mode is serial data and single bus. The temperature
measurement range is -20 \~ +60℃, accuracy is ±2℃. However, the humidity
range is 5 \~ 95%RH, the accuracy is ±5%RH.  

![](media/0b9c44c3e4f3706638b9cf15871b861c.png)

2.  **Control Pin**

|                                 |    |
| ------------------------------- | -- |
| Temperature and Humidity Sensor | 17 |

#### Project 9.1 Temperature and Humidity Tester

**1. Test Code**

<table>
<tbody>
<tr class="odd">
<td><p># Import machine, time and dht modules.</p>
<p>import machine</p>
<p>import time</p>
<p>import dht</p>
<p>from time import sleep_ms, ticks_ms</p>
<p>from machine import I2C, Pin</p>
<p>from i2c_lcd import I2cLcd</p>
<p>#Associate DHT11 with Pin(17).</p>
<p>DHT = dht.DHT11(machine.Pin(17))</p>
<p>DEFAULT_I2C_ADDR = 0x27</p>
<p>i2c = I2C(scl=Pin(22), sda=Pin(21), freq=400000)</p>
<p>lcd = I2cLcd(i2c, DEFAULT_I2C_ADDR, 2, 16)</p>
<p>while True:</p>
<p>DHT.measure() # Start DHT11 to measure data once.</p>
<p># Call the built-in function of DHT to obtain temperature</p>
<p># and humidity data and print them in “Shell”.</p>
<p>print('temperature:',DHT.temperature(),'℃','humidity:',DHT.humidity(),'%')</p>
<p>lcd.move_to(1, 0)</p>
<p>lcd.putstr('T: {}'.format(DHT.temperature()))</p>
<p>lcd.move_to(1, 1)</p>
<p>lcd.putstr('H: {}'.format(DHT.humidity()))</p>
<p>time.sleep_ms(1000)</p></td>
</tr>
</tbody>
</table>

**2. Test Result**

The LCD1602 displays the temperature (T = \*\* ° C) and humidity (H =
\*\* %RH). When you breathe into the T/H sensor, you can see that the
humidity rises.  

### Project 10: RFID RC522 Module

1.  **Component Knowledge**

Radio frequency identification, the card reader is composed of a radio
frequency module and a high-level magnetic field. The Tag transponder is
a sensing device, which doesn’t contain a battery. It only contains tiny
integrated circuit chips and media for storing data and antennas for
receiving and transmitting signals.

To read the data in the tag, first put it into the reading range of the
card reader. The reader will generate a magnetic field, which can
produce electricity according to Lenz's law, then the RFID tag will
supply power, thereby activating the device.

![](media/982ac6a9da0e8f55465ca5a969ac0dfe.png)

2.  **Control Pins**
    
    Use IIC communication

|     |     |
| --- | --- |
| SDA | SDA |
| SCL | SCL |

#### Project 10.1 Open the Door

**1. Test Code**

![](media/03cab1a254dc41e5a07fda0a11daba59.png)

<table>
<tbody>
<tr class="odd">
<td><p>from machine import Pin, PWM,I2C, Pin</p>
<p>import time</p>
<p>from mfrc522_i2c import mfrc522</p>
<p>pwm = PWM(Pin(13))</p>
<p>pwm.freq(50)</p>
<p>button1 = Pin(16, Pin.IN, Pin.PULL_UP)</p>
<p>#i2c config</p>
<p>addr = 0x28</p>
<p>scl = 22</p>
<p>sda = 21</p>
<p>rc522 = mfrc522(scl, sda, addr)</p>
<p>rc522.PCD_Init()</p>
<p>rc522.ShowReaderDetails() # Show details of PCD - MFRC522 Card Reader details</p>
<p>data = 0</p>
<p>while True:</p>
<p>if rc522.PICC_IsNewCardPresent():</p>
<p>#print("Is new card present!")</p>
<p>if rc522.PICC_ReadCardSerial() == True:</p>
<p>print("Card UID:")</p>
<p>#print(rc522.uid.uidByte[0 : rc522.uid.size])</p>
<p>for i in rc522.uid.uidByte[0 : rc522.uid.size]:</p>
<p>data = data + i</p>
<p>print(data)</p>
<p>if(data == 656):</p>
<p>pwm.duty(128)</p>
<p>print("open")</p>
<p>else:</p>
<p>print("error")</p>
<p>data = 0</p>
<p>btnVal1 = button1.value()</p>
<p>if(btnVal1 == 0):</p>
<p>pwm.duty(25)</p>
<p>print("close")</p>
<p>time.sleep(1)</p></td>
</tr>
</tbody>
</table>

**2. Test Result**

Close the provided card to the RFID induction area, the door will turn
and open, and the shell shows "open". Click button 1 and the door turns
and closes. However, when swiping another blue induction block, the
shell shows "Error".  

![](media/03fd569d64704a7e9705c1891f4d4856.png)

### Project 11: [Morse](C:/Users/NINGMEI/AppData/Local/youdao/dict/Application/8.10.7.0/resultui/html/index.html#/javascript:;) [Code](C:/Users/NINGMEI/AppData/Local/youdao/dict/Application/8.10.7.0/resultui/html/index.html#/javascript:;) 

Morse code, also known as Morse password, is an on-again, off-again
signal code that expresses different letters, numbers, and punctuation
marks in different sequences. Now we use it as our password gate.

The Morse code corresponds to the following characters:

![](media/1a5e70c0d091e2617acbfc274827b4fd.png)

#### Project 11.1 Morse Code Open the Door

**1. Description**

We use ![](media/9491f7768f28ee4901e6fdb83632c27c.png)as the correct password. What’s more,
there is a button library file OneButton, which is very simple to click,
double click, long press and other functions. For Morse password, click
is “.”, long press and release is “-”.  

**2. Test Code**

<table>
<tbody>
<tr class="odd">
<td><p># Import machine, time and dht modules.</p>
<p>from machine import Pin, PWM</p>
<p>from time import sleep_ms, ticks_ms</p>
<p>from machine import I2C, Pin</p>
<p>from i2c_lcd import I2cLcd</p>
<p>DEFAULT_I2C_ADDR = 0x27</p>
<p>i2c = I2C(scl=Pin(22), sda=Pin(21), freq=400000)</p>
<p>lcd = I2cLcd(i2c, DEFAULT_I2C_ADDR, 2, 16)</p>
<p>button1 = Pin(16, Pin.IN, Pin.PULL_UP)</p>
<p>button2 = Pin(27, Pin.IN, Pin.PULL_UP)</p>
<p>count = 0</p>
<p>time_count = 0</p>
<p>password = "" #Enter password</p>
<p>correct_password = "-.-" #Correct password</p>
<p>lcd.putstr("Enter password")</p>
<p>pwm = PWM(Pin(13))</p>
<p>pwm.freq(50)</p>
<p>while True:</p>
<p>btnVal1 = button1.value() # Read the value of button 1</p>
<p>if(btnVal1 == 0):</p>
<p>sleep_ms(10)</p>
<p>while(btnVal1 == 0):</p>
<p>time_count = time_count + 1 #Start counting the pressed time of the button</p>
<p>sleep_ms(200) #The time is 200ms cumulative</p>
<p>btnVal1 = button1.value()</p>
<p>if(btnVal1 == 1):</p>
<p>count = count + 1</p>
<p>print(count)</p>
<p>print(time_count)</p>
<p>if(time_count &gt; 3): #If the pressed time of the button is more than 200*3ms，add"-" to password</p>
<p>lcd.clear()</p>
<p>#lcd.move_to(1, 1)</p>
<p>password = password + "-"</p>
<p>else:</p>
<p>lcd.clear()</p>
<p>password = password + "." #Otherwise add "."</p>
<p>lcd.putstr('{}'.format(password))</p>
<p>time_count = 0</p>
<p>btnVal2 = button2.value()</p>
<p>if(btnVal2 == 0):</p>
<p>if(password == correct_password): #If the password is correct</p>
<p>lcd.clear()</p>
<p>lcd.putstr("open")</p>
<p>pwm.duty(128) #Open the door</p>
<p>password = "" #Remove the password</p>
<p>sleep_ms(1000)</p>
<p>else: #If the password is wrong</p>
<p>lcd.clear()</p>
<p>lcd.putstr("error")</p>
<p>pwm.duty(25) #Close the door</p>
<p>sleep_ms(2000)</p>
<p>lcd.clear()</p>
<p>lcd.putstr("enter again")</p>
<p>password = "" #Remove the password</p></td>
</tr>
</tbody>
</table>

3.  **Test Result**

At first, the LCD1602 displays "Enter password", then click or long
press button 1 to tap the password. If we input the correct password
"-.-", then click button 2, the door will open, and the LCD1602 will
display "open".

If other incorrect passwords are entered, the door will be closed and
the LCD1602 will display error, which shows “enter again” 2s later.

### Project 12: WiFi 

The easiest way to access the Internet is to use a WiFi to connect. The
ESP32 main control board comes with a WiFi module, making our smart home
accessible to the Internet easily.

![](media/f74baff97695aa2ee33a8c19370d2547.png)

#### Project 12.1 Smart Home 

**1. Description**

We connect the smart home to a LAN, which is the WiFi in your home or
the hot spot of your phone. After the connection is successful, an
address will be assigned. We will print the assigned address in the
shell.  

**2. Test Code**

Note: ssiD and password in the code should be filled with your own WiFi
name and password.  

![](media/278cbdc272b5cc1a6461a7934eabe5c0.png)

<table>
<tbody>
<tr class="odd">
<td><p>import time</p>
<p>import network #Import network module</p>
<p>#Enter correct router name and password</p>
<p>ssidRouter = 'ChinaNet-2.4G-0DF0' #Enter the router name</p>
<p>passwordRouter = 'ChinaNet@233' #Enter the router password</p>
<p>def STA_Setup(ssidRouter,passwordRouter):</p>
<p>print("Setup start")</p>
<p>sta_if = network.WLAN(network.STA_IF) #Set ESP32 in Station mode</p>
<p>if not sta_if.isconnected():</p>
<p>print('connecting to',ssidRouter)</p>
<p>#Activate ESP32’s Station mode, initiate a connection request to the router</p>
<p>#and enter the password to connect.</p>
<p>sta_if.active(True)</p>
<p>sta_if.connect(ssidRouter,passwordRouter)</p>
<p>#Wait for ESP32 to connect to router until they connect to each other successfully.</p>
<p>while not sta_if.isconnected():</p>
<p>pass</p>
<p>#Print the IP address assigned to ESP32 in “Shell”.</p>
<p>print('Connected, IP address:', sta_if.ifconfig())</p>
<p>print("Setup End")</p>
<p>try:</p>
<p>STA_Setup(ssidRouter,passwordRouter)</p>
<p>except:</p>
<p>sta_if.disconnect()</p></td>
</tr>
</tbody>
</table>

**3. Test Result**

If the WiFi is connected successfully, the serial monitor will print out
the connected WiFi name and assigned IP address.  

![](media/8c021cf89562d7ee27a6446f54be17bf.png)

## 3. Resources

Download code, libraries and more details, please refer to the following
link:

https://fs.keyestudio.com/KS5009
