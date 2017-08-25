Mambo RS Edu bluetooth networking HOWTO
=======================================

> Warning/disclaimer: this HOWTO requires a Parrot Mambo flashed with the custom firmware
residing in this directory.


Step 1: make sure all necessary packages are installed
------------------------------------------------------

You need to install a bluetooth manager and a network manager.
In this document, we use `blueman` and `NetworkManager` as an example.
On a recent debian distribution, this should install the required packages:

```
$ sudo apt-get install blueman network-manager
```

Step 2: scan and locate your Mambo device
-----------------------------------------

Launch the Bluetooth manager (assuming you have sufficient permission to do this):

```
$ blueman-manager
```

After clicking on *Search*, you should see at least a line `Mambo_XXXXXX` matching your device.

> Note: you may sometimes need to retry this step several times or restart blueman-manager before
your device is successfully listed.

Make sure all stale Mambo pairings are removed.
If you reflashed or performed a factory reset on your Mambo, you should remove
old pairings from your PC; to do this, right-click on the device line and click *Remove*.

Step 3: pair with your Mambo device
-----------------------------------

First, you need to locate the correct `Mambo_XXXXXX` line in the list of scanned devices.
Its device type should be *Doll* (this is probably a bug in blueman-manager, as it should instead say *Toy Vehicle*).
Be careful, as another similar Bluetooth Low Energy `Mambo_XXXXXX` line also appears in the list (without *Doll* as device type).

![Screenshot1][sc01]

Once you have found the correct line, just right-click and select *Pair* in the drop-down menu.
After a few seconds, the Bluetooth manager should briefly indicate that you are connected to the device
(notice the orange, green and blue icons on the right-end of the line), then disconnect; this is normal.

![Screenshot2][sc02]

> Note: this pairing needs to be done only once, and will remain valid until you update or perform a factory
reset on your Mambo, or remove the pairing in Bluetooth Manager.

Step 4: activate the network connection
---------------------------------------

Launch a Network Manager controlling interface:

```
$ nmtui
```

![Screenshot3][sc03]

This should show a `Mambo_XXXXXX` connection; select *Activate* to connect.

Step 5: verify connection
-------------------------

Typing the following command should show a `bnep0` interface with IP address 192.168.3.2.

```
$ ifconfig

bnep0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.3.2  netmask 255.255.255.0  broadcast 192.168.3.255
        inet6 fe80::f63e:ab2b:95b7:ff94  prefixlen 64  scopeid 0x20<link>
        ether 5c:e0:c5:af:50:a9  txqueuelen 1000  (Ethernet)
        RX packets 4  bytes 1000 (1000.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 34  bytes 4559 (4.4 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1  (Local Loopback)
        RX packets 1434  bytes 131615 (128.5 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1434  bytes 131615 (128.5 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```

You can now open a shell on the drone:

```
$ telnet 192.168.3.1
```

**You're all set, happy hacking !**

[sc01]: https://github.com/Parrot-Developers/MinidronesEdu/images/mambo_rsedu_01.png "Mambo device with type Doll"
[sc02]: https://github.com/Parrot-Developers/MinidronesEdu/images/mambo_rsedu_02.png "Paired Mambo device"
[sc03]: https://github.com/Parrot-Developers/MinidronesEdu/images/mambo_rsedu_03.png "NetworkManager text control interface"
