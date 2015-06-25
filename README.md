# Developing a Collision Avoidance System for the Parrot AR Drone 2.0 to Navigate Autonomously Indoors

- I decided to modify/hack the Parrot AR Drone 2.0 to enable it to fly autonomously in an indoor environment and avoid collisions in real time
- There are few projects out there that use monocular methods using the drone camera to detect objects
- This project differs from others as it uses sensors (ultrasonic) to successfully detect objects and navigate around them, kinda like at BAT!
- I decided to make it Open Source so that anyone can build it and hopefully improve the system!

### Picture of the Drone System
![](http://s1.postimg.org/ifhv0i2m7/finalproject.png)

## Video Links

- [Take/off land](https://www.youtube.com/watch?v=tnUtOJ1HANE)
- [Fly forward land](https://www.youtube.com/watch?v=ha_LJ0GN5-8)
- [Reaction to hand](https://www.youtube.com/watch?v=DDz88Plf-ZM)
- [Fly around object 1](https://www.youtube.com/watch?v=0trEX6bGvBs)
- [Fly around object 2](https://www.youtube.com/watch?v=jnLXxSN--ww)
- [Fly in corridor](https://www.youtube.com/watch?v=5PRqsZNZKbs)
- [Fly in corridor around manikin](https://www.youtube.com/watch?v=fN-N3yeGaZU)
- [Fly around me](https://www.youtube.com/watch?v=IYlJpIIJdXU)

## Table of Contents

- How the System works
- Systems Overview
- Flightpath configuration
- Project Status
- Hardware
- Ultrasonic System
- Ultrasonic System Software
- Command Script
- Configurations
- Getting Software onto the drone
- Make it happen
- Know Issues
- MIT License Copyright (c)

## How the System works
![](http://s21.postimg.org/9788e9w9j/Sys_Con.png)
## System Overview
![](http://s3.postimg.org/6gyjf1orn/Sys_Over.png)
## Flightpath Configurations
![](http://s2.postimg.org/hx1ve4a2x/Flightpath_Config.png)

## Project Status

- Currently in Prototype form/development so it kinda works (see videos below), BUT use at your own risk

## Hardware

(Stuff you need)
- [Parrot AR Drone 2.0](http://ardrone2.parrot.com/)
- Ardunio Uno
- At least 4 HC-SR04 Ultraonsic Sensors: [Datasheet](http://www.micropik.com/PDF/HCSR04.pdf)
- Lots of Wire connectors, the breadboard kind
- Level Shifter [Mirumod](http://people.eecs.ku.edu/~jpince/Project%20Files/Serial%20Port%20&%20Power%20wiring%20diagram.pdf)

## Ultrasonic System

- Works just like a [simple range finder](http://www.instructables.com/id/Ultrasonic-Range-detector-using-Arduino-and-the-SR/)
- The sensors calculate the distances of objects around the drone in the their respective directions, but instead of a distance output, it prints the direction the drone should travel 
- Diagram of how the system is connected together
![](http://s10.postimg.org/xb4nhknxl/Ultra_Sys.png)
- The Rx on the Ardunio is connected to the Tx on the serial port of the drone motherboard
![](http://s29.postimg.org/csb3b3ch3/Droneserial.png)

## Ultrasonic System Software

- The Sketch once uploaded onto the Ardunio Prints the direction the drone will travel or functions it completes depending on the spatial environment (Forward F, Right R, Left L, Stop S etc.)
![](http://s12.postimg.org/wjl1uppx9/flowchart1.png)
![](http://s30.postimg.org/4pr2p91td/table.png)
- There are two sketches availble:
- Sketch 1: Continuously prints data to the drone
- Sketch 2: Only prints new data if it differs from the current data (This method takes up less memory)
- Note: You should upload a sketch to just the ultrasonic system (not connected to the drone) and block the sensors to see what the data output is. This way you can better understand how it works and test it

## Command Script
- This defines the drone functions using the API and tells the drone where to navigate to depending on the data received from the ultrasonic system
![](http://s24.postimg.org/8wnclu01x/flowchart2.png)

## Configurations
- Sensors positioned Front, Left, Right and on the Top
- Blocking the Top sensor triggers the drone to land
- Blocking the Front sensor less than 50cm will make it hover in its current position
- More sensor and distance configurations can be modified to change drone functions

## Getting Software onto Drone

- Get node on the drone (This method was identified by [Maxodgen](https://gist.github.com/maxogden/4152815))
- download node and node-serialport from https://github.com/felixge/node-cross-compiler/downloads
- untar node-serialport and put node and the node-serialport folder onto a usb thumbstick thingy
- put the thumbstick thingy into the drone (like in the above photo)
- telnet into drone, `telnet 192.168.1.1`
- `cd /data/video`
- `cp usb/node .` (it might be usb1)
- `chmod +x node`
- `mkdir node_modules`
- `cp -r usb/node-serialport/ node_modules/`

- Copy the [ar-drone library](https://github.com/felixge/node-ar-drone) by [felixge](https://github.com/felixge/node-ar-drone) and paste the folder in `cd data/video` the same way you did node

## Make It Happen

- Upload a Sketch onto the ardunio (`Sketch 1` or `Sketch 2` from this Jist)
- Turn ON drone and wait till all LEDS are GREEN
- Then connect the 5V to the ardunio and connect up the Rx to the Tx on the drone
- Connect to Drone via telnet, `telnet 192.168.1.1`
- `cd /data/video`
- Execute node `./node`
- Once running, paste the `Command Script` from this gist into the window and hit enter!
- Now watch your drone autonomously fly and avoid obstacles MUHAHAHA!

## Known Issues (YES, IT IS BUGGY)

- Memory leak error (currently no fix, soz)
- Drone going crazy (restart everything/ Drone ON/OFF seems to work)
- Use fully charged battery (Will not work is charge is less than 40%)
- Restart system after each flight mission

## MIT License Copyright (c)

Copyright (c) 2015 [Viraj Doshi](https://uk.linkedin.com/in/virajdoshi
) (@vdkd888)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:
The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
