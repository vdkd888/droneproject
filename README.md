# Developing a Collision Avoidance System for the Parrot AR Drone 2.0 to Navigate Autonomously Indoors
### By [Viraj Doshi](https://uk.linkedin.com/in/virajdoshi)
#### I decided to modify/hack the Parrot AR Drone 2.0 to enable it to fly autonomously in an indoor environment and avoid collisions in real-time. There are few projects out there that use monocular methods using the drone camera to detect objects. This project differs from others as it uses sensors (ultrasonic) to successfully detect objects and navigate around them, kinda like a BAT! So I basically enabled the data from an ardunio to be understood by the drone and, in turn, execute API functions from it (Yay!). I decided to make it Open Source so that anyone can build it and hopefully improve the system!

Extending the capabilities and functionalities of Drones is the next frontier to explore in the new era of aerial vehicle technology. The following portrays the prototype development of the Parrot AR Drone framework to create a spatially aware Drone that has the ability to autonomously navigate in indoor environments and avoid collisions in real-time. Unlike related works in this field, the system did not require vision based guidance firmware or a network of powerful host computers for data processing. The Drone had the ability to autonomously create a flightpath and avoid obstacles in a closed-loop system. Depicted through demonstrations, the project concludes that by leveraging the Parrot AR Drone client API through Node.js, designing an Ultrasonic System and understanding the Drone’s Serial port architecture, it was possible to create a Drone capable of computing spatial data to avoid obstacles and navigate indoors autonomously.

### Final Drone System
![](https://drive.google.com/file/d/0ByAVenVVIJ0CUy1mbkpVd21GYzg/view?usp=sharing)
## Video Demonstration: Youtube links and Gifs

- [Take/off land](https://www.youtube.com/watch?v=tnUtOJ1HANE)

![](http://share.gifyoutube.com/y0OE6G.gif)

- [Fly forward land](https://www.youtube.com/watch?v=ha_LJ0GN5-8)

![](http://share.gifyoutube.com/KY9ka1.gif)

- [Reaction to hand](https://www.youtube.com/watch?v=DDz88Plf-ZM)

![](http://share.gifyoutube.com/mLZk6p.gif)

- [Fly around object 1](https://www.youtube.com/watch?v=0trEX6bGvBs)

![](http://share.gifyoutube.com/KBlaD6.gif)

- [Fly around object 2](https://www.youtube.com/watch?v=jnLXxSN--ww)

![](http://share.gifyoutube.com/KRGk38.gif)

- [Fly in corridor](https://www.youtube.com/watch?v=5PRqsZNZKbs)

![](http://share.gifyoutube.com/vMLkgl.gif)

- [Fly in corridor around manikin](https://www.youtube.com/watch?v=fN-N3yeGaZU)

![](http://share.gifyoutube.com/Kr0pbA.gif)

- [Fly around me](https://www.youtube.com/watch?v=IYlJpIIJdXU)

![](http://share.gifyoutube.com/yAb9lg.gif)

### Contents

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
- Make it Happen
- Known Issues
- MIT License Copyright (c)

### How the System works
![](http://s21.postimg.org/9788e9w9j/Sys_Con.png)
### System Overview
![](http://s3.postimg.org/6gyjf1orn/Sys_Over.png)
### Flightpath Configurations
![](http://s11.postimg.org/yhhlyfnr7/Flightpath_Config.png)

### Project Status

Currently in Prototype form/development so it kinda works (see videos), BUT use at your own risk

### Hardware

- [Parrot AR Drone 2.0](http://ardrone2.parrot.com/)
- Ardunio Uno
- At least 4 HC-SR04 Ultraonsic Sensors: [Datasheet](http://www.micropik.com/PDF/HCSR04.pdf)
- Lots of Wire connectors, the breadboard kind
- Level Shifter [Mirumod](http://people.eecs.ku.edu/~jpince/Project%20Files/Serial%20Port%20&%20Power%20wiring%20diagram.pdf)

### Ultrasonic System

- Works just like a [simple range finder](http://www.instructables.com/id/Ultrasonic-Range-detector-using-Arduino-and-the-SR/)
- The sensors calculate the distances of objects around the drone in the their respective directions, but instead of a distance output, it prints the direction the drone should travel 
- Diagram of how the system is connected together
![](http://s10.postimg.org/xb4nhknxl/Ultra_Sys.png)
- The Rx on the Ardunio is connected to the Tx on the serial port of the drone motherboard
![](http://s29.postimg.org/csb3b3ch3/Droneserial.png)

## Ultrasonic System Software

The Sketch once uploaded onto the Ardunio Prints the direction the drone will travel or functions it completes depending on the spatial environment (Forward F, Right R, Left L, Stop S etc.)

![](http://s8.postimg.org/yg0ox6c05/flowchart1.png)
![](http://s30.postimg.org/4pr2p91td/table.png)

There are two sketches available:
`Sketch 1`: Continuously prints data to the drone
`Sketch 2`: Only prints new data if it differs from the current data (This method takes up less memory)
Note: You should upload a sketch to just the ultrasonic system (not connected to the drone) and block the sensors to see what the data output is. This way you can better understand how it works and test it

### Command Script

This defines the drone functions using the API and tells the drone where to navigate to depending on the data received from the ultrasonic system

![](http://s9.postimg.org/eh0xldxsv/flowchart2.png)

### Configurations

- Sensors positioned Front, Left, Right and on the Top
- Blocking the Top sensor triggers the drone to land
- Blocking the Front sensor less than 50cm will make it hover in its current position
- More sensor and distance configurations can be modified to change drone functions

### Getting Software onto Drone

- Get node on the drone (This method was identified by [Maxodgen](https://gist.github.com/maxogden/4152815))
- download node and node-serialport from https://github.com/felixge/node-cross-compiler/downloads
- untar node-serialport and put node and the node-serialport folder onto a usb
- put the usb into the drone
- telnet onto drone, `telnet 192.168.1.1` (use putty for windows)
- `cd /data/video`
- `cp usb/node .` (it might be usb1)
- `chmod +x node`
- `mkdir node_modules`
- `cp -r usb/node-serialport/ node_modules/`
- Copy the [ar-drone library](https://github.com/felixge/node-ar-drone) by [felixge](https://github.com/felixge/node-ar-drone) and paste the folder in `cd data/video` the same way you did node

## Make It Happen

- Upload a Sketch onto the arduino (`Sketch 1` or `Sketch 2` from this gist)
- Turn ON drone and wait till all LEDS are GREEN
- Then connect the 5V to the ardunio and connect up the Rx to the Tx on the drone
- Connect to Drone via telnet, `telnet 192.168.1.1`
- `cd /data/video`
- Execute node `./node`
- Once running, paste the `Command Script` from this gist into the window and hit enter!
- Now watch your drone autonomously fly and avoid obstacles MUHAHAHA!

### Known Issues (YES, IT IS BUGGY)

- Memory leak error (currently no fix, soz)
- Drone going crazy (restart everything/ Drone ON/OFF seems to work)
- Use fully charged battery (Will not work if charge is less than 40%)
- Restart system after each flight mission

## MIT License Copyright (c)

Copyright (c) 2015 [Viraj Doshi](https://uk.linkedin.com/in/virajdoshi) (@vdkd888)

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
