---
layout: post
category : lessons
tagline: "Supporting tagline"
tags : [intro, beginner, jekyll, tutorial]
---
{% include JB/setup %}

If you've been involved with IT in any capacity in recent years, you've
probably heard the term "Internet of Things," or IoT. According to
Gartner, IoT is at the [top of the hype
cycle](http://www.gartner.com/technology/research/hype-cycles/), meaning
a lot of people are excited about it, but not much real development is
happening yet. While less than a billion devices were connected to the
Internet in 2009, [Gartner
predicts](http://www.gartner.com/newsroom/id/2636073%3D) that there will
be 26 billion IoT devices installed in 2020, generating \$300 billion in
revenue for manufacturers and service providers and making a \$1.9
trillion impact on the global economy.

In a nutshell, IoT is about using smart devices to collect data that is
transmitted via the Internet to other devices. It's closely related to
machine-to-machine (M2M) technology. While the concept had been around
for some time, the term "Internet of Things" was first used in 1999 by
Kevin Ashton, who was a Procter & Gamble employee at the time.

Since then, the idea has spread rapidly and widely. A [survey conducted
by
ARM](http://www.arm.com/files/pdf/EIU_Internet_Business_Index_WEB.PDF)
found that more than 75 percent of enterprises are either already using
IoT in some capacity or exploring ways to do so. And 96 percent of those
surveyed expected to be using IoT by 2016.

Part of the reason for the great interest in IoT is the potential it
offers. In a [2006
article](http://www.rfidjournal.com/articles/view?4986) Ashton
explained, "If we had computers that knew everything there was to know
about things—using data they gathered without any help from us—we would
be able to track and count everything, and greatly reduce waste, loss
and cost. We would know when things needed replacing, repairing or
recalling, and whether they were fresh or past their best." He
concluded, "The Internet of Things has the potential to change the
world, just as the Internet did. Maybe even more so."

Much of the early work on IoT technology and standards has taken place
within the open source community. This month we're featuring some of the
more interesting open source IoT projects currently in active
development. While our open source lists generally focus on software,
this list also features an array of open source hardware, many of which
are available for hobbyists to purchase at low prices.

As always, if you know of projects that you think should be on our list,
feel free to note them in the comments section below.

{{{more}}}

Development Tools
=================

1.  [Arduino](http://www.arduino.cc/)

Arduino is both a hardware specification for interactive electronics and
a set of software that includes an IDE and the Arduino programming
language. The website explains that Arduino is "a tool for making
computers than can sense and control more of the physical world than
your desktop computer." The organization behind it offers a variety of
boards, starter kits, robots and related products for sale, and many
other groups have used Arduino to build IoT-related hardware and
software products of their own.

1.  [Eclipse IoT Project](http://iot.eclipse.org/)

Eclipse is sponsoring several different projects surrounding IoT. They
include application frameworks and services; open source implementations
of IoT protocols, including MQTT CoAP, OMA-DM and OMA LWM2M; and tools
for working with Lua, which Eclipse is promoting as an ideal IoT
programming language. Eclipse-related projects include Mihini, Koneki
and Paho. The website also includes sandbox environments for
experimenting with the tools and a live demo.

1.  [Kinoma](http://www.marvell.com/kinoma/)

Owned by Marvell, the Kinoma software platform encompasses three
different open source projects. Kimona Create is a DIY construction kit
for prototyping electronic devices. Kimona Studio is the development
environment that works with Create and the Kinoma Platform Runtime.
Kimona Connect is a free iOS and Android app that links smartphones and
tables with IoT devices.

1.  [M2MLabs Mainspring](http://www.m2mlabs.com/framework)

Designed for building remote monitoring, fleet management and smart grid
applications, Mainspring is an open source framework for developing M2M
applications. It capabilities include flexible modeling of devices,
device configuration, communication between devices and applications,
validation and normalization of data, long-term data storage, and data
retrieval functions. It's based on Java and the Apache Cassandra NoSQL
database.

1.  [Node-RED](http://nodered.org/)

Built on Node.js, Node-RED describes itself as "a visual tool for wiring
the Internet of Things." It allows developers to connect devices,
services and APIs together using a browser-based flow editor. It can run
on Raspberry Pi, and more than 60,000 modules are available to extend
its capabilities.

Hardware
========

1.  [Arduino
    Yún](http://arduino.cc/en/Main/ArduinoBoardYun?from%3DMain.ArduinoYUN)

This microcontroller combines the ease of an Arduino-based board with
Linux. It includes two processors—the ATmega32u4 (which supports
Arduino) and the Atheros AR9331 (which runs Linux). Other features
include Wi-Fi, Ethenet support, a USB port, micro-SD card slot, three
reset buttons and more. They are available for purchase from the Arduino
website.

1.  [BeagleBoard](http://beagleboard.org/)

BeagleBoard offers credit-card sized computers that can run Android and
Linux. Because they have very low power requirements, they're a good
option for IoT devices. Both the hardware designs and the software they
run are open source, and BeagleBoard hardware (often sold under the name
BeagleBone) is available through a wide variety of distributors.

1.  [Flutter](http://www.flutterwireless.com/)

Flutter's claim to fame is its long range. This Arduino-based board has
a wireless transmitter that can reach more than a half mile. Plus, you
don't need a router; flutter boards can communicate with each other
directly. It includes 256-bit AES encryption, and it's easy to use. Both
the hardware and the software are completely open source, and the price
for a basic board is just \$20.

1.  [Local Motors Connected
    Car](https://localmotors.com/awest/connected-car-project-internet-of-things/)

Local Motors is a car company that manufactures open source car designs
on a small scale. They collaborated with IBM on an IoT-connected vehicle
that they showed off at a conference last spring. Much of the open
source software and design specifications for the prototype are
available for download from the link above.

1.  [Microduino](http://www.microduino.cc/)

As you might guess from its name, Microduino offers really small boards
that are compatible with Arduino. In fact, these boards are about the
size of a quarter and can be stacked together to create new things. All
the hardware designs are open source, and core modules start at just \$8
each. It was funded by a Kickstarter campaign that raised \$134,563.

1.  [OpenPicus](http://www.openpicus.com/)

This company offers a line of programmable modules and kits for
connecting devices to the cloud and the Internet of Things. Its platform
and hardware are open source, but its products can be used to create
closed source commercial products. The company also offers its
development services for hire.

1.  [Pinoccio](https://pinocc.io/)

Arduino-compatible Pinnoccio boards (which the company calls "Scouts")
connect to each other in a low-power mesh network. They include a
built-in rechargeable battery that can connect to solar panels or any
USB power supply. The organization also offers Pinoccio HQ, a GUI for
monitoring the activities of the scouts, and ScoutScript, an easy-to-use
scripting language for controlling the devices. A starter kit costs
\$197.

1.  [RasWIK](http://shop.ciseco.co.uk/raswik/)

Made by a company called Ciseco, RasWIK is short for the Raspberry Pi
Wireless Inventors Kit. It allows anyone with a Raspberry Pi to
experiment with building their own Wi-Fi-connected devices. It includes
documentation for 29 different projects or you can come up with one of
your own. There is a fee for the devices, but all of the included code
is open source, and you can use it to build commercial products if you
choose.

1.  [SODAQ](http://www.sodaq.net/)

Short for "Solar-Powered Data Acquistion," SADAQ offers
Arduino-compatible boards with Lego-like plug-in modules. The website
includes a number of tutorials, making it a suitable for beginners. And
the solar panel makes it a good choice for logging environmental data in
various locations where power and Internet connections might not be
available. A basic board starts at \$39.

1.  [Tessel](https://tessel.io/)

Tessel aims to make hardware development easier for software developers
with this JavaScript-enabled microcontroller that plugs into any USB
port. You can also connect it to additional modules to add
accelerometer, ambient light and sound, camera, Bluetooth, GPS and/or
nine other capabilities. One board and a module starts at \$99 with
additional modules available for \$25. All the software and hardware
designs are fully open source.

1.  [UDOO](http://www.udoo.org/)

This Arduino-compatible board can also run Android or Linux (a
distribution called UDOObuntu) from its second processor. It boasts that
it is four times as powerful as a Raspberry Pi. Multiple tutorials and
projects are available on the website, and it also offers a "Made by
UDOOers" section where people can show off their creations. Prices start
at \$99 for a basic board.

Home Automation Software
========================

1.  [OpenHAB](http://www.openhab.org/)

OpenHAB lets the smart devices you already have in your home talk to one
another. It's vendor and hardware-neutral, running on any Java-enabled
system. One of its goals is to allow users to add new features to their
devices and combine them in new ways. It's won several awards, and it
has a companion cloud computing service called my.openHAB.

1.  [The Thing System](http://thethingsystem.com/index.html)

This project includes both software components and network protocols. It
promises to find all the Internet-connected things in your house and
bring them together so that you can control them. It supports a long
list of devices, including Nest thermostats, Samsung Smart Air
Conditioners, Insteon LED Bulbs, Roku, Google Chromecast, Pebble
smartwatches, Goji smart locks and much more. It's written in Node.js
and can fit on a Raspberry Pi.

Middleware
==========

1.  [IoTSyS](https://code.google.com/p/iotsys/)

This IoT middleware provides a communication stack for smart devices. It
supports multiple standards and protocols, including IPv6, oBIX,
6LoWPAN, Constrained Application Protocol and Efficient XML Interchange.
Several videos on the website show how it works in action.

1.  [OpenIoT](http://openiot.eu/)

The OpenIoT website explains that the project is "an open source
middleware for getting information from sensor clouds, without worrying
what exact sensors are used." It aims to enable cloud-based "sensing as
a service," and has developed use cases for smart agriculture,
intelligent manufacturing, urban crowdsensing, smart living and smart
campuses. Its backers include Athens Information Technology (AIT), École
Polytechnique Fédérale de Lausanne (EPFL), the Fraunhofer Institute for
Optronics, System Technology and Image Exploitation IOSB, SENSAP
Microsystems AE, AcrossLimits, the Commonwealth Scientific and
Industrial Research Organisation (CSIRO), the University of Zagreb
Faculty of Electrical Engineering and Computing, and the National
University of Ireland, Galway.

Operating Systems
=================

1.  [AllJoyn](https://allseenalliance.org/developer-resources/alljoyn-open-source-project)

Originally created by Qualcomm, this open source operating system for
the Internet of Things is now sponsored by one of the most prominent IoT
organizations—The AllSeen Alliance, whose members include the Linux
Foundation, Microsoft, LG, Qualcomm, Sharp, Panasonic, Cisco, Symantec
and many others. It includes a framework and a set of services that will
allow manufacturers to create compatible devices. It's cross-platform
with APIs available for Android, iOS, OS X, Linux and Windows 7.

1.  [Contiki](http://www.contiki-os.org/)

Contiki describes itself as "the open source OS for the Internet of
Things." It connects low-power microcontrollers to the internet and
supports standards like IPv6, 6lowpan, RPL and CoAP. Other key features
include highly efficient memory allocation, full IP networking, very low
power consumption, dynamic module loading and more. Supported hardware
platforms include Redwire Econotags, Zolertia z1 motes, ST
Microelectronics development kits and Texas Instruments chips and
boards. Paid commercial support is available.

1.  [Raspbian](http://raspbian.org/)

While the Raspberry Pi was intended as an educational device, many
developers have begun using this credit-card-sized computer for IoT
projects. The complete hardware specification is not open source, but
much of the software and documentation is. Raspbian is a popular
Raspberry Pi operating system that is based on the Debian distribution
of Linux.

1.  [RIOT](http://riot-os.org/)

RIOT bills itself as "the friendly operating system for the Internet of
Things." Forked from the FeuerWhere project, RIOT debuted in 2013. It
aims to be both developer- and resource-friendly. It supports multiple
architectures, including MSP430, ARM7, Cortex-M0, Cortex-M3, Cortex-M4,
and standard x86 PCs.

1.  [Spark](https://www.spark.io/)

Spark is a distributed, cloud-based IoT operating system. The same
company also offers easy-to-use hardware development kits and related
products that start at just \$39 (and the hardware designs are also open
source). It includes a Web-based IDE, a command-line interface, support
for multiple languages, and libraries for working with many different
IoT devices. It has a very active user community, and a lot of
documentation and online help are available.

Monitoring
==========

1.  [Freeboard](http://freeboard.io/)

Freeboard aims to let users create their own dashboards for monitoring
IoT deployments. The code is freely available on GitHub or you can try
the service for free if you make your dashboard public. Low-priced plans
are also available for those who want to keep their data private. Sample
dashboards oon the site show how they can be used to track air quality,
residential appliances, distillery performance or environmental
conditions in a humidor.

Printing
========

1.  [Exciting Printer](http://exciting.io/printer/)

Exciting offers an open source kit for experimenting with IoT printing.
It makes it possible to build your own small printer and use that
printer to print out information obtained from various IoT devices. For
example, it could print out a list of daily reminders, the weather
report, etc. And in a interesting twist, if you want to contact the
project owners, you can draw a picture that will be printed on the IoT
printer in their office.

Platforms and Integration Tools
===============================

1.  [DeviceHive](http://www.devicehive.com/)

This project offers a machine-to-machine (M2M) communication framework
for connecting devices to the Internet of Things. It includes
easy-to-use Web-based management software for creating networks,
applying security rules and monitoring devices. The website offers
sample projects built with DeviceHub, and it also has a "playground"
section that allows users to use DeviceHub online to see how it works.

1.  [Devicehub.net](http://devicehub.net/dev/api#summary)

Devicehub.net describes itself as "the open source backbone for the
Internet of Things." It's a cloud-based service that stores IoT-related
data, provides visualizations of that data and allows users to control
IoT devices from a Web page. Developers have used the service to create
apps that track health information, monitor the location of children,
automate household appliances, track vehicle data, monitor the weather
and more.

1.  [IoT Toolkit](http://iot-toolkit.com/)

The group behind this project is working on a variety of tools for
integrating multiple IoT-related sensor networks and protocols. The
primary project is a Smart Object API, but the group is also working on
an HTTP-to-CoAP Semantic mappin , an application framework with embedded
software agents and more. They also sponsr a meetup group in Silicon
Valley for people who are interested in IoT development.

1.  [Mango](http://forum.infiniteautomation.com/)

Mango bills itself as "the world's most popular open source
Machine-to-Machin (M2M) software." Web-based, it supports multiple
platforms. Key features include support for multiple protocols and
databases, meta points, user-defined events, import/export and more.

1.  [Nimbits](http://www.nimbits.com/index.jsp)

Nimbits can store and process a specific type of data—data that has been
time- or geo-stamped. A public platform as a service is available, or
you can download the software and deploy it on Google App Engine, any
J2EE server on Amazon EC2 or on a Raspberry Pi. It supports multiple
programming languages, including Arduino, JavaScript, HTML or the
Nimbits.io Java library.

1.  [OpenRemote](http://www.openremote.com/)

OpenRemote offers four different integration tools for home-based
hobbyists, integrators, distributors, and manufacturers. It supports
dozens of different existing protocols, allowing users to create nearly
any kind of smart device they can imagine and control it using any
device that supports Java. The platform is open source, but the company
also sells a wide variety of support, ebooks and other tools to aid in
the design and product development process.

1.  [SiteWhere](http://www.sitewhere.org/)

This project provides a complete platform for managing IoT devices,
gathering data and integrating that data with external systems.
SiteWhere releases can be downloaded or used on Amazon's cloud. It also
integrates with multiple big data tools, including MongoDB and
ApacheHBase.

1.  [ThingSpeak](https://thingspeak.com/)

ThingSpeak can process HTTP requests and store and process data. Key
features of the open data platform include an open API, real-time data
collection, geolocation data, data processing and visualizations, device
status messages and plugins. It can integrate multiple hardware and
software platforms including Arduino, Raspberry Pi,
ioBridge/RealTime.io, Electric Imp, mobile and Web applications, social
networks and MATLAB data analytics. In addition to the open source
version, a hosted service is also available.

Reference
=========

[35 open source tools for the IOT](http://www.datamation.com/open-source/35-open-source-tools-for-the-internet-of-things-1.html)
--------------------------------------------------------------------------------------------------------------------------------
