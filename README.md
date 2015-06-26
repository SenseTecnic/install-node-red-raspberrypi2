This script provides routine commands to install and configure a default installation of node red in your Raspberry Pi.

** This script is based on instructions found at http://nodered.org/docs/hardware/raspberrypi.html **

## CHANGELOG
+ 2015-02-18 First Version.
+ 2015-02-19 Improved instructions and checked for previous installations.
+ 2015-06-25 Update for Raspbery Pi 2 (Raspbian 3.18.11-v7+)


## Nodes Installed

Currently this script will install the following nodes:

+ node-red-contrib-wotkit
+ node-red-node-web-nodes
+ node-red-node-web-nodes
+ node-red-node-pushbullet
+ node-red-node-wordpos
+ node-red-node-xmpp
+ node-red-node-badwords
+ node-red-node-suncalc
+ node-red-node-smooth
+ node-red-node-ping
+ node-red-contrib-moment
+ node-red-node-fitbit
+ node-red-contrib-slack

If you want to add your own please send a pull request or post an issue.

## STEP-BY-STEP-INSTRUCTIONS

In a console in your Raspberry Pi issue:

```
cd ~
curl -sL https://raw.githubusercontent.com/SenseTecnic/install-node-red-pi2/master/install-node-red.sh | sudo bash
```

If you are in need of a shorter version you can use:

```
curl -sL http://bit.ly/node-red-pi2 | sudo bash
```

After you're finished you can find your ip address with:

```
hostname -I
```

Then, visit http:://your-rpi-address:1880/, you can then go to "menu > import > clipboard" and import the following nodes. They are a blinking demo, demonstrating also how to read from a digital input.

```
[{"id":"2462d9f5.db9d26","type":"function","name":"Toggle 0/1 on input","func":"\ncontext.state = context.state || 0;\n\n(context.state == 0) ? context.state = 1 : context.state = 0;\nmsg.payload = context.state;\n\nreturn msg;","outputs":1,"x":362,"y":76,"z":"2fd1a8b8.d02e58","wires":[["f7fad3c0.08053"]]},{"id":"47794a9.fb886b4","type":"debug","name":"","active":true,"x":341,"y":146.00002098083496,"z":"2fd1a8b8.d02e58","wires":[]},{"id":"bc3c8444.43c378","type":"inject","name":"tick every 3 secs","topic":"","payload":"","payloadType":"date","repeat":"3","crontab":"","once":false,"x":160,"y":76.00002098083496,"z":"2fd1a8b8.d02e58","wires":[["2462d9f5.db9d26"]]},{"id":"f7fad3c0.08053","type":"rpi-gpio out","name":"","pin":"12","set":false,"out":"out","x":533.8333740234375,"y":75.83333396911621,"z":"2fd1a8b8.d02e58","wires":[]},{"id":"8035245c.7fcad8","type":"rpi-gpio in","name":"","pin":"16","intype":"tri","read":true,"x":182,"y":145.8333339691162,"z":"2fd1a8b8.d02e58","wires":[["47794a9.fb886b4"]]}]
```

To wire your Raspberry you can use a breakout and cable like the Pi Cobbler sold by adafruit http://www.adafruit.com/products/914. In our example nodes above every 3 seconds the resistor of pin 12 is changed (pullup, pulldown), simultaneously we read pin 16 and output the value to the console.

For demo purposes we wire a resistor (330 ohms) to pin pin 12, wire pin 12 to pin 16 (to read when it changes) and connect an LED to the resistor and the 3V output. Like so:

![alt tag](https://raw.github.com/SenseTecnic/install-node-red-raspberrypi2/master/blinkwiring.jpg)

You can learn more about the Raspbery Pi's GPIO pins here: [http://pi.gadgetoid.com/pinout](http://pi.gadgetoid.com/pinout)


## ASSUMPTIONS

I am assuming you have raspbian equal or greater than 3.18.11-v7, that is when you issue the command "uname -r" you get

```
$ uname -r
'3.18.11-v7'
```

This script will work with an installation via NOOBS => 1.4.1 [http://www.raspberrypi.org/downloads/](http://www.raspberrypi.org/downloads/)

## LIMITATIONS

 + This script will only work for Raspberry Pi v2
 + This is intended as an initial setup. There are important security flaws, for example the /etc/init.d/node-red file is using the root user making it insecure for production.
 + This script was tested with a clean install of raspbian NOOBS 1.4.1
