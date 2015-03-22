---
title: "Testing on multiple devices with pow and xip.io"
---

[xip.io](http://xip.io) is a smart domain name service that provides wildcard DNS for any IP address, so for example 192.168.0.11.xip.io resolves to 192.168.0.11. It can be used for testing your apps and websites on multiple devices within a local network, including mobile devices and VMs. [Pow](http://pow.cx) supports it out of the box, so if you normally access your website with myapp.dev domain on your local box, you can also reach this with myapp.192.168.0.11.xip.io (where 192.168.0.11 is IP of your local box). Simple like that. Very very convenient.
