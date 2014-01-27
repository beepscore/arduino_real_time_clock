# Purpose
Use Arduino with real time clock (RTC) module.

# References

## Meetup- Time to Get Wired
2014-01-26  
http://www.meetup.com/KingMakers/events/159661162/

## I2C Devices and Arduino
by Dan Tebbs

## RTC Modules
### Adafruit
DS1307 Real Time Clock Breakout Board Kit  
http://learn.adafruit.com/ds1307-real-time-clock-breakout-board-kit/overview  
DS1307-breakout-board schematic and layout files  
https://github.com/adafruit/DS1307-breakout-board  

Chronodot - newer than DS1307. Temperature compensated, more accurate  
http://www.adafruit.com/products/255

### Vetco
Real Time Clock Module (DS1307) for Arduino  
Vetco Electronics, Bellevue WA  
http://www.vetco.net/catalog/product_info.php?products_id=12866  
Part Number: VUPN5978  
$11.99

### Sparkfun
https://www.sparkfun.com/products/99

## SALEAE Logic device
Digital logic analyzer connects to host computer via USB.  
Multi-platform Windows, OS X, Linux.  
http://www.saleae.com/logic
Dan and others at the meeting say its great!  
$150  
16 channel has memory and can buffer input that runs at higher frequency than USB bus to host computer.
Some competitor products may have low speed analog input to act as an oscilloscope.  

# Equipment
## Hardware
Arduino Duemilanove w/ ATmega328 (Duemilanove="2009")

### RTC Module
Vetco Real Time Clock Module (DS1307) for Arduino  
Part Number: VUPN5978

## Software library
https://github.com/adafruit/RTClib  
Put in ~/Documents/Arduino/libraries/RTClib

# Results
Copied file from library RTClib/examples/ds1307/ds1307.pde

## Hardware Connect
The clock module runs at 5 V.

### WARNING: CONNECTING A SECOND DEVICE TO THE I2C BUS RUNNING AT 3V COULD DAMAGE THE 3V DEVICE.
http://learn.adafruit.com/tsl2561/wiring  
paraphrasing...  
"As long as all the sensors/device on the i2c bus are running the same voltage, we're fine.  
However, don't use a 3.3V device like the TSL2561 at the same time as a 5.0v i2c device (like the DS1307) with pullups.

| Sensor | Arduino Duemilanove Pin | Arduino Leonardo Pin | Arduino Due Pin  |
| ------ | ----------------------- | -------------------- | ---------------- |
| VCC    | 5V                      | 5V                   | 5V               |
| GND    | GND                     | GND                  | GND              |
| SDA    | A4                      | D2                   | D20 or SDA1      |
| SCL    | A5                      | D3                   | D21 or SCL1      |

http://arduino.cc/en/Reference/Wire

### I2C info
#### Pull up resistors
Both SCL and SDA should be connected through their own pull up resistors to +Vcc.  
R approx 10 kohm, good for battery powered projects.  
R approx 1 kohm?, good for highest speed projects.  
Higher resistance uses less power, but has larger RC time constant and lower bandwidth.  
Arduino wiring library may be able to configure Arduino to use internal pull up resistors.  
The Adafruit example programs work without separate pull up resistors.  

#### ACK
Master sends data, slave sends ack acknowledge or nack (no acknowledge).  
ack is low, nack is high.  

Quick and dirty hack hobby program may not bother to check for ack and handle error.  
However, this may not be very reliable.  

#### Address
Two types of address.  
Bus address, one per slave.  
Device register addresses, may be multiple per slave.  
Also be careful sometimes an address may be written in different bases e.g. 10 or hexadecimal.  
I2C implementations vary. Address may need to be shifted!  
See "Major Gotcha Alert" in "I2C Devices and Arduino" by Dan Tebbs.  
