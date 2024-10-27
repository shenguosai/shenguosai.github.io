---
title: ZeroTier Documention
date: 2024-03-06 10:09:35
tags: Zerotier
categories: 
- [技术兴趣,网络服务]
---

[**ZeroTier Documentation**](https://docs.zerotier.com/)
<!--more-->

# Getting Started with ZeroTier
ZeroTier securely connects all your devices and services with each other, anywhere.

## What is ZeroTier?
ZeroTier is a way to connect devices over your own private network anywhere in the world. You do this by creating a network and then joining *two or more devices* to that network. You can use ZeroTier to play games, connect to remote business resources or even as a cloud backplane for your enterprise.[Create your network.](https://docs.zerotier.com/start)

> **ADVANCED**
> ZeroTier uses a set of globally distributed root servers and network controllers to automatically negotiate P2P connections for your devices and provision your networks. You use Central to manage your networks. [Here's a technical deep dive of the protocol](https://docs.zerotier.com/protocol).

## Ceate a Network
A ZeroTier network is essentially a secure Local Area Network(LAN) that you can use anywhere in the world. Let's make one and connect two devices over ZeroTier.
We'll use ```ping``` to test the connection. Any two devices that can run ZeroTier will do: laptop, phone, virtual machine, etc...
Both devices can be at the same location, on the same physical network. If you move one to a cafe or to your office, it should still just work.
The rough outline is:
  - Cerate a ZeroTier network
  - Join the network from two devices
  - ```ping``` one device from the other over the ZeroTier network
This should take about 5 minutes.
> Results Preview
> Here is a summary of the results of this tutorial, if you're a networking person.
> If this doesn't mean anything to you, that's OK. We'll get there.
> Each zerotier network you join creates a network interface on your devices. It's like adding another Ethernet port to your computer.
> ```
node1# ip -o a
1: lo    inet 127.0.0.1/8 scope host lo\       valid_lft forever preferred_lft forever
2: eth0    inet 192.168.182.201/24 brd 192.168.182.255 scope global dynamic noprefixroute eth0\       valid_lft 3277sec preferred_lft 2827sec
9: zt3jn2z57r    inet 10.2.0.11/23 brd 10.2.1.255 scope global zt3jn2z57r\       valid_lft forever preferred_lft forever
> ```
> ```
node2# ip -o a
1: lo    inet 127.0.0.1/8 scope host lo\       valid_lft forever preferred_lft forever
2: eth0    inet 192.168.182.202/24 brd 192.168.182.255 scope global dynamic noprefixroute eth0\       valid_lft 3277sec preferred_lft 2827sec
9: zt3jn2z57r    inet 10.2.0.12/23 brd 10.2.1.255 scope global zt3jn2z57r\       valid_lft forever preferred_lft forever
> ```
>```
node1# ping -c 3 10.2.0.12
PING 10.2.0.2 (10.2.0.12) 56(84) bytes of data.
64 bytes from 10.2.0.12: icmp_seq=1 ttl=64 time=5.66 ms
64 bytes from 10.2.0.12: icmp_seq=2 ttl=64 time=6.62 ms
64 bytes from 10.2.0.12: icmp_seq=3 ttl=64 time=8.50 ms

--- 10.2.0.12 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2004ms
> ```

### Create your first ZeroTier network
#### Create an account
> NOTE
> It's free, no credit card is required.
- Go to [my.zerotier.com](https://my.zerotier.com/) and create an account.
#### Create a network
- Make sure you're on the "Networks" tab of my.zerotier.com
![20240306141725](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240306141725.png)
- Click the **Create A Network** button.
This creates a virtual network with a random ID and a random name. We got "fervent_smathers" and ```d5e04297a16fa690``` here.
![20240306142035](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240306142035.png)
- Click anywhere on the network to go to the details page for this network.
See the Network Settings panel:
![20240306142144](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240306142144.png)
We don't need to change any settings, but we can change the name of the network to personalize it.
- Change "fervent_smathers" to "my cool network" or whatever you like.
- Collapse the Settings panel. Click on the word "Settings" at the top of the panel.
You don't need to change any other settings.
- See the Network Members panel:
![20240306142515](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240306142515.png)
It should say "No devices have joined this network".
- Leave this brower tab open. We'll look at it again later.

### Setup the ZeroTier app
#### Download and install ZeroTier
For mobile devices, use the app store.
- Go to [zerotier.com/download](https://www.zerotier.com/download)
- Run the installer
The ZeroTier client should now running on your devices.
#### Join your first ZeroTier network
We need to tell the client to "join" the virtual network we just created.
- Copy the Network ID of the network from my.zerotier.com. This is the long number that looks like: ```d5e04297a16fa690```
- Paste the Network ID into the "join" command on your device
On macOS and Windows, there is a menubar/tray app. Select "join" from the menu.
> **NOTE**
> Every running instance of ZeroTier has a unique address. It's the 10 digit "Address" in the app, or ```zerotier-cli info``` command.
> ZeroTier addresses are a very secure method of unique identification.

### Authorize your device on your network
At this point, your client should say "Access Denied." A device can't talk on your network unless you allow it, even if someone discovers the network's ID.

#### Authorize your device
- Go to the Members panel that we left open on my.zerotier.com
- Your node that just "joined" should appear here.
- The "Address" should match the address in your client.
- Click the "Auth?" check box for it.
- Give it a name. Type something like "laptop" or "bob" into the ```(short name)``` input.
![20240306150746](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240306150746.png)

#### Confirm authorization
Back on your computer, your client should now say "OK" instead of "ACCESS DENIED" and it should show your custom "my cool network" name.
Now you have 1 member on your network. A network with 1 member can't do much.

### Repeat with another device
We need to have 2 devices connected to the same ZeroTier network.
- Repeat the join and authorize steps with your second device.

### Test connectivity
Now you have two authorized nodes on your network. They should be able to talk over ZeroTier.
Your Network Members section should look something like this:
![20240306151255](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240306151255.png)
The "Managed IPs" will be different on your network.
We're going to test with ```ping```. It's the only program that we can think of that exists by default on every operating system.
This is a command line program, but don't worry: You can do it.
#### Gotcha: Windows blocks ping
~~Windows by default doesn't respond to pings. If you try to ping a Windows computer from a different computer, it won't work. You can enable ping.~~
ZeroTier automatically enables ping on your ZeroTier network adapter now. You can probably skip this step!
> How to enable ping on Windows
> - Search for Windows Firewall in the Start Menu, and click to open it.
> - Click Advanced Settings on the left.
> - From the left pane of the resulting window, click Inbound Rules.
> - In the right pane, find the rules titled File and Printer Sharing(Echo Request - ICMPv4-In).
> - Right-click each rule and choose Enable Rule.
> Here is a [tutorial by Microsoft](https://learn.microsoft.com/en-us/windows/security/threat-protection/windows-firewall/create-an-inbound-icmp-rule)
#### Open the command line
- Open the command line on your computer
#### Find the ZeroTier IP Address of your devices
![20240306153157](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240306153157.png)
#### Try the ping command
Back in the Command Line / Terminal that you just opened:
- type ```ping -c 5 $ZEROTIER_IP_ADDRESS``` ```<enter>``` into your command line.
A successful ```ping```
```
% ping -c 5 172.22.217.93
PING 172.22.217.93 (172.22.217.93): 56 data bytes
64 bytes from 172.22.217.93: icmp_seq=0 ttl=64 time=22.362 ms
64 bytes from 172.22.217.93: icmp_seq=1 ttl=64 time=10.157 ms
64 bytes from 172.22.217.93: icmp_seq=2 ttl=64 time=9.414 ms
64 bytes from 172.22.217.93: icmp_seq=3 ttl=64 time=9.019 ms
64 bytes from 172.22.217.93: icmp_seq=4 ttl=64 time=9.180 ms

--- 172.22.217.93 ping statistics ---
5 packets transmitted, 5 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 9.019/12.026/22.362/5.182 ms
```
Try it with both ZeroTier Managed addresses on your network.
One of them is the same device you're on, so you're pinging yourself. Pinging the other device might be a little more interesting.
> **INFO**
> If something goes wrong you might see something like:
> ```
% ping -c 5 172.22.217.92
PING 172.22.217.92 (172.22.217.92): 56 data bytes
Request timeout for icmp_seq 0
Request timeout for icmp_seq 1
Request timeout for icmp_seq 2
Request timeout for icmp_seq 3

--- 172.22.217.92 ping statistics ---
5 packets transmitted, 0 packets received, 100.0% packet loss
> ```
or
> ```
ping -c 5 192.168.123.234
PING 192.168.123.234 (192.168.123.234): 56 data bytes
92 bytes from 192.168.82.1: Destination Port Unreachable
Vr HL TOS  Len   ID Flg  off TTL Pro  cks      Src      Dst
 4  5  00 5400 56e7   0 0000  3f  01 d4ad 192.168.82.217  192.168.123.234
> ```
> There may just be a typo in the IP address. Double check that your device is authorized at my.zerotier.com
> Contact us on the [discussion forum](https://discuss.zerotier.com/) and see the [troubling section](https://docs.zerotier.com/faq) if you get stuck.

### Conclusion
`ping` doesn't accomplish anything, but it does tell us ZeroTier is working. It's useful to know about for troubleshooting networks, not just ZeroTier networks.
Visit the [discussion forum](https://discuss.zerotier.com/) to talk about your use-cases or if you get stuck.
#### Now, use ZeroTier to do something you want to do
#### Some popular uses
- Windows Remote Desktop
- ssh(try [mosh](https://mosh.org/))
- Private Gaming LAN
- Access the web interfaces of your home lab
- Build your own [VPN](https://zerotier.atlassian.net/wiki/spaces/SD/pages/7110693/Overriding+Default+Route+Full+Tunnel+Mode)
- Route to a [remote subnet](https://zerotier.atlassian.net/wiki/spaces/SD/pages/224395274/Route+between+ZeroTier+and+Physical+Networks)
- Route to a [Docker network](https://zerotier.atlassian.net/wiki/spaces/SD/pages/7274520/Using+NDP+Emulated+6PLANE+Addressing+With+Docker)
- Add [dns](https://docs.zerotier.com/dns) to your network
#### Join multiple networks
A node can join many networks at once. Make sure they don't use the same subnet!
You can have a ```home``` network, a ```friends``` network, and a ```work``` network, for example.
They don't all need to be networks that you've created. You can join other people's network.
#### Check out the Whitepaper
For more info on the cryptography and protocol, see the: [Design Whitepaper](https://docs.zerotier.com/protocol)

## FAQ
### Minimum System Requirements
ZeroTier is lightweight, portable, and compatible across all major platforms and architectures. It typically consumes less than 16 MB of memory, only about 1 MB of storage, and even has a [low-bandwidth mode](https://docs.zerotier.com/lbm) for IoT applications. It supports ```32-bit ARM(arm32)```, ```64-bit ARM(arm64)```, ```32-bit Intel(x86)```, ```64-bit Intel(x64/amd64)```, ```MIPS```, and ```s390x```. You can run it on [Linux](https://docs.zerotier.com/linux), [macOS](https://docs.zerotier.com/macos), [Windows](https://docs.zerotier.com/windows), [iOS/iPadOS](https://docs.zerotier.com/ios), [Android](https://docs.zerotier.com/android), and [FreeBSD](https://docs.zerotier.com/freebsd). You can run it on [Routers](https://docs.zerotier.com/routers), [Network Attached Storage](https://docs.zerotier.com/nas), and we [have solutions](https://docs.zerotier.com/proxy) for when you can't install ZeroTier on a small IoT sensor.
Our most recent client can still be run on tiny single-board computers like the original Raspberry Pi made over a decade ago and all versions of ZeroTier are backwards compatible.
ZeroTier is currently single-threaded so devices with more than two cores typically will not offer significant performance gains. Future versions of ZeroTier will introduce multithreading.
- While ZeroTier will operate on very low-power hardware(e.g. a single core 32-Bit ARM running at 600Mhz without AES hardware acceleration(AES-NI)), its performance will suffer.
We recommend the following for a happy ZeroTier experience: ```>=1GHz``` CPU with at least ```2``` cores, and ```AES-NI```.
> **ARM**
> If you're planning on embedding ZeroTier in a product such as a router, network attached storage or some other IoT application we see that our partners have a better experence with ```64-bit ARM``` as opposed to ```32-bit ARM``` since the core speeds are typically higher and the chips are more likely to have ```AES-NI```.
### What happens if a ZeroTier, Inc root goes down?
Out roots are globally distributed and redundant, if one fails requests will seamlessly fail-over to other roots.
If you are running your own roots the effect would be that no new connections can be established between nodes but existing connections would still work.
### Can ZeroTier, Inc. See my traffic?
No. Your traffic is end-to-end encrypted and your device's private identity keys are never transmitted off of your device. Learn more about our [cryptograph protocol](https://docs.zerotier.com/protocol#crypto).
### Metrics and Monitoring
ZeroTier Inc doesn't have access to your traffic. We don't current supply a monitoring "dashboard" for your networks and nodes. You can build your own.
- [Prometheus Metrics](https://github.com/zerotier/ZeroTierOne#prometheus-metrics) for the zerotier-one agent are available. Pipe these into the common prometheus/grafana setup.
- Use your preferred monitoring tool *over* your ZeroTier networks. Some examples: Prometheus [Blackbox exporter](https://github.com/prometheus/blackbox_exporter), [SmokePing](https://oss.oetiker.ch/smokeping/), [UptimeKuma](https://uptime.kuma.pet/)
- [Traffic Observation and Interception](https://docs.zerotier.com/rules#353trafficobservationandinterceptionaname3_5_3a)
### Why does my peers list have nodes I don't recognize?
The nodes are usually our infrastructure that your node needs to talk to in order to function. This includes thing like: [Root servers](https://docs.zerotier.com/roots) and [Controllers](https://docs.zerotier.com/controller). Or possibly nodes from a previously joined network. The command ```zerotier-cli peers``` shows a list of nodes that your node knows about. Nodes can not talk to each other unless they are joined and authorized on the same network. Our root servers and controllers do not have access to you nodes' encryption keys and thus cannot decrypt your traffic.
> **HOW TO IDENTIFY A CONTROLLER**
> A controller is a ZeroTier node. The first 10 digits of a Network ID is the controller's address.
### What is error ```NOT_FOUND```?
When your ZeroTier Client is showing NOT_FOUND as your network status, this usually means that you've entered your network ID incorrectly and are trying to join a non-existent network. Please check your network ID and try again.
### High Latency & Relaying
If you're experiencing high latency(or high ping times), it's possible the connection between two nodes is being Relayed. The command ```zerotier-cli peers``` (available in 1.4.x and above) will give you infomation on what if any connections are being relayed. The output will look something like this:
```
[root@host] # zerotier-cli peers
<ztaddr>   <ver>  <role> <lat> <link> <lastTX> <lastRX> <path>
61d294b9cb -      PLANET    43 DIRECT 1617     6580     50.7.73.34/9993
62f865ae71 -      PLANET   193 DIRECT 1617     1424     2001:49f0:d0db:2::2/9993
6xxxxxxxx1 1.6.4  LEAF     730 DIRECT 8714     8714     35.222.83.92/62191
778cde7190 -      PLANET    76 DIRECT 1617     11607    103.195.103.66/9993
992fcf1db7 -      PLANET   163 DIRECT 6623     1454     2a02:6ea0:c024::/9993
deadbeef01 1.6.4  LEAF      -1 RELAY
```
If you see the peer you're trying to contact in the RELAY state, that means packets are bouncing through our root servers because a direct connection between peers cannot be established. Side effects of RELAYING are increased latency and possible packet loss. See [Router Configuration Tips](https://docs.zerotier.com/routertips) for how to resolve this.
> **TIP**
> If you experience high latency or packet jitter for more than four hours please contact your network admin as this may be a sign of a serious network condition.
### Bandwidth considerations
In the best case scenario, and in most cases, ZeroTier connects peer-to-peer and none of your network traffic travels through our servers. This means transfers go as fast as your CPU & network can compress, encrypt and send packets, and how fast the remote end can receive them.
There are some cases, such as hostile NATs & firewalls in which your encrypted packets do indeed get relayed through our root servers. Relaying through our root adds latency. The packets must travel farther physically than they would for a direct, peer to peer connection.
We do not throttle any of these packets, nor can we read the contents of the packets due to encryption. We also, however, cannot guarantee connection reliability when this happens. We do our best effort to get your packets where they need to go, but this is not always possible.
Considerations:
- CPU speed. The current version of zerotier-one is single threaded. Raw CPU speed is important in high bandwidth use cases.
- Physical Internet connection. ZeroTier can only go as fast as the physical connection it's running over.
- Hardware Encryption. ZerTier uses hardware AES acceleration when available. Most current ARM and x86_64 platforms have AES acceleration.
- Geographic distance. If your remote peer is on a different continent, there will be latency. We can't improve the speed of light.
> TIP
> If you are experiencing slow network speeds or difficulty making direct connections see [Router Configuration Tips](https://docs.zerotier.com/routertips) or [Corporate Firewalls](https://docs.zerotier.com/corporate-firewalls)
#### Will transfers go faster on paid accounts?
ZeroTier performance is not at all related to your account or subscription level. You may be able to change things on your end to enable faster connections between peers.
### Error: Cannot connect to ZeroTier service
This error means that the ZeroTier background service on your computer is either not running, or your local firewall is preventing the UI or CLI from talking to it.
#### Windows 10
Open Task Manager and go to the "Services" tab. Scroll down until you see ```ZeroTierOneService```. The status column should say ```Running```. If it does not, right click on the line and click ```Start```
#### macOS
Open Terminal.app and excute the following commands
```
sudo launchctl unload /Library/LaunchDaemons/com.zerotier.one.plist
sudo launchctl load /Library/LaunchDaemons/com.zerotier.one.plist
```
#### Linux
If your Linux distribution uses systemd, execute ```sudo service zerotier-one start```
If not, execute ```sudo /etc/init.d/zerotier-one start```
#### Still doesn't work?
Your system firewall is likely blocking communication with the ZeroTier service. Look up instructions for how to unblock an application from the firewall for your OS. ZeroTier will need to be accessible via TCP port 9993 fro the UI and CLI to interact with it.
### Where can I find old version of ZeroTier?
See [Release](https://docs.zerotier.com/releases#old)
### What is Earth?
`Earth` or `8056c2e21c000001` was a test network operated by ZeroTier, Inc., it was decommissioned in 2023.
### How do I Update Zerotier?
When you update ZeroTier, your configuration stays. You don't need to re-join your networks.
#### macOS and Windows
Grab the latest version from the [download page](https://www.zerotier.com/download) and run the installer
#### Linux
Use your OS's package manager. eg ```apt``` on Debian based distributions, or ```yum``` or ```dnf``` on RedHat based distributions.
#### iOS and Android
Use the respective app stores.
### How to clear or reset your ZeroTier address
If you would like to clear or reset ZeroTier's address on a device (the 10-digit address node ID) or you have cloned a device and you want to prevent it from using the same address, follow these steps in order:
#### Step 1. Stop the service
##### On Windows
On Windows this is done with the service manager.(Open the Start Menu and start typing "service")
##### On macOS
On macOS you can open a terminal and use
```
sudo launchctl unload /Library/LaunchDaemons/com.zerotier.one.plist
```
##### On Linux
This is usually
```
sudo systemctl stop zerotier-one
```
or
```
sudo service zerotier-one stop
```
#### Step 2. Delete the file identity.public and identity.secret from ZeroTier's working directory
- On Windows this is usually ```\ProgramData\ZeroTier\One```
- On Mac this is ```/Library/Application Support/ZeroTier/One``` in your terminal, type opne ```/Library/Application Support/ZeroTier/One``` to open the folder in Finder
- On Linux this is usually ```/var/lib/zerotier-one```
#### Step 3. Restart the service
##### On Windows
Starting via the service manager on Windows
##### On Mac
```
sudo launchctl load /Library/LaunchDaemons/com.zerotier.one.plist
```
##### On Linux
```
sudo systemctl start zerotier-one
```
or
```
sudo service zerotier-one start
```
When started without identities ZeroTier will generate new ones. You will need to authorize this new identity on any networks.
> **CAN'T FIND AN ANSWER?**
> [Ask our community for help](https://discuss.zerotier.com/)

> **NEXT STEPS**
> Click [here](https://docs.zerotier.com/start/) to create your network and start adding devices.

## Troubleshooting
> **CAN'T FIND AN ANSWER?**
> [Ask out community for help](https://discuss.zerotier.com/)
### Ping is not working
First, make sure your device is authorized on the network and you're using the ZeroTier assigned Managed IP address. Aside from that, some OSes block pings in their local firewall by default.
#### Windows Firewall
ZeroTier versions 1.10.3 and greater automatically enable ping on ZeroTier adapters.
#### macOS Firewall
The firewall is not enabled by default on macOS, and thus pings will not be blocked by default. If your firewall is enabled on macOS, go into System Preferences -> Security & Privacy. Under Firewall Options, ensure "Enable stealth mode", is disabled. Stealth mode blocks pings.
#### Linux Firewalls
There are far too many Linux distributions out there to list instructions for all of them here. Please refer to your distribution's documentation for how to unblock ICMP packets.
> **GET STARTED**
> Click [here](https://docs.zerotier.com/start/) to create your network and start adding devices.
### Error: Cannot connect to ZeroTier service(or Node ID "Unknown" in the GUI apps)
This error means that the ZeroTier background service on your computer is either not running, or your local firewall is preventing the UI or CLI from talking to it.
#### Windows Service
Open Task Manager and go to the "Services" tab. Scroll down until you see "SeroTierOneService". The status column should say "Running". If it does not, right click on the line and click "Start".
#### macOS Service
Open Terminal.app and excute the following commands
```
sudo launchctl unload /Library/LaunchDaemons/com.zerotier.one.plist
sudo launchctl load /Library/LaunchDaemons/com.zerotier.one.plist
```
#### Linux Service
If your Linux distribution uses systemd, excute:
```
sudo service zerotier-one start
```
If not, excute:
```
/etc/init.d/zerotier-one start
```
#### Still doesn't work?
Your system firewall is likely blocking communication with the ZeroTier service. Look up instructions for how to unblock an application from the firewall for your OS. ZeroTier will need to be accessible via TCP port 9993 for the UI and CLI to interact with it.
### Emergency Instructions
"It was working, but now it's not and I'm not sure why. And I need to get up and running quick."
First, check with your friendly firewall admin that no configuration in the physical network connection has changed.
See the [CLI](https://docs.zerotier.com/cli) article for help with the CLI.
We're not aware of any bugs that require these steps, or we'd fix them, but sometimes poeple try the below.
Listed in order of severity:
#### Restart the service
##### macOS
Open Terminal.app, paste the below, and press enter:
```
sudo launchctl unload /Library/LaunchDaemons/com.zerotier.one.plist
sudo launchctl load /Library/LaunchDaemons/com.zerotier.one.plist
```
It will ask you for your password. It's the password you use to log in to your mac.
##### Windows
- Type "Services" into the Start Menu to open the Windows Services Manager
- Start and Stop the zerotier-one service in the Windows Services Manage.
#### Linux
```
systemctl restart zerotier-one
```
#### Leave all networks and rejoin them
Use the UI, or
```zerotier-cli leave <network-id>``` and ```zerotier-cli join <network-id>```
#### Stop service, Delete peers.d, Start servic
Find peers.d in the [zerotier system directory](http://localhost:3000/config#system)
#### Reset Node ID
The new node ID will have be re-authorized on any networks, and the node's managed IP address manually re-assigned if needed.
- Stop the service
- Move or delete indentity.secret and identity.public files in the [zerotier system directory](http://localhost:3000/config#system)
- Delete peers.d too
- Start the service

## What can I do with ZeroTier?
A List of Awesome ZeroTier Things
This repository contains a list of all things ZeroTier that can be found on The Internet. Feel free to add yours!
[Pull Request](https://github.com/zerotier/awesome-zerotier/edit/main/README.md) accepted!
### Contents
- [ZeroTier Self-Hosting](https://docs.zerotier.com/awesome#zerotier-self-hosting)
- [ZeroTier Training](https://docs.zerotier.com/awesome#zerotier-training)
- [ZeroTier Networking](https://docs.zerotier.com/awesome#zerotier-networking)
- [ZeroTier Linux](https://docs.zerotier.com/awesome#zerotier-linux)
- [ZeroTier Remote Access](https://docs.zerotier.com/awesome#zerotier-remote-access)
- [ZeroTier SD-WAN](https://docs.zerotier.com/awesome#zerotier-sd-wan)
- [ZeroTier IoT](https://docs.zerotier.com/awesome#zerotier-iot)
- [ZeroTier Drone/UAV](https://docs.zerotier.com/awesome#zerotier-droneuav)
- [ZeroTier Robotics](https://docs.zerotier.com/awesome#zerotier-robotics)
- [ZeroTier Automotive](https://docs.zerotier.com/awesome#zerotier-automotive)
- [ZeroTier Blockchain/Crypto](https://docs.zerotier.com/awesome#zerotier-blockchaincrypto)
- [ZeroTier Infosec](https://docs.zerotier.com/awesome#zerotier-infosec)
- [ZeroTier IBM](https://docs.zerotier.com/awesome#zerotier-ibm)
- [ZeroTier Terraform](https://docs.zerotier.com/awesome#zerotier-terraform)
- [ZeroTier Kubernetes](https://docs.zerotier.com/awesome#zerotier-kubernetes)
- [ZeroTier Docker](https://docs.zerotier.com/awesome#zerotier-docker)
- [ZeroTier Bridging](https://docs.zerotier.com/awesome#zerotier-bridging)
- [ZeroTier MikroTik](https://docs.zerotier.com/awesome#zerotier-mikrotik)
- [ZeroTier Ubiquiti](https://docs.zerotier.com/awesome#zerotier-ubiquiti)
- [ZeroTier Teltonika](https://docs.zerotier.com/awesome#zerotier-teltonika)
- [ZeroTier OpenWrt](https://docs.zerotier.com/awesome#zerotier-openwrt)
- [ZeroTier OPNsense](https://docs.zerotier.com/awesome#zerotier-opnsense)
- [ZeroTier pfSense](https://docs.zerotier.com/awesome#zerotier-pfsense)
- [ZeroTier Synology](https://docs.zerotier.com/awesome#zerotier-synology)
- [ZeroTier Raspberry Pi](https://docs.zerotier.com/awesome#zerotier-raspberry-pi)
- [ZeroTier 3-D Printing](https://docs.zerotier.com/awesome#zerotier-3-d-printing)
- [ZeroTier Homelab](https://docs.zerotier.com/awesome#zerotier-homelab)
- [ZeroTier Home Automation](https://docs.zerotier.com/awesome#zerotier-home-automation)
- [ZeroTier Video/Camera/CCTV](https://docs.zerotier.com/awesome#zerotier-videocameracctv)
- [ZeroTier EVE-NG](https://docs.zerotier.com/awesome#zerotier-eve-ng)
- [ZeroTier Blender](https://docs.zerotier.com/awesome#zerotier-blender)
- [ZeroTier Oculus](https://docs.zerotier.com/awesome#zerotier-oculus)
- [ZeroTier Gaming](https://docs.zerotier.com/awesome#zerotier-gaming)
- [ZeroTier Everything Else](https://docs.zerotier.com/awesome#zerotier-everything-else)
- [ZeroTier Reddit](https://docs.zerotier.com/awesome#zerotier-reddit)
- [ZeroTier YouTube](https://docs.zerotier.com/awesome#zerotier-youtube)
> **NEXT STEPS**
> Click [here]() to create your network and start adding devices.
### ZeroTier Self-Hosting
#### Self-Hosting Network Controller Interfaces
- [key-networks / ztncui](https://github.com/key-networks/ztncui) - GUI for self-hosted ZeroTier.
- [key-networks / ztncui-aio](https://github.com/key-networks/ztncui-aio) - ZeroTier network controller user interface in a Docker container.
- [thedunston / bash_cli_zt](https://github.com/thedunston/bash_cli_zt) - Command Line interface for self-hosted ZeroTier.
- [dec0dOS / zero-ui](https://github.com/dec0dOS/zero-ui) - GUI for self-hosted ZeroTier.
- [mdplusplus/zerotier-network-controller-ui](https://hub.docker.com/r/mdplusplus/zerotier-network-controller-ui) - Docker image for self-hosted ZeroTier.
#### Self-Hosting article
- [zero - The Dorknet rises](https://www.exclusionzone.org/2019/12/10/zerotier-the-dorknet-rises/) - Self-hosted ZeroTier on OPNsense.
- [From zero to Zerotier in k3s way](https://medium.com/iotops/from-zero-to-zerotier-in-k3s-way-eadff5745566) - Self-hosted ZeroTier on a Raspberry Pi, using k3s.
#### Self-Hosting videos
- [DB Tech - ZeroTier Network Controller in Docker](https://www.youtube.com/watch?v=oC7y_qYKUTU) - Self-hosted ZeroTier on Docker.
### ZeroTier Training
#### ZeroTier Training courses
- [CBT Nuggets - Securely Connect Local Network Devices to AWS VPC with ZeroTier](https://www.cbtnuggets.com/it-training/skills/securely-connect-local-network-devices-aws-vpczerotier)
### ZeroTier Networking
#### Networking articles
- [Abusing Public DNS for Zerotier Routing](https://dev.to/issmirnov/abusing-public-dns-for-zerotier-routing-36ke)
- [Building a Private Backplane Network for your VPSs with ZeroTier | Pete Keen](https://www.petekeen.net/building-a-private-backplane-network-with-zerotier)
- [Creating Secure Private Networks With ZeroTier VPN](https://dzone.com/articles/creating-secure-private-networks-with-zerotier-vpn)
- [Creating a Site-to-Site VPN with ZeroTier and BGP](https://www.linkedin.com/pulse/creating-site-to-site-vpn-zerotier-bgp-robert-lynch/)
- [How To Connect Everything From Everywhere with ZeroTier](https://blog.fosketts.net/2022/01/14/how-to-connect-everything-from-everywhere-with-zerotier/)
- [Multi-Cloud K3s, and also I got (temporarily) kicked off Google Cloud](https://www.danmanners.com/posts/2021-12-multi-cloud-k3s-and-booted-from-gcloud/)
- [Network Modeling: Segmented Lab access with Containerlab and ZeroTier](https://stubarea51.net/2021/11/23/network-modeling-segmented-lab-access-with-containerlab-and-zerotier)
- [Routing traffic to ZeroTier's subnet from all devices on the LAN](https://chrisatech.wordpress.com/2021/02/22/routing-traffic-to-zerotiers-subnet-from-all-devices-on-the-lan/)
#### Networking articles global
- [比frp更好用的内网穿透工具-ZeroTier One](https://jiayaoo3o.github.io/2020/06/11/%E6%AF%94frp%E6%9B%B4%E5%A5%BD%E7%94%A8%E7%9A%84%E5%86%85%E7%BD%91%E7%A9%BF%E9%80%8F%E5%B7%A5%E5%85%B7-ZeroTier%20One/#more)
- [zerotier 基礎使用步驟教學](https://blog.toolman.xyz/article/240)
- [ZeroTier | Δικτύωση απομακρυσμένων Linux, Windows, macOS, BSD, Android, iOS συσκευών](https://cerebrux.net/2021/10/24/zerotier-%ce%b4%ce%b9%ce%ba%cf%84%cf%8d%cf%89%cf%83%ce%b7-%ce%b1%cf%80%ce%bf%ce%bc%ce%b1%ce%ba%cf%81%cf%85%cf%83%ce%bc%ce%ad%ce%bd%cf%89%ce%bd-%cf%83%cf%85%cf%83%ce%ba%ce%b5%cf%85%cf%8e%ce%bd/)
#### Networking videos
- [Considered Normal? - Creating a Local LAN with ZeroTier](https://www.youtube.com/watch?v=Af758HL6VkA)
- [Duane Dunston - Private ZeroTier Network on the Public Internet](https://www.youtube.com/watch?v=xp2ujXe1SOU)
- [Duane Dunston - ZeroTier Hub and Spoke](https://www.youtube.com/watch?v=Fb65bU3oyEo)
- [Eric Sloof - ZeroTier - The ethernet switch for planet earth](https://www.youtube.com/watch?v=XrLk21Xy2i0)
- [infoTK - Finally ! work from home using VPN free and without Public IP](https://www.youtube.com/watch?v=JwGKi3UWJ_I)
- [MRP - Local Network everywhere - Zero Tier : Global Area Networking](https://www.youtube.com/watch?v=eI-yxrMH3uY)
- [Network Collective - How Does ZeroTier Actually Work?](https://www.youtube.com/watch?v=Lao9T_RQTak)
- [Lawrence Systems - Open Source Mesh VPN Solutions](https://www.youtube.com/watch?v=QfcwiSkV_AU)
- [Lawrence Systems - ZeroTier Tutorial: Delivering the Capabilities of VPN, SDN, and SD-WAN via an Open Source System](https://www.youtube.com/watch?v=Bl_Vau8wtgc)
- [LearnLinuxTV - Networking with ZeroTier: Creating software-defined networks with Ease](https://www.youtube.com/watch?v=9GTXN0opsdw)
- [Sheridan Computers - ZeroTier | Virtual Networking | Remote Desktop | Remote Working | VPN](https://www.youtube.com/watch?v=1Sobvh6OiC8)
#### Networking videos global
- [ADINATA - ZeroTier Solusi Remote Perangkat Aman Dan Nyaman Untuk Pengguna Internet Broadband Dan Dedicated](https://www.youtube.com/watch?v=7tN9iw7FIo4)
- [Usando ZeroTier One y NextDNS para acceder a tus datos por VPN](https://blog.ayudait.eu/2020/11/usando-zerotier-one-y-nextdns.html)
- [NASeros - Incorporación de un ordenador a ZeroTier](https://www.youtube.com/watch?v=ZtXhZCkxaak)
- [Servicios Virtuales Administrados SVA - Multi Usuario RDP/VPN ZeroTier -- CURSO GRATUITO](https://www.youtube.com/watch?v=z8N1NeYORbM)
- [Software Defined Networks with ZeroTier | Urdu](https://www.youtube.com/watch?v=Pk5EXqNLFzA&t=1s)
- [VPN multisitio con ZeroTier][(](https://www.youtube.com/watch?v=ZDcwKQM1J4s&t=2s))
- [Zerotier: la Terra è il tuo ufficio (sicuro e privato)](https://www.youtube.com/watch?v=LHilEAF5S2U&t)
### ZeroTier Linux
- [Duoslow / zerotierIndicator](https://github.com/Duoslow/zerotierIndicator)
#### Linux articles
- [Curl and jq to Manage ZeroTier networks](https://www.simplecto.com/zerotier-jq-manage-zerotier-networks/)
- [Install Zerotier CLI Linux](https://www.greghilston.com/post/install_zerotier/)
#### Linux videos
- [Roel Van de Paar - Install ZeroTier on Ubuntu with armhf hardware](https://www.youtube.com/watch?v=9KMHwCUzf4g)
### ZeroTier Remote Access
#### Remote Access articles
- [Remote Access Without Port Forwarding](https://john.muchovej.com/thoughts/remote-access-without-port-forwarding/)
- [Setting up Remote Access During a Crisis](https://www.spikefishsolutions.com/post/setting-up-remote-access-during-a-crisis)
- [Stratospherix FileBrowser - How to access your files from anywhere](https://www.stratospherix.com/support/how-to-access-your-files-from-anywhere-with-zerotier.php)
#### Remote Access articles global
- [Télétravail, RDP & VPN](https://www.canaletto.fr/post/teletravail-rdp-vpn)
- [zerotier 팀뷰어만 되는 환경에서 사용하니 정말 좋군요.](https://www.clien.net/service/board/park/14534750)
#### Remote Access videos
- [Lawrence Systems - How To Work Remotely Using ZeroTier & Windows Remote Desktop (RDP)](https://www.youtube.com/watch?v=ZShna7v77xc)
- [Odly Otter - Setting up ZeroTier to securely connect to your home server while roaming](https://www.youtube.com/watch?v=EgdFzxit_6M)
- [Remote Access: Securely connect your devices over the internet with ZeroTier](https://www.youtube.com/watch?v=7C2AGnr9Q-w)
- [The Cyber Mentor - Hack From Anywhere! - ZeroTier Remote Access](https://www.youtube.com/watch?v=bUOU8BW0IrM)
### ZeroTier SD-WAN
#### SD-WAN articles
- [A consumer-grade SD-WAN solution to improve mobile internet](https://www.linkedin.com/pulse/consumer-grade-sd-wan-solution-improve-mobile-jonathan-gearinger/)
- [How do I create a VPN/SD-WAN with Zerotier and Teltonika?](https://know.innon.com/vpn-zerotier-and-teltonika)
- [ZeroTier on OpenWrt (VPN + SD-WAN)](https://foro.seguridadwireless.net/openwrt/vpn-con-zerotier-en-openwrt/)
#### SD-WAN videos
- [Profitap HQ B.V - How to Connect IOTA to ZeroTier SD-WAN environment](https://www.youtube.com/watch?v=dn8rFCttJVg)
- [SecurityGuy - Hands-on with ZeroTier SD-WAN for Cloud Connectivity](https://www.youtube.com/watch?v=pntbQBtneZg)
#### SD-WAN videos global
- [LACNIC RIR - ZeroTier - Usando una solución Open Source para integrar VPNs e iniciar operaciones SD-WAN](https://www.youtube.com/watch?v=pM2ZQ9trcM8)
### ZeroTier IoT



## Source Code
### Github
[github.com/zerotier/zerotierone](https://github.com/zerotier/)
> **NEXT STEPS**
> Click [here](https://docs.zerotier.com/start/) to create your network and start adding devices.

## Releases
> **BEFORE DOWNLOADING**
> Click [here](https://docs.zerotier.com/start/) to create your network and then start adding devices.
### Downloads Page
[http://www.zerotier.com/download](http://www.zerotier.com/download)
### Past versions of ZeroTier
[download.zerotier.com/RELEASES/](http://download.zerotier.com/RELEASES/)
### Release Notes
[github.com/zerotier/zerotierone](https://github.com/zerotier/ZeroTierOne/blob/dev/RELEASE-NOTES.md)

# How ZeroTier Works
Our installable client service.
## The Protocol
ZeroTier is a smart programmable Ethernet switch for planet Earth. It allows all networked devices, VMs, containers, and applications to communicate as if they all reside in the same physical data center or cloud region.
### Overview
ZeroTier is a distributed network hypervisor built atop a cryptographically secure global peer to peer network. It provides advanced network virtualization and management capabilities on par with an enterprise SDN switch, but across both local and wide area networks and connecting almost any kind of app or device.
This is accomplished by combining a cryptographically addressed and secure peer to peer network (termed VL1) with an Ethernet emulation layer somewhat similar to VXLAN (termed VL2). Our VL2 Ethernet virtualization layer includes advanced enterprise SDN features like fine grained access control rules for network micro-segmentation and securty monitoring.
All ZeroTier traffic is encrypted end-to-end using secret keys that only you control. Most traffic flows peer to peer, though we offer free (but slow) relaying for users who cannot establish peer to peer connections.
Everything in the ZeroTier world is controlled by two types of identifier: 40-bit/10-digit *ZeroTier address* and 64-bit/16-digit *network IDs*. These identifiers are easily distinguished by their length. A ZeroTier address identifies a node or "device"(laptop, phone, server, VM, app, etc.) while a network ID identifies a virtual Ethernet network that can be joined by devices.
ZeroTier addresses can be thought of as port numbers on an enormous planet-wide enterprise Ethernet smart switch supporting VLANs. Network IDs are VLAN IDs to which these ports may be assigned. A single port can be assigned to more than one VLAN.
A ZeroTier address looks like ```8056c2e21c``` and a network ID looks like ```8056c2e21c000001```. Network IDs are composed of the ZeroTier address of that network's primary controller and an arbitrary 24-bit ID that identifies the network on this controller. Network controllers are roughly analogous to SDN controllers in SDN protocols like [OpenFlow](https://en.wikipedia.org/wiki/OpenFlow), though as with the analogy between VXLAN and VL2 this should not be read to imply that the protocols or design are the same. You can use our convenient and inexpensive SaaS hosted controllers at [my.zerotier.com](https://my.zerotier.com/) or [run your own controller](https://github.com/zerotier/ZeroTierOne/controller/) if you don't mind messing around with JSON configuration files or writing scripts to do so.
> **INFO**
> Visit [ZeroTier's site](https://www.zerotier.com/) for more information and [pre-built binary packages](https://www.zerotier.com/download/). Apps for Android and iOS are available for free in the Google Play and Apple app stores.

### Origin and Design Philosophy
The goals and design principles of ZeroTier are inspired by among other things the original [Google BeyondCorp](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/43231.pdf) paper and the [Jericho Forum](https://en.wikipedia.org/wiki/Jericho_Forum) with its notion of "deperimeterizationg."
### Network Hypervisor
The ZeroTier network hypervisor (currently found in the [node/](https://github.com/zerotier/ZeroTierOne/tree/master/node) subfolder of the ZeroTierOne git repository) is a self-contained network virtualization engine that implements an Ethernet virtualization layer similar to [VXLAN](https://en.wikipedia.org/wiki/Virtual_Extensible_LAN) on top of a global encrypted peer to peer network.
The ZeroTier protocol is original, though aspects of it are similar to VXLAN and IPSec. It has two conceptually separate but closely coupled layer [in the OSI model](https://en.wikipedia.org/wiki/OSI_model) sense: **VL1** and **VL2**. VL1 is the underlying peer to peer transport layer, the "virtual wire," while VL2 is an emulated Ethernet layer that provides operating systems and apps with a familiar communication medium.
### The ZeroTier Peer to Peer Network (VL1)
A global data center requires a global wire closet.
In conventional networks L1 (OSI layer 1) refers to the actual CAT5/CAT6 cables or wireless radio channels over which data is carried and the physical transceiver chips that modulate and demodulate it. VL1 is a peer to peer network that does the same thing by using encryption, authentication, and a lot of networking tricks to create virtual wires on a dynamic as-needed basis.
### Network Topology and Peer Discovery
VL1 is designed to be zero-configuration. A user can start a new ZeroTier node without having to write configuration files or provide the IP addresses of other nodes. It's also designed to be fast. Any two devices in the world should be able to locate each other and communicate almost instantly.
To achieve this VL1 is organized like DNS. At the base of the network is a collection of always-present **root servers** whose role is similar to that of [DNS root name servers.](https://en.wikipedia.org/wiki/Root_name_server) Roots run the same software as regular endpoints but reside at fast stable locations on the network and are designated as such by a **world definition**. World definitions come in two forms: the **planet** and one or more **moons**. The protocol includes a secure mechanism allowing world definitions to be updated in-band if root servers' IP addresses or ZeroTier addresses change.
There is only one planet. Earth's root servers are operated by ZeroTier, Inc. as a free service. There are currently four root servers distributed across the globe and multiple network providers. Almost everyone in the wrold has one within less than 100ms network latency from their location.
A node can "orbit" any number of moons. A moon is just a convenient way to add user-defined root servers to the pool. Users can create moons to reduce dependency on ZeroTier, Inc. infrastructure or to locate root servers closer for better performance. For on-premise SDN use a cluster of root servers can be located inside a building or data center so that ZeroTier can continue to operate normally if Internet connectivity is lost.
Nodes start with no direct links to one another, only upstream to roots (planet and moons). Every peer on VL1 possesses a globally unique 40-bit (10 hex digit) **ZeroTier address**, but unlike IP addresses these are opaque cryptographic identifiers that encode no routing information. To communicate peers first send packets "up" the tree, and as these packets traverse the network they trigger the opportunistic creation of direct links along the way. The tree is constantly trying to "collapse itself" to optimize itself to the pattern of traffic it is carrying.
Peer to peer connection setup goes like this:
1. A wants to send a packet to B, but since it has no direct path it sends it upstream to R (a root).
2. If R has a direct link to B, it forwards the packet there. Otherwise it sends the packet upstream until planetary roots are reached. Planetary roots know about all nodes, so eventually the packet will reach B if B is online.
3. R also sends a message called *rendezvous* to A containing hints about how it might reach A.
4. A and B get their *rendezous* messages and attempt to send test messages to each other, possibly accomplishing [hole punching](https://en.wikipedia.org/wiki/UDP_hole_punching) of any NATs or stateful firewalls that happen to be in the way. If this works a direct link is established and packets no longer need to take the scenic route.

Since roots forward packets, A and B can reach each other instantly. A and B then begin attempting to make a direct peer to peer connection. If this succeeds it results in a faster lower latency like. We call this *transport triggered link provisioning* since it's the forwarding of the packet itself that triggers the peer to peer network to attempt direct connection.
VL1 never gives up. If a direct path can't be established, communication can continue through (slower) relaying. Direct connection attempts continue forever on a periodic basis. VL1 also has other features for establishing direct connectivity including LAN peer discovery, port prediction for traversal of symmetric IPv4 NATs, and explicit port mapping using uPnP and/or NAT-PMP if these are available on the local physical LAN.
*[A blog post from 2014 by ZeroTier's original author explains some of the reasoning behind VL1's design.](http://adamierymenko.com/decentralization.html)*
### Addressing
Every node is uniquely identified on VL1 by a 40-bit (10 hex digit) **ZeroTier address**. This address is computed from the public portion of public/provate key pair. A node's address, public key, and private key together form its **identity**.
*On devices running ZeroTier One the node identity is stored in `identity.public` and `identity.secret` in the service's home directory.*
When ZeroTier starts for the first time it generates a new identity. It then attempts to advertise it upstream to the network. In the very unlikely event that the identity's 40-bit unique address is taken, it discards it and generates another.
Identities are claimed on a first come first serve basis and currently expire from planetary roots after 60 days of inactivity. If a long-dormant device returns it may re-claim its identity unless its address has been taken in the meantime (again, highly unlikely).
The address derivation algorithm used to compute addresses from public keys imposes a computational cost barrier against the intentional generation of a collision. Currently it would take approximately 10,000 CPU-years to do so (assuming e.g. a 3ghz intel core). This is expensive but not impossible, but it's only the first line of defense. After generating a collision an attacker would then have to compromise all upstream nodes, network controllers, and anything else that has recently communicated with the target node and replace their cached identities.
ZeroTier addresses are, once advertised and claimed, a very secure method of unique identification.
When a node attempts to send a message to another node whose identity is not cached, it sends a *whois* query upstream to a root. Roots provide an authoritative identity cache.
### Cryptography
If you don't know much about cryptography you can safely skip this section. **TL;DR: packets are end-to-end encrypted and can't be read by roots or anyone else, and we use modern 256-bit crypto in ways recommended by the professional cryptographers that created it.**
Asymmetric public key encryption is [Curve25519/Ed25519](https://en.wikipedia.org/wiki/Curve25519), a 256-bit elliptic curve variant.
Every VL1 packet is encrypted end to end using (as of the current version) 256-bit [Salsa20](https://ianix.com/pub/salsa20-deployment.html) and authenticated using the [Poly1305](https://en.wikipedia.org/wiki/Poly1305) message authentication (MAC) algorithm. MAC is computed after encryption [(encrypt-then-MAC)](https://tonyarcieri.com/all-the-crypto-code-youve-ever-written-is-probably-broken) and the cipher/MAC composition used is identical to the [NaCI reference implementation](https://nacl.cr.yp.to/).
As of today we do not implement [forward secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) or other stateful cryptographic features in VL1. We don't do this for the sake of simplicity, reliability, and code footprint, and because frequently changing state makes features like clustering and fail-over much harder to implement. See [our discussion on GitHub](https://github.com/zerotier/ZeroTierOne/issues/204).
We may implement forward secrecy in the future. For those who want this level of security today, we recommend using other cryptographic protocols such as SSL or SSH over ZeroTier. These protocols typically implement forward secrecy, but using them over ZeroTier also provides the secondary benefit of defense in depth. Most cryptography is compromised not by a flaw in encryption bu through bugs in the implementation. If you're using two secure transports, the odds of a critical bug being discovered in both at the same time is very low. The CPU overhead of double-encryption is not significant for most work loads.
> **SECURITY**
> [More information on ZeroTier's security practices](https://docs.zerotier.com/faq-security)

### Trusted Paths for Fast Local SDN
To support the use of ZeroTier as a high performance SDN/NFV protocol over physically secure network the protocol supports a feature called *trusted paths*. It is possible to configure all ZeroTier devices on a given network to skip encryption and authentication for traffic over a designated physical path. This can cut CPU use noticeably in high traffic scenarios but at the cost of losing virtually all transport security.
Trusted paths do not prevent communication with devices elsewhere, since traffic over other paths will be encrypted and authenticated normally.
We don't recommend the use of this feature unless you really need the performance and you know what you're doing. We also recommend thinking carefully before disabling transport security on a cloud private network. Larger cloud providers such as Amazon and Azure tend to provide good network segregation but many less costly providers offer private networks that are "party lines" and are not much more secure than the open Internet.
### Multipath
Multipath allows the simultaneous (or conditional) aggregation of multiple physical links into a bond for increased total throughput, load balancing, redundancy, and fault tolerance. There is a set of standard bonding policies available that can be used right out of the box with no configuration. These policies are inspired by the policies offered by the Linux kernel. A bonding policy can be used easily without specifying any additional parameters.
> **MULTIPATH GUIDE**
> - See [here](https://docs.zerotier.com/multipath) for more info and examples.

### Ethernet Virtualization Layer (VL2)
**VL2** is a [VXLAN](https://en.wikipedia.org/wiki/Virtual_Extensible_LAN)-like network virtualization protocol with SDN management features. It implements secure VLAN boundaries, multicast, rules, capability based security, and certificate based access control.
VL2 is built atop and carried by VL1, and in so doing it inherits VL1's encryption and endpoint authentication and can use VL1 asymmetric keys to sign and verify credentials. VL1 also allows us to implement VL2 entirely free of concern for underlying physical network topology. Connectivity and routing efficiency issues are VL1 concerns. It's important to understand that there is no relationship between VL2 virtual networks and VL1 paths. Much like VLAN multiplexing on a wired LAN, two nodes that share multiple network memberships in common will still only have one VL1 path (virtual wire) between them.
### Network Identifiers and Controllers
Each VL2 network (VLAN) is identified by a 64-bit (16 hex digit) **ZeroTier network ID** that contains the 40-bit ZeroTier address of the network's **controller** and a 24-bit number identifying the network on the controller.
```
Network ID: 8056c2e21c123456
            |         |
            |         Network number on controller
            |
            ZeroTier address of controller
```
When a node joins a network or requests a network configuration update, it sends a network config query message (via VL1) to the network's controller. The controller can then use the node's VL1 address to look it up on the network and send it the appropriate certificates, credentials, and configuration information. From the perspective of VL2 virtual networks, VL1 ZeroTier addresses can be thought of as port numbers on an enormous global-scale virtual switch.
A common misunderstanding is to conflate network controllers with root servers (planet and moons). Root servers are connection facilitators that operate at the VL1 level. Network controllers are configuration managers and certificate authorities that belong to VL2. Generally root servers don't join or control virtual networks and network controllers are not root servers, though it is possible to have a node do both.
#### Controller Security Considerations
Network controllers serve as certificate authorities for ZeroTier virtual networks. As such, their `identity.secret` files should be guarded closely and backed up securely. Compromise of a controller's secret key would allow an attacker to issue fraudulent network configurations or admit unauthorized members, while loss of the secret key results in loss of ability to control the network in any way or issue configuration updates and effectively renders the network unusable.
It is important that controllers' system clocks remain relatively accurate (to within 30-60 seconds) and that they are secure against against remote tampering. Many cloud providers provide secure time sources either directly via the hypervisor or via NTP servers within their networks.
### Certificates and Other Credentials
All credentials issued by network controllers to member nodes in a given network are signed by the controller's secret key to allow all network members to verify them. Credentials have timestamp fields populated by the controller, allowing relative comparison without the need to trust the node's local system clock.
Credentials are issued only to their owners and are then pushed peer to peer by nodes that wish to communicate with other nodes on the network. This allows networks to grow to enormous sizes without requiring nodes to cache large numbers of credentials or to constantly consult the controller.
#### Credential Types
- **Certificates of Membership**: a certificate that a node presents to obtain the riht to communicate on a given network. Certificates of membership are accepted if they *agree*, meaning that the submitting member's certificate's timestamp differs from the recipient's certificate's timestamp by no more than the recipient certificate's maximum timestamp delta value. This creates a decentralized moving-window scheme for certificate expiration without requiring node clock synchronization or constant checking with the controller.
- **Revocations**: a revocation instantaneously revokes a given credential by setting a hard timestamp limit before which it will not be accepted. Revocations are rapidly propagated peer to peer among members of a network using a rumor mill algorithm, allowing a controller to revoke a member credential across the entire network even if its connection to some members is unreliable.
- **Capabilities**: a capability is a bundle of network rules that is signed by the controller and can be presented to other members of a network to grant the presenter elevated privileges within the framwork of the network's base rule set. More on this in the section on rules.
- **Tags**: a tag is a key/value pair signed by the controller that is automatically presented by members to one another and can be matched on in base or capability network rules. Tags can be used to categorize members by role, department, classification, ect.
- **Certificates of Ownership**: these certify that a given netwrok member owns something, such as an IP address. These are currently only used to lock down netwroks against IP address spoofing but could be used in the future to certify ownership of other network-level entities that can be matched in a filter.

### Multicast, ARP, NDP, and Special Addressing Modes
ZeroTier netwroks support multicast via a simple publish/subscribe system.
When a node wishes to receive multicasts for a given multicast group, it advertises membership in this group to other members of the network with which it is communicating and to the network controller. When a node wishes to send a multicast it both consults its cache of recent advertisements and periodically solicits additional advertisements.
Broadcast (Ethernet *ff:ff:ff:ff:ff:ff*) is treated as a multicast group to which all members subscribe. It can be disabled at the network level to reduce traffic if it is not needed. IPv4 ARP receives special handling (see below) and will still work if normal broadcast is disabled.
Multicasts are propagated using simple sender-side replication. This places the full outbound bandwidth load for multicast on the sender and minimizes multicast latency. Network configurations contain a network-wide **multicast limit** configurable at the network controller. This specifies the maximum number of other nodes to which any node will send a multicast. If the number of known recipients in a given multicast group exceeds the multicast limit, the sender chooses a random subset.
There is no global limit on multicast recipients, but setting the multicast limit very high on very large networks could result in significant bandwidth overhead.
#### Special Handling of IPv4 ARP Broadcasts
IPv4 [ARP](https://en.wikipedia.org/wiki/Address_Resolution_Protocol) is built on simple Ethernet broadcast and scales poorly on large or distributed networks. To improve ARP's scalability ZeroTier generates a unique multicast group for each IPv4 address detected on its system and then transparently intercepts ARP queries and sends them only to the correct group. This converts ARP into effectively a unicast or narrow multicast protocol (like IPv6 NDP) and allows IPv4 ARP to work reliably across wide area networks without excess bandwidth consumption. A similar strategy is implemented under the hood by a number of enterprise switches and WiFi routers designed for deployment on extremely large LANs. This ARP emulation mode is transparent to the OS and application layers, but it does mean that packet sniffers will not see all ARP queries on a virtual network the way they typically can on smaller wired LANs.
#### Multicast-Free IPv6 Addressing Modes
IPv6 uses a protocol called [NDP](https://en.wikipedia.org/wiki/Neighbor_Discovery_Protocol) in place of ARP. It is similar in role and design but uses narrow multicast in place of broadcast for superior scalability on large networks. This protocol nevertheless still imposes the latency of an additional multicast lookup whenever a new address is contacted. This can add hundreds of milliseconds over a wide area network, or more if latencies associated with pub/sub recipient lookup are significant.
IPv6 addresses are large enough to easily encode ZeroTier addresses. For faster operation and better scaling we've implemented several special IPv6 addressing modes that allow the local node to emulate NDP. These are ZeroTier's **rfc4193** and **6plane** IPv6 address assignment schemes. If these addressing schemes are enabled on a network, nodes locally intercept outbound NDP queries for matching addresses and then locally generate spoofed NDP replies.
Both modes dramatically reduce initial connection latency between network members. **6plane** additionally exploits NDP emulation to transparently assign an entire IPv6 /80 prefix to every node without requiring any node to possess additional routing table entries. This is designed for virtual machine and container hosts that wish to auto-assign IPv6 addresses to guests and is very useful on microservice architecture backplane networks.
Finally there is a security benefit to NDP emulation. ZeroTier addresses are cryptographically authenticated, and since Ethernet MAC addresses on networks are computed from ZeroTier addresses these are also secure. NDP emulated IPv6 addressing modes are therefore not vulnerable to NDP reply spoofing.
Normal non-NDP-emulated IPv6 addresses (including link-local addresses) can coexist with NDP-emulated addressing schemes. Any NDP queries that do not match NDP-emulated addresses are sent via normal multicast.
### Ethernet Bridging
ZeroTier emulates a true Ethernet switch. This includes the ability to L2 bridge other Ethernet networks (wired LAN, WiFi, virtual backplanes, etc.) to virtual networks using conventional Ethernet bridging.
To act as a bridge a network member must be designated as such by the controller. This is for security reasons as normal network members are not permitted to send traffic from any origin other than their MAC address. Designated bridges also receive special treatment from the multicast algorithm, which more aggressively and directly queries them for group subscriptions and replicates all broadcast traffic and ARP requests to them. As a result bridge nodes experience a slightly higher amount of multicast bandwidth overhead.
Bridging has been tested extensively on Linux using the Linux kernel native bridge, which cleanly handles network MTU mismatch. There are third party reports of bridging working on other platforms. The details of setting up bridging, including how to selectively block traffic like DHCP that may not be wanted across the bridge, are beyond the scope of this manual.
> **GUIDE**
> See our [bridging tutorial](https://docs.zerotier.com/bridging)

### Public Networks
It is possible to disable access control on a ZeroTier network. A public network's members do not check certificates of membership, and new members to a public network are automatically marked as authorized by their host controller. It is not possible to de-authorize a member from a public network.
Rules on the other hand are enforced, so it's possible to implement a special purpose public network that only allows access to a few things or that only allows a restricted subset of traffic.
Public networks are useful for testing and for peer to peer "party lines" for gaming, chat, and other applications. **Participants in public networks are warned to pay special attention to security. If joining a public network be careful not to expose vulnerable services or accidentally share private files via open network shares or HTTP servers. Make sure your operating system, applications, and services are fully up to date.**
### Ad-Hoc Networks
A special kind of public network called an ad-hoc network may be accessed by joining a network ID with the format:
```
ffSSSSEEEE000000
| |   |   |
| |   |   Reserved for future use, must be 0
| |   End of port range (hex)
| Start of port range (hex)
Reserved ZeroTier address prefix indicating a controller-less network
```
Ad-hoc networks are public (no access control) networks that have no network controller. Instead their configuration and other credentials are generated locally. Ad-hoc networks permit only IPv6 UDP and TCP unicast traffic (no multicast or broadcast) using 6plane format NDP-emulated IPv6 addresses. In addition an ad-hoc network ID encodes an IP port range. UDP packets and TCP SYN (connection open) packets are only allowed to destination ports within the encoded range.
For example `ff00160016000000` is an ad-hoc network allowing only SSH, while `ff0000ffff000000` is an ad-hoc network allowing any UDP or TCP port.
Keep in mind that these networks are public and anyone in the entire world can join them. Care must be taken to avoid exposing vulnerable services or sharing unwanted files or other resources.
> **GETTING STARTED**
> Click [here](https://docs.zerotier.com/start/) to create your network and start adding devices.

## Security
### Cryptography
ZeroTier uses state of the art cryptographic methods to encrypt your traffic end-to-end. [Read more here](https://docs.zerotier.com/protocol#cryptography)
### Report a vulnerability
If you believe you've found a vulnerability please follow the instructions here to [report it to our team](https://github.com/zerotier/ZeroTierOne/blob/dev/SECURITY.md)
### Audit
In March 2020 we worked with `@trailofbits` to audit our protocol and cryptographic designs for ZeroTier 2.0. As one of its subjects is in our just-released beta, we are ready to make our first preliminary audit public [here](https://storage.googleapis.com/zt-web-large-files/ZeroTier%20Protocol%20Review%20Summary.pdf). A full code audit is coming after 2.0.
## Rules Engine
Traffic on ZeroTier networks can be observed and controlled with a system of globally applied network rules. These are enforced in a distributed fashion by both the senders and the receivers of packets. To escape the rules engine a malicious attacker would need to fully compromise both sides of any conversation.
The ZeroTier VL2 rules engine differs from most other firewalls and SDN rules engines in several ways. The most immediately relevant of these is that the ZeroTier rules engine is stateless, meaning it lacks connection tracking. This means that bidirectional whitelisting can't be accomplished by simply whitelisting reply packets to established connections. Instead some thought must be put into how to allow both sides of a desired flow. Rule patterns to achieve the most common desired objectives are included in this manual.
The decision to make our rules engine stateless was a design trade-off driven by several concerns. First we wanted to keep complexity, code footprint, and memory use very low to support small embedded devices. The second and more fundamental reason is that distributed stateful filtering requires distributed state synchronization. This would have added a large volume of additional sync traffic as well as introducing [inescapable](https://en.wikipedia.org/wiki/CAP_theorem) new sources of instability and failure and a lot of surface area for security vulnerabilities.
While ZeroTier lacks state tracking, its rules engine includes something not found anywhere else in the enterprise networking space: [capability-based security](https://en.wikipedia.org/wiki/Capability-based_security) and device tagging. Capabilities and tags allow extremely complex micro-segmented network rule schemes to be implemented in a sane, conceptual way that is both easier for human beings to understand and more efficient for machines to handle.
This section assumes some level of familiarity with network rules as they're commonly used on firewalls and routers, etc. While the rules engine is part of VL2, it's been given its own section in this manual due to the depth and cross-cutting nature of the topic.
### Rule Sets and Rule Evaluation
Rule sets are ordered lists of one or more rules, with each rule consisting of one or more **match** conditions followed by one **action**. As a rule set is evaluated, each match is tested in order and is then ANDed or ORed with the previous match result state. When an action is encountered it is taken if the result of the preceding matches is true. An action with no preceding matches is always taken. If no permissive actions are taken by any rule set the packet is discarded.
Here is a simple rule set that constrains Ethernet traffic on a network to only IPv4, ARP, or IPv6 as it would appear in the raw JSON format used by ZeroTier One's built-in network controller implementation. (Don't worry if this seems verbose and difficult. We have a more human-friendly way of writing rule sets, but before we introduce it it's important to understand what is really happening.)
```json
[
  {
    "etherType": 2048,
    "not": true,
    "or": false,
    "type": "MATCH_ETHERTYPE"
  },
  {
    "etherType": 2054,
    "not": true,
    "or": false,
    "type": "MATCH_ETHERTYPE"
  },
  {
    "etherType": 34525,
    "not": true,
    "or": false,
    "type": "MATCH_ETHERTYPE"
  },
  {
    "type": "ACTION_DROP"
  },
  {
    "type": "ACTION_ACCEPT"
  }
]
```
This checks whether an Ethernet level packet is not IPv4 (ethertype 2048) and not IPv4 ARP (ethertype 2054) and not IPv6 (ethertype 34525). If all three matches evaluate to true (meaning the ethertype is none of these) then the **drop** action is taken. Otherwise the **accept** action is taken.
Networks have one base rule set that is applied to all traffic. Its size is constrained to 1024 entries (each match or action is an entry). It should be used to set the overall policies for all members of the network, and for most common use cases it's all you'll need. For more complex scenarios, both capabilities and tags provide methods of both managing complexity and scaling the overall size of a network's rule system.
### Actions and Match Conditions
These are the available matches and actions in raw form. The rule definition language outlined in [Rule Definition Language](https://docs.zerotier.com/rules#rule-definition-language) provides a friendlier way for human beings to specify rules.

**Action**|**Argument(s)**|**Description**
----|----|----
ACTION_DROP||Drop packet and terminate all rule evaluation, including capabilities.
ACTION_BREAK||Terminate evaluation of this rule set but continue evaluating capabilities.
ACTION_ACCEPT||Accept packet and terminate further evaluation.
ACTION_TEE|length, address|Send a copy of up to the first *length* bytes (-1 for all) to a ZeroTier address.
ACTION_REDIRECT|address|Transparently redirect this packet to a ZeroTier address without changing its headers.
**Match Condition**|**Argument(s)**|**Description**
MATCH_SOURCE_ZEROTIER_ADDRESS|zt|VL1 source address.
MATCH_DEST_ZEROTIER_ADDRESS|zt|VL1 destination address.
MATCH_MAC_SOURCE|mac|Ethernet source address.
MATCH_MAC_DEST|mac|Ethernet destination address.
MATCH_IPV4_SOURCE|ip|IPv4 source address.
MATCH_IPV4_DEST|ip|IPv4 destination address.
MATCH_IPV6_SOURCE|ip|IPv6 source address.
MATCH_IPV6_DEST|ip|IPv6 destination address.
MATCH_IP_TOS|mask,start,end|IP TOS field bitwise-ANDed with mask is within range.
MATCH_IP_PROTOCOL|ipProtocol|IP protocol number.
MATCH_ETHERTYPE|etherType|Ethernet frame type.
MATCH_ICMP|icmpType,icmpCode|ICMP type and code, if applicable. (V4 or V6)
MATCH_IP_SOURCE_PORT_RANGE|start,end|IP (V4 or V6) source port range (inclusive).
MATCH_IP_DEST_PORT_RANGE|start,end|IP (V4 or V6) destination port range (inclusive).
MATCH_CHARACTERISTICS|mask|Bitwise AND characteristic bits with mask, true if result is nonzero.
MATCH_FRAME_SIZE_RANGE|start,end|Ethernet frame size range (inclusive).
MATCH_RANDOM|probability|Match if a random 32-bit number is less than or equal to the probability.
MATCH_TAGS_DIFFERENCE|id,value|Difference between tags with this ID is less than or equal to the value.
MATCH_TAGS_BITWISE_AND|id,value|Tags ANDed together equal value.
MATCH_TAGS_BITWISE_OR|id,value|Tags ORed together equal value.
MATCH_TAGS_BITWISE_XOR|id,value|Tags XORed together equal value.
MATCH_TAGS_EQUAL|id,value|Both tags equal the same value.
MATCH_TAG_SENDER|id,value|Sending side's tag equals this value.
MATCH_TAG_RECEIVER|id,value|Receiving side's tag equals this value.
### Capabilities
A capability is a small rule set that is bundled into a credential object, signed by the network controller, and issued to only those member(s) permitted to exercise it. When a member detects that outgoing traffic does not match the base rule set but is allowed by one of its capabilities, it periodically pushes the matching capability credential to the recipient ahead of the packet(s) in question. Peer to peer capability distribution is automatic and is triggered by capability match.
When the recipient receives the capability it authenticates it by checking its signature and timestamp and, provided the capability is valid, adds it to the set of capabilities to apply to incoming traffic from the capability's owner. The sender has effectively told the recipient "I can too send this packet! Teacher says so!"
Capabilities allow large systems of rules to be broken down into functional aspects and then distributed intelligently only to those members with a need to know. This avoids the bandwidth and storage overhead of distributing huge monolithic rule sets and organizes rules conceptually to make them easier for administrators to understand.
There are three terminating actions that can be taken in a rule set: **accept**, **break**, and **drop**. The accept action terminates rule evaluation and accepts the packet. The break action terminates the evaluation of the current rule set but permits the further evaluation of capabilities. The drop action terminates rule evaluation and drops the packet without checking capabilities in the base rule set, but is equivalent to break in capability rule sets. In most cases break should be used unless certain traffic must be absolutely prohibited under any circumstance.
In the simple base rule set example in section 3.1 the drop action is taken in the unapproved case. This means that Ethernet whitelisting cannot be overridden by a capability. If we change *ACTION_DROP* in our example to *ACTION_BREAK*, then it becomes possible to issue the following capability:
```json
[
  {
    "etherType": 2114,
    "not": false,
    "or": false,
    "type": "MATCH_ETHERTYPE"
  },
  {
    "type": "ACTION_ACCEPT"
  }
]
```
Ethertype 2114 is [wake-on-LAN](https://wiki.wireshark.org/WakeOnLAN), a special packet that can cause some systems to wake from sleep mode. If we place the above tiny rule set into a capability and issue it to a device, this device *but no others* will now be permitted to send wake-on-LAN magic packets. (Wake-on-LAN requires hardware support so it would only work to target devices plugged into a physical network bridged to a ZeroTier network, but don't worry about that here. It's just an example of special traffic.)
Capability rule sets are limited to only 64 entries. The idea is to keep them small and simple. A capability should grant one thing or one small set of conceptually related things.
### Tags
ZeroTier provides a second mechanism to control rule set complexity. Tags are 32-bit numeric key-value pair credentials that are issued to network members and signed by the controller. They are then distributed peer to peer on a need to know basis in a similar manner to capabilities.
Tags provide a way to conditionally drop or allow traffic between members by member classification. They allow very detailed network micro-segmentation by member role, permission, function, etc. without resulting in a combinatorial explosion in rules table size.
Let's say we want to permit traffic on TCP ports 139 and 445 (netbios/CIFS file sharing) only between systems that belong to the same department. Our company has 12,000 devices and 10 departments. Without tags this would require 144,000,000 rules, but with tags it can be accomplished by only a few.
First a tag is created to represent the department. Let's give it tag ID 100. Each member system receives the tag with a value from 1 to 10 indicating which department it belongs to. We can then add the following rules to our network's base rule set (or to a capability if so desired):
```json
[
  {
    "type": "MATCH_IP_DEST_PORT_RANGE",
    "not": false,
    "or": false,
    "start": 139,
    "end": 139
  },
  {
    "type": "MATCH_IP_DEST_PORT_RANGE",
    "not": false,
    "or": true,
    "start": 445,
    "end": 445
  },
  {
    "type": "MATCH_IP_PROTOCOL",
    "not": false,
    "or": false,
    "ipProtocol": 6
  },
  {
    "type": "MATCH_TAGS_DIFFERENCE",
    "not": false,
    "or": false,
    "id": 100,
    "value": 0
  },
  {
    "type": "ACTION_ACCEPT"
  }
]
```
This tells members in our network to accept TCP packets on ports 139 or 445 if the difference between tags with tag ID 10 is zero, meaning they match. (If a member does not have a value for this tag, it does not match.) Now all members of the same department can access CIFS file shares, but CIFS sharing between departments could still be prohibited. (TCP whitelisting requires some additional rules due to the stateless nature of our rules engine. See the section below on rule design patterns.)
Tags can be compared on numeric value or as bit fields via several different bit mask operations allowing many different systems of member classification to be implemented.
Tags without a default value may behave confusingly. As a best practice, use `default 0` in your tag definitions.
### Rule Definition Language
Raw rule sets are verbose and difficult to write, so we created a minimal rule definition language that's easier for human beings.
The parser for this language can be found in the [rule-compiler subfolder of the ZeroTierOne project](https://github.com/zerotier/ZeroTierOne/tree/master/rule-compiler) and as [zerotier-rule-compiler](https://www.npmjs.com/package/zerotier-rule-compiler) on NPM. It's written in JavaScript and is the same code that powers the in-browser editor in ZeroTier Central.
### An Introductory Example
```bash
# Whitelist only IPv4 (/ARP) and IPv6 traffic and allow only ZeroTier-assigned IP addresses
drop                      # drop cannot be overridden by capabilities
  not ethertype ipv4      # frame is not ipv4
  and not ethertype arp   # AND is not ARP
  and not ethertype ipv6  # AND is not ipv6
  or not chr ipauth       # OR IP addresses are not authenticated (1.2.0+ only!)
;

# Allow SSH, HTTP, and HTTPS by allowing all TCP packets (including SYN/!ACK) to these ports
accept
  dport 22 or dport 80 or dport 443
  and ipprotocol tcp
;

# Create a tag for which department someone is in
tag department
  id 1000                 # arbitrary, but must be unique
  enum 100 sales          # has no meaning to filter, but used in UI to offer a selection
  enum 200 engineering
  enum 300 support
  enum 400 manufacturing
  default 0
;

# Allow Windows CIFS and netbios between computers in the same department using a tag
accept
  dport 139 or dport 445
  and ipprotocol tcp
  and tdiff department 0  # difference between department tags is 0, meaning they match
;

# Drop TCP SYN,!ACK packets (new connections) not explicitly whitelisted above
break                     # break can be overridden by a capability
  chr tcp_syn             # TCP SYN (TCP flags will never match non-TCP packets)
  and not chr tcp_ack     # AND not TCP ACK
;

# Create a capability called "superuser" that lets its holders override all but the initial "drop"
cap superuser
  id 1000 # arbitrary, but must be unique
  accept; # allow with no match conditions means allow anything and everything
;

# Accept other packets
accept;
```
This creates a network that can pass IPv4 (and ARP) and IPv6 traffic but no other Ethernet frame types. In addition the `not chr ipauth` condition drops traffic between IP addresses that have not been assigned by ZeroTier to their respective sources or destinations, blocking all IP spoofing. These are enforced with a hard `drop`, preventing them from being overridden by any capability.
All UDP is allowed, but all non-whitelisted new TCP connections (SYN/!ACK packets) are blocked. This is done with a soft `break`, allowing it to be overridden by capabilities. New TCP connections are allowed on ports 22, 80, and 443 for everyone. Windows file sharing is allowed only between computers in the same department by way of a tag. A super-user capability that can be assigned to administrative nodes allows the sender to initiate any kind of connection.
See TCP [Whitelisting](https://docs.zerotier.com/rules/#tcp-whitelisting) for a discussion of how we accomplish TCP whitelisting here.
### Rule Definition Language Syntax
```bash
# The remainder of this line is a comment.

action [ ... args ... ]
  [and|or] [not] match [... args ...]
  [ ... additional matches ... ]
;

tag <name>
  id <id>                 # arbitrary unique 32-bit ID
  [default <value>]       # default tag value to assign to members
  [enum <value> <name>]   # value can be any 32-bit unsigned integer
  [flag <bit> <name>]     # bit can be 0 to 31
  [ ... additional enums or flags ... ]
;

cap <name>
  id <id>                 # arbitrary unique 32-bit ID
  action [ ... args ... ]
    [ ... ]
  ;
  [ ... additional action blocks ... ]
;

macro <name[($var1,...)]>
  action [ ... args ... ]
    [ ... ]
  ;
;

include <name(value,...)>
```
Each action, tag, capability, or macro block ends with a semicolon. White space separates things. Indentation is not significant. Hash symbols indicate that the remainder of a line is a comment.
As described in sections [Rule Sets and Rule Evaluation](https://docs.zerotier.com/rules#rulesets) through [Tags](https://docs.zerotier.com/rules#tags), a rule set is composed of one or more sequences of match,[match],…,action in which the action is taken if the chain of matches evaluates to true. Capabilities are small bundles of rules that can be assigned to nodes to give them special abilities, and tags are key/value pairs that can be assigned to nodes to allow rules to be selectively applied.
Macros are just a convenient way of defining more complex actions that can then be applied in multiple places. An example of a macro might be:
```bash
macro allowtcp($port)
  accept
    ipprotocol tcp
    and dport $port
  ;
;
```
You could then allow TCP ports in a standard TCP whitelisting scheme by just saying:
```bash
include allowtcp(80)
include allowtcp(443)
include allowtcp(8080)
```
Tag `enum` and `flag` directives have no meaning to the actual ZeroTier rules engine, but they can be used by user interfaces like ZeroTier Central to make it easier to assign and search tags.
### Actions, Matches, Operators, and Constants

**Action**|**Argument(s)**|**Description**
----|----|----
drop||Drop packet and terminate all rule evaluation, including capabilities.
break||erminate evaluation of this rule set but continue evaluating capabilities.
accept||Accept packet and terminate further evaluation.
tee|&lt;length&gt; &lt;address&gt;|Send a copy of up to the first length bytes (-1 for all) to a ZeroTier address.
redirect|&lt;address&gt;|Transparently redirect this packet to a ZeroTier address without changing its headers.
**Match Condition**|**Argument(s)**|**Description**
ztsrc|&lt;address&gt;|VL1 source address.
ztdest|&lt;address&gt;|VL1 destination address.
macsrc|&lt;address&gt;|Ethernet source address.
macdest|&lt;address&gt;|Ethernet destination address.
ipsrc|&lt;address/prefix&gt;|Source IP address or network (V4 or V6 auto-detected).
ipdest|&lt;address/prefix&gt;|Destination IP address or network (V4 or V6 auto-detected).
iptos|&lt;mask&gt; &lt;start[-end]&gt;|IP TOS field bitwise-ANDed with mask is within range.
ipprotocol|&lt;protocol&gt;|IP protocol number.
ethertype|&lt;type&gt;|Ethernet frame type.
icmp|&lt;type&gt; &lt;code&gt;|ICMP type (V4 or V6) and code. Use -1 for code if not applicable.
sport|&lt;start[-end]&gt;|IP (V4 or V6) source port range (inclusive).
dport|&lt;start[-end]&gt;|IP (V4 or V6) destination port range (inclusive).
chr|&lt;mask/name&gt;|Bitwise AND characteristic bits with mask, true if result is nonzero.
framesize|&lt;start[-end]&gt;|Ethernet frame size range (inclusive).
random|&lt;probability&gt;|Match randomly with probability 0.0 (0%) to 1.0 (100%).
tdiff|&lt;id&gt; &lt;value&gt;|Difference between tags (absolute value) with this ID is less than or equal to the value.
tand|&lt;id&gt; &lt;value&gt;|Tags ANDed together equal value.
tor|&lt;id&gt; &lt;value&gt;|Tags ORed together equal value.
txor|&lt;id&gt; &lt;value&gt;|Tags XORed together equal value.
teq|&lt;id&gt; &lt;value&gt;|Both tags equal the same value.
tseq|&lt;id&gt; &lt;value&gt;|Sending side's tag equals this value.
treq|&lt;id&gt; &lt;value&gt;|Receiving side's tag equals this value.

Match conditions may be joined by **and** (default if none specified) or **or** and may be modified by **not**.
Match conditions are evaluated from left to right; `a and b or c` is a different rule than `b or c and a`. The rules language doesn't support parentheses, but for example purposes the above would evaluate as: `(a and b) or c` vs `(b or c) and a`
Traffic won't be `tee`d if it is blocked by a `break` or `drop`.
For convenience the following symbols can be used when matching on certain packet attributes:
- IP protocols (`ipprotocol`): **icmp** (for IPv4), **igmp**, **ipip**, **tcp**, **egp**, **igp**, **udp**, **rdp**, **esp**, **ah**, **icmp6** (for IPv6), **l2tp**, **sctp**, and **udplite**.
- Ethernet frame types (`ethertype`): **ipv4**, **arp**, **ipv6**, **wol** (wake on LAN), **rarp**, **atalk**, **aarp**, **ipx_a**, **ipx_b**.
- Packet characteristics (bit masks for `chr`):
  - **inbound**: packet is being filtered on the receiving side (use not inbound for sending side)
  - **multicast**: destination is a multicast or broadcast MAC
  - **broadcast**: destination is the broadcast MAC
  - **ipauth**: sender IP is assigned by ZeroTier to the sending node
  - **tcp_fin**: packet is TCP with FIN flag set
  - **tcp_syn**: packet is TCP with SYN flag set
  - **tcp_rst**: packet is TCP with RST flag set
  - **tcp_psh**: packet is TCP with PSH flag set
  - **tcp_ack**: packet is TCP with ACK flag set
  - **tcp_urg**: packet is TCP with URG flag set
  - **tcp_ece**: packet is TCP with ECE flag set
  - **tcp_cwr**: packet is TCP with CWR flag set
  - **tcp_ns**: packet is TCP with NS flag set
  - **tcp_rs2**: packet is TCP with RS2 (reserved bit 2) flag set
  - **tcp_rs1**: packet is TCP with RS1 (reserved bit 1) flag set
  - **tcp_rs0**: packet is TCP with RS0 (reserved bit 0) flag set

### Usefull Design Patterns
### TCP Whitelisting
First, add this at or near the bottom of your rules:
```
# Block TCP SYN,!ACK to prevent new non-whitelisted TCP connections from being initiated
# unless previously whitelisted or allowed by a capability.
break chr tcp_syn and not chr tcp_ack;
```
Then above the SYN,!ACK break (or in a capability) add rules to allow TCP packets with permitted destination ports:
```
# Allow TCP port 80 (HTTP)
accept ipprotocol tcp and dport 80;
```
ZeroTier's filter is stateless. If we block all TCP packets except those with the correct destination port, this will prevent reply packets from returning to their senders.
Allowing reply packets with the correct (inverse) source port seems like a simple fix, but this is insecure as it allows anyone to initiate any TCP connection if they bind to whitelisted source ports.
The best solution is to block all non-whitelisted TCP packets with flags SYN,!ACK (SYN and not ACK) and and then whitelist desired destination ports. This way TCP replies and control traffic are allowed, but new connections cannot be opened unless they are permitted.
### Locking Down UDP
UDP is tougher to deal with in a stateless paradigm. It's connectionless so there is no way to specifically select a new session vs. an existing session. The best way to lock down UDP on a network is to use tags to allow it to and from things like DNS servers that need to speak it.
```
tag udpserver
  id 1000
  default 0
;

accept
  ipprotocol udp
  and tor udpserver 1
  or chr multicast
;

break ipprotocol udp;
```
First we define a tag called udpserver with a default value of 0. We don't set any enums or flags for this tag since it will be used as a boolean. For servers that need to respond to DNS queries, set the udpserver to 1.
Then we accept UDP traffic if the value of the udpserver tag is 1 when both sender and receiver tags are ORed together, or if UDP traffic is multicast. This allows multicast mDNS and Netbios announcements and allows UDP traffic to and from UDP servers, but prohibits other horizontal UDP traffic.
### Traffic Observation and Interception
Here's a simple rule to monitor everything:
```
# Send a copy of EVERY packet on both sender and receiver side to ZeroTier address "deadbeef11".
tee -1 deadbeef11;
```
That's going to flood `deadbeef11` with two full copies of every single packet, since it will match on both the sender and the recipient side. A less bandwidth-intensive security monitor setup might look like this:
```
tee 128 deadbeef11
  chr inbound
  and chr tcp_syn or chr tcp_rst or chr tcp_fin
  or random 0.1
;

tee 128 deadbeef22
  not chr inbound
  and chr tcp_syn or chr tcp_rst or chr tcp_fin
  or random 0.1
;
```
This is a bit more clever. It sends the first 128 bytes of every TCP SYN, RST, or FIN packet (TCP connection open and close) to one observer on the inbound side and another observer on the outbound side. It also sends the first 128 bytes of other packets with a probability of one in ten. The first 128 bytes of a packet will be enough to see the Ethernet and IP headers as well as layer 4+ information about many protocols.
This would allow observers to watch every new TCP connection on the network and also passively monitor other traffic in a "fuzzy" probabilistic fashion without using very much bandwidth. We split sending and receiving observers to prevent duplicate packets and also to allow us to detect cases where one side is failing to tee traffic more frequently than would be easily explained by packet loss.
There are many, many variations on the above that are possible but hopefully these will be enough to get you started.
Passive out-of-line monitoring with **tee** is not perfect. If bandwidth between two parties exceeds the bandwidth of one or both parties to the observer, packets will get dropped. It also provides no mechanism to filter traffic in depth since the observer cannot directly intervene (though it could de-authorize a member via the controller API).
Active in-line monitoring can be achieved with **redirect**:
```
redirect deadbeef11
  dport 80 or sport 80
  and ipprotocol tcp
;
```
This will pipe all HTTP traffic through `deadbeef11`. The target node will receive each Ethernet frame intact including its original Ethernet source and destination MAC address. To forward the packet on to its final destination, simply re-send it unmodified via the same interface on which it was received. The intermediate node could scan, modify, and interrupt HTTP traffic across the network. To regular network participants this is completely invisible as it occurs at layer 2 (Ethernet), though some performance degradation may be noticed especially if the link is running across WAN.
### The Classified System Pattern
One useful pattern for access control resembles the way military organizations classify data. Information is deemed classified, and only those who have the required level of classification are allowed to access it.
A model resembling this can be implemented in ZeroTier network rules as follows:
```
# Is this member classified?
tag classified
  id 2
  enum 0 no
  enum 1 secret
  enum 2 top
  default no
;

# Clearance flags (a bit like groups)
tag clearance
  id 1
  default 0
  flag 0 staging
  flag 1 production
  flag 2 financial
  flag 3 security
  flag 4 executive
;

# If one party is classified, require at least one overlapping clearance bit
break
  not tor classified 0
  and tand clearance 0
;
```
Initially members will be assigned a default classification of 0 ("no"). These can freely communicate since the bitwise OR of their classified tags will be zero. Neither member possesses a classification requirement.
To restrict access to a member, set its classified tag to secret or top. (In this example there is no difference, but two levels are included in case you want to implement some kind of more detailed segmentation based on these.) Now the first match (`not tor classified 0`) will be true and the packet will be dropped unless the two communicating members have at least one clearance bit in common (`tand clearance 0)`.
## Client Configuration
ZeroTier One is a service that can run on laptops, desktops, servers, virtual machines, and containers to provide virtual network connectivity through a virtual network port much like a VPN client. It can also act as a network controller and as a federated root server.
[Binary packages are available on the ZeroTier site](https://www.zerotier.com/download.shtml) and source code is found on [GitHub](https://github.com/zerotier/ZeroTierOne).
After the service is installed and started, networks can be joined using their 16-digit network IDs. Each network appears as a virtual "tap" network port on your system that behaves just like an ordinary Ethernet port.
### ZeroTier runs as Admin
There are two separate ZeroTier apps on PCs.
There is a ZeroTier system service. It runs as Admin. It does all the networking. There is a user interface tray app. It does not run as Admin. It talks to the service to join and leave networks.
For the app to talk to the service, it needs access to the secret token. If the user running the UI app can't access the token, it can't talk the service, so they can't leave or join networks or change other settings.
During install, the token is copied to a location that the installing user can access.
### Configuration Files
#### System
The ZeroTier One service keeps its configuration and state information in its working directory. The working directory location is:
- Windows: `C:\ProgramData\ZeroTier\One`
- macOS: `/Library/Application Support/ZeroTier/One`
- Linux: `/var/lib/zerotier-one`
- FreeBSD/OpenBSD: `/var/db/zerotier-one`

##### Network Specific Configuration
See `$WORKING/networks.d` in the working directory
##### `&lt;network-id&gt;.conf` is a binary file. You can't edit it by hand
If you place an empty file named `&lt;network-id&gt;.conf` in `networks.d`, ZeroTier will join that network when it starts.
##### `&lt;network-id&gt;.local.conf` text file with the netwrok's setting
The contents look like this:
```
allowManaged=1
allowGlobal=0
allowDefault=0
allowDNS=0
```
These settings apply to the specific ZeroTier network.
Here is a summary of their meanings:
- Allow Managed. Default Yes. Allow ZeroTier to set IP Addresses and Routes ( [local/private](https://en.wikipedia.org/wiki/Private_network) ranges only)
- Allow Global. Default No. Allow ZeroTier to set Global/Public/Not-Private range IPs and Routes.
- Allow Default. Default No. Allow ZeroTier to set the Default Route on the system. See [Full Tunnel Mode](https://zerotier.atlassian.net/wiki/spaces/SD/pages/7110693/Overriding+Default+Route+Full+Tunnel+Mode).
- Allow DNS. Default No. Allow ZeroTier to set DNS servers.

ZeroTier will use these settings when it starts. If you change these settings from the UI or zerotier-cli, the file will update. If you edit the file directly, you need to restart the service.
#### User
Some user specific settings may be stored in the user's path:
- `C:\Users\<User>\AppData\Local\ZeroTier` (Windows)
- `~/Library/Application\ Support/ZeroTier` (macOS)
### Local Configuration OPtions
A file called local.conf in the ZeroTier home folder contains configuration options that apply to the local node. It can be used to set up trusted paths, blacklist physical paths, set up physical path hints for certain nodes, and define trusted upstream devices (federated roots). Most of the time, you don't need to change any of these settings.
In a large deployment it can be deployed using a tool like Puppet, Chef, SaltStack, etc. to set a uniform configuration across systems.
`local.conf` is a JSON format file that can also be edited and rewritten by ZeroTier One itself, so ensure that proper JSON formatting is used. Paste your JSON into a JSON tool before saving your configuration file.
Settings available in `local.conf` (this is not valid JSON, and JSON does not allow comments):
```json
{
    "physical": { /* Settings that apply to physical L2/L3 network paths. */
        "NETWORK/bits": { /* Network e.g. 10.0.0.0/24 or fd00::/32 */
            "blacklist": true|false, /* If true, blacklist this path for all ZeroTier traffic */
            "trustedPathId": 0|!0 /* If present and nonzero, define this as a trusted path (see below) */
        } /* ,... additional networks */
    },
    "virtual": { /* Settings applied to ZeroTier virtual network devices (VL1) */
        "##########": { /* 10-digit ZeroTier address */
            "try": [ "IP/port"/*,...*/ ], /* Hints on where to reach this peer if no upstream/roots are online */
            "blacklist": [ "NETWORK/bits"/*,...*/ ] /* Blacklist a physical path for only this peer. */
        }
    },
    "settings": { /* Other global settings */
        "primaryPort": 0-65535, /* If set, override default port of 9993 and any command line port */
        "portMappingEnabled": true|false, /* If true (the default), try to use uPnP or NAT-PMP to map ports */
        "softwareUpdate": "apply"|"download"|"disable", /* Automatically apply updates, just download, or disable built-in software updates */
        "softwareUpdateChannel": "release"|"beta", /* Software update channel */
        "softwareUpdateDist": true|false, /* If true, distribute software updates (only really useful to ZeroTier, Inc. itself, default is false) */
        "interfacePrefixBlacklist": [ "XXX",... ], /* Array of interface name prefixes (e.g. eth for eth#) to blacklist for ZT traffic */
        "allowManagementFrom": [ "NETWORK/bits" ]|null, /* If non-NULL, allow JSON/HTTP management from this IP network. Default is 127.0.0.1 only. */
        "allowTcpFallbackRelay": true|false /* Allow or disallow establishment of TCP relay connections (true by default) */
    }
}
```
- **trustedPathld**:A trusted path is a physical network over which encryption and authentication are not required. This provides a performance boost but sacrifices all ZeroTier's security features when communicating over this path. Only use this feature if you know what you are doing and really need the performance! To set up a trusted path, all devices on the same trusted physical network must have the same trusted path ID. Trusted path IDs are arbitrary unsigned 64-bit integers. These are not secrets. The security of a trusted path depends on its physical configuration. Take special care that any firewalls at its boundaries do not allow traffic in our out with IPs overlapping the trusted network range.

An example local.conf:
```json
{
    "physical": {
        "10.0.0.0/24": {
            "blacklist": true
        },
        "10.10.10.0/24": {
            "trustedPathId": 101010024
        },
    },
    "virtual": {
        "feedbeef12": {
            "role": "UPSTREAM",
            "try": [ "10.10.20.1/9993" ],
            "blacklist": [ "192.168.0.0/24" ]
        }
    },
    "settings": {
        "softwareUpdate": "apply",
        "softwareUpdateChannel": "release"
    }
}
```
### `authtoken` location
The installer copies `authtoken.secret` to the installing user's [path](https://docs.zerotier.com/config#user).
To control the ZeroTier system service, you need the token. If you don't have the token, you can't leave or join networks, for example. The UI app uses the token to control the system service. The cli, `zerotier-cli`, uses it also.
The token is called `authtoken.secret` and it is stored in the ZeroTier working directory. You need to be Admin to access the working directory.
The user's copy of `authtoken.secret` is in:
- C:\Users\<User>\AppData\Local\ZeroTier (Windows)
- ~/Library/Application\ Support/ZeroTier (macOS)

If you don't want a user to control ZeroTier, don't give them this file and don't give them access to the working directory.
## What is a Network Controller?
> **INFO**
> [Set up your own Controller](https://docs.zerotier.com/controller)

Every ZeroTier virtual network has a network controller responsible for admitting members to the network, issuing certificates, and issuing default configuration information.
This is our reference controller implementation and is almost the same as the one we use to power our own hosted services at [my.zerotier.com](https://my.zerotier.com/). The only difference is the database backend used.
Controller data is stored in JSON format under `controller.d` in the ZeroTier working directory. It can be copied, rsync'd, placed in `git`, etc. The files under `controller.d` should not be modified in place while the controller is running or data loss may result, and if they are edited directly take care not to save corrupt JSON since that can also lead to data loss when the controller is restarted. Going through the API is strongly preferred to directly modifying these files.
See the API section below for information about controlling the controller.
### Scalability and Reliability
Controllers can in theory host up to 2^24 networks and serve many millions of devices (or more), but we recommend spreading large numbers of networks across many controllers for load balancing and fault tolerance reasons. Since the controller uses the filesystem as its data store we recommend fast filesystems and fast SSD drives for heavily loaded controllers.
Since ZeroTier nodes are mobile and do not need static IPs, implementing high availability fail-over for controllers is easy. Just replicate their working directories from master to backup and have something automatically fire up the backup if the master goes down. Modern orchestration tools like Nomad and Kubernetes can be of help here.
### Dockerizing Controllers
ZeroTier network controllers can easily be run in Docker or other container systems. Since containers do not need to actually join networks, extra privilege options like "--device=/dev/net/tun --privileged" are not needed. You'll just need to map the local JSON API port of the running controller and allow it to access the Internet (over UDP/9993 at a minimum) so things can reach and query it.
### Upgrading from Older (1.1.14 or earlier) Version
Older versions of this code used a SQLite database instead of in-filesystem JSON. A migration utility called `migrate-sqlite` is included here and must be used to migrate this data to the new format. If the controller is started with an old `controller.db` in its working directory it will terminate after printing an error to stderr. This is done to prevent "surprises" for those running DIY controllers using the old code.
The migration tool is written in nodeJS and can be used like this:
```bash
cd migrate-sqlite
npm install
node migrate.js </path/to/controller.db> </path/to/controller.d>
```
### Network Controller API
The controller API is hosted via the same JSON API endpoint that ZeroTier One uses for local control (usually at 127.0.0.1 port 9993). All controller options are routed under the `/controller` base path.
The controller microservice itself does not implement any fine-grained access control. Access control is via the ZeroTier control interface itself and `authtoken.secret`. This can be sent as the `X-ZT1-Auth` HTTP header field or appended to the URL as `?auth=<token>`. Take care when doing the latter that request URLs are not being logged.
While networks with any valid ID can be added to the controller's database, it will only actually work to control networks whose first 10 hex digits correspond with the network controller's ZeroTier ID. See [Network Identifiers and Controllers](https://docs.zerotier.com/protocol#nwid).
The controller JSON API is very sensitive about types. Integers must be integers and strings strings, etc. Incorrect types may be ignored, set to default values, or set to undefined values.
Full documentation of the Controller API can be found on our [documentation site](https://docs.zerotier.com/service/v1#tag/controller)
### Prometheus Metrics
Controller specific metrics are available from the `/metrics` endpoint.
**Metric Name**|**Type**|**Description**
----|----|----
controller_network_count|Gauge|number of networks the controller is serving
controller_member_count|Gauge|number of network members the controller is serving
controller_network_change_count|Counter|number of times a network configuration is changed
controller_member_change_count|Counter|number of times a network member configuration is changed
controller_member_auth_count|Counter|number of network member auths
controller_member_deauth_count|Counter|number of network member deauths

# Central
## Create a Network
A ZeroTier network is essentially a secure Local Area Network (LAN) that you can use anywhere in the world. Let's make one and connect two devices over ZeroTier.
We'll use `ping` to test the connection. Any two devices that can run ZeroTier will do: laptop, phone, virtual machine, etc…
Both devices can be at the same location, on the same physical network. If you move one to a cafe or to your office, it should still just work.
The rough outline is:
- Create a ZeroTier network
- Join the network from two devices
- `ping` one device from the other over the ZeroTier network

This should take about 5 minutes.
> **Results Preview**
> Here is a summary of the results of this tutorial, if you're a networking person.
> If this doesn't mean anything to you, that's OK. We'll get there.
> Each zerotier network you join creates a network interface on your device. It's like adding another Ethernet port to your computer.
> ```
> node1# ip -o a
> 1: lo    inet 127.0.0.1/8 scope host lo\       valid_lft forever preferred_lft forever
> 2: eth0    inet 192.168.182.201/24 brd 192.168.182.255 scope global dynamic noprefixroute eth0\       valid_lft 3277sec preferred_lft 2827sec
> 9: zt3jn2z57r    inet 10.2.0.11/23 brd 10.2.1.255 scope global zt3jn2z57r\       valid_lft forever preferred_lft forever
> ```
> ```
> node2# ip -o a
> 1: lo    inet 127.0.0.1/8 scope host lo\       valid_lft forever preferred_lft forever
> 2: eth0    inet 192.168.182.202/24 brd 192.168.182.255 scope global dynamic noprefixroute eth0\       valid_lft 3277sec preferred_lft 2827sec
> 9: zt3jn2z57r    inet 10.2.0.12/23 brd 10.2.1.255 scope global zt3jn2z57r\       valid_lft forever preferred_lft forever
> ```
> ```
> node1# ping -c 3 10.2.0.12
> PING 10.2.0.2 (10.2.0.12) 56(84) bytes of data.
> 64 bytes from 10.2.0.12: icmp_seq=1 ttl=64 time=5.66 ms
> 64 bytes from 10.2.0.12: icmp_seq=2 ttl=64 time=6.62 ms
> 64 bytes from 10.2.0.12: icmp_seq=3 ttl=64 time=8.50 ms
> 
> --- 10.2.0.12 ping statistics ---
> 3 packets transmitted, 3 received, 0% packet loss, time 2004ms
> ```

### Create your first ZeroTier network
#### Create an account
> **NOTE**
> It's free, no credit card is required.

- Go to [my.zerotier.com](https://my.zerotier.com/) and create an account.
#### Create a network
- Make sure you're on the "Networks" tab of my.zerotier.com
![20240316155636](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240316155636.png)
- Click the **Create A Network** button.

This creates a virtual network with a random ID and a random name. We got "fervent_smathers" and d5e04297a16fa690 here.
![20240316155855](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240316155855.png)
- Click anywhere on the network to go to the details page for this network.

See the Network Settings panel:
![20240316155931](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240316155931.png)
We don't need to change any settings, but we can change the name of the network to personalize it.
- Change "fervent_smathers" to "my cool network" or whatever you like.
- Collapse the Settings panel. Click on the word "Settings" at the top of the panel.

You don't need to change any other settings.
- See the Network Members panel:
![20240316160036](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240316160036.png)
It should say "No devices have joined this network".
- Leave this browser tab open. We'll look at it again later.

### Setup the ZeroTier app
#### Download and install ZeroTier
For mobile devices, use the app store.
- Go to [zerotier.com/download](https://www.zerotier.com/download) in a different tab of your browser.
- Run the installer

The ZeroTier client should now running on your device.
#### Join your first ZeroTier netwrok
We need to tell the client to "join" the virtual network we just created.
- Copy the Network ID of the network from my.zerotier.com This is the long number that looks like like: d5e04297a16fa690
- Paste the Network ID into the "join" command on your device
On macOS and Windows, there is a menubar/tray app. Select "join" from the menu.
> **NOTE**
> Every running instance of ZeroTier has a unique address. It's the 10 digit "Address" in the app, or `zerotier-cli` info command.
> ZeroTier addresses are a very secure method of unique identification.

### Authorize your device on your network
At this point, your client should say "Access Denied." A device can't talk on your network unless you allow it, even if someone discovers the network's ID.
#### Authorize your device
- Go to the Members panel that we left open on my.zerotier.com
- Your node that just "joined" should appear here.
- The "Address" should match the address in your client.
- Click the "Auth?" check box for it.
- Give it a name. Type something like "laptop" or "bob" into the (short name) input.
![20240316160617](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240316160617.png)

#### Confirm authorization
Back on your computer, your client should now say "OK" instead of "ACCESS DENIED" and it should show your custom "my cool network" name.
Now you have 1 member on your network. A network with 1 member can't do much.
### Repeat with another device
We need to have 2 devices connected to the same ZeroTier network.
- Repeat the join and authorize steps with your second device.

### Test connectivity
Now you have two authorized nodes on your network. They should be able to talk over ZeroTier.
Your Network Members section should look something like this:
![20240316160752](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240316160752.png)
The "Managed IPs" will be different on your network.
We're going to test with `ping`. It's the only program that we can think of that exists by default on every operating system.
This is a command line program, but don't worry: You can do it.
#### Gotcha: Windows blocks ping
~~Windows by default doesn't respond to pings. If you try to ping a Windows computer from a different computer, it won't work. You can enable ping.~~
ZeroTier automatically enables ping on your ZeroTier network adapter now. You can probably skip this step!
> **How to enable ping on Windows**
> - Search for Windows Firewall in the Start Menu, and click to open it.
> - Click Advanced Settings on the left.
> - From the left pane of the resulting window, click Inbound Rules.
> - In the right pane, find the rules titled File and Printer Sharing (Echo Request - ICMPv4-In).
> - Right-click each rule and choose Enable Rule.
> Here is a [tutorial by Microsoft](https://learn.microsoft.com/en-us/windows/security/threat-protection/windows-firewall/create-an-inbound-icmp-rule)

#### Open the command line
- Open the command line on your computer

##### macOS
- Use Spotlight (cmd-space) to search for Terminal

[Apple's Instructions](https://support.apple.com/guide/terminal/open-or-quit-terminal-apd5265185d-f365-44cb-8b09-71a064a42125/mac)
##### Windows
- Search for "powershell" and open it

[Microsoft's Instructions](https://learn.microsoft.com/en-us/powershell/scripting/windows-powershell/starting-windows-powershell?view=powershell-7.3)
##### Linux
- It's different on every flavor of Linux. You'll have to search duckduckgo for "open terminal ubuntu" or similar.

##### Mobile
Mobile operating systems don't have a command line. You can download a "ping" app from your app store if you want.
Or `ping` your phone from your desktop computer.
Try switching your phone from wifi to cell and back. It may take about a minute, but ZeroTier should automatically keep the connection working.
#### Find the ZeroTier IP Addresses of your devices
![20240316161618](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240316161618.png)
#### Try the ping command
Back in the Command Line / Terminal that you just opened:
type `ping -c 5 $ZEROTIER_IP_ADDRESS` `<enter>` into your command line.
A successful `ping`:
```cmd
% ping -c 5 172.22.217.93
PING 172.22.217.93 (172.22.217.93): 56 data bytes
64 bytes from 172.22.217.93: icmp_seq=0 ttl=64 time=22.362 ms
64 bytes from 172.22.217.93: icmp_seq=1 ttl=64 time=10.157 ms
64 bytes from 172.22.217.93: icmp_seq=2 ttl=64 time=9.414 ms
64 bytes from 172.22.217.93: icmp_seq=3 ttl=64 time=9.019 ms
64 bytes from 172.22.217.93: icmp_seq=4 ttl=64 time=9.180 ms

--- 172.22.217.93 ping statistics ---
5 packets transmitted, 5 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 9.019/12.026/22.362/5.182 ms
```
Try it with both ZeroTier Managed addresses on your network.
One of them is the same device you're on, so you're pinging yourself. Pinging the other device might be a little more interesting.
> **INFO**
> If something goes wrong you might see something like:
> ```
> % ping -c 5 172.22.217.92
> PING 172.22.217.92 (172.22.217.92): 56 data bytes
> Request timeout for icmp_seq 0
> Request timeout for icmp_seq 1
> Request timeout for icmp_seq 2
> Request timeout for icmp_seq 3
> 
> --- 172.22.217.92 ping statistics ---
> 5 packets transmitted, 0 packets received, 100.0% packet loss
> ```

or

> ```
> ping -c 5 192.168.123.234
> PING 192.168.123.234 (192.168.123.234): 56 data bytes
> 92 bytes from 192.168.82.1: Destination Port Unreachable
> Vr HL TOS  Len   ID Flg  off TTL Pro  cks      Src      Dst
>  4  5  00 5400 56e7   0 0000  3f  01 d4ad 192.168.82.217  192.168.123.234
> ```
> There may just be a typo in the IP address. Double check that your device is authorized at my.zerotier.com
> Contact us on the [discussion forum](https://discuss.zerotier.com/) and see the [troubleshooting section](https://docs.zerotier.com/faq) if you get stuck.

### Conclusion
`ping` doesn't accomplish anything, but it does tell us ZeroTier is working. It's useful to know about for troubleshooting networks, not just ZeroTier networks.
Visit the [discussion forum](https://discuss.zerotier.com/) to talk about your use-cases or if you get stuck.
#### Now, use ZeroTier to do something you want to do
#### Some popular uses
- Windows Remote Desktop
- ssh (try [mosh](https://mosh.org/))
- Private Gaming LAN
- Access the web interfaces of your home lab
- Build your own [VPN](https://zerotier.atlassian.net/wiki/spaces/SD/pages/7110693/Overriding+Default+Route+Full+Tunnel+Mode)
- Route to a [remote subnet](https://zerotier.atlassian.net/wiki/spaces/SD/pages/224395274/Route+between+ZeroTier+and+Physical+Networks)
- Route to a [Docker network](https://zerotier.atlassian.net/wiki/spaces/SD/pages/7274520/Using+NDP+Emulated+6PLANE+Addressing+With+Docker)
- Add [dns](https://docs.zerotier.com/dns) to your network

#### Join multiple networks
A node can join many networks at once. Make sure they don't use the same subnet!
You can have a `home` network, a `friends` network, and a `work` network, for example.
They don't all need to be networks that you've created. You can join other people's networks.
#### Check out the Whitepaper
For more info on the cryptography and protocol, see the: [Design Whitepaper](https://docs.zerotier.com/protocol)

## Central FAQ
### Can I move or transfer my network(s) to another user?
When you join an [Organization](https://docs.zerotier.com/organizations), all of your networks are transferred to the Organization.
Other than that, there is no way to transfer individual networks between accounts.
#### To change your Organization Owner
What you can do is change the email address of your account to a different address. From the account page, select "Manage Account" then "Personal Info". From there you can change the email address. After changing, please log out and log back in.
Note: This email address must not already have a ZeroTier Central account.
Many users prefer to use something like "billing" or "admin" `@example.com` for their organization owner email account and then invite `alice@example.com` and `bob@example.com` as network admins.
> **TIP**
> You can avoid this process by using a shared email address or distribution list (example: `zerotier@example.com`) instead of a personal address for the account that manages your organization.

### Can I make my subnet bigger?
Yes. Your network's existing members will keep their existing IPs.
1. Create a new, larger Managed Route.
2. Delete the old, smaller Managed Route.
3. Change IPv4 Auto-Assign from Easy to Advanced
4. Delete the existing range
5. Add the new, bigger range

For example, if your network was on the Easy Mode `192.168.195.*` (/24)
- Change the managed route to 192.168.194.0/23
- Change the auto assign range to: 192.168.194.1 to 192.168.195.254

If you change to a completely different, non-overlapping subnet, your network's members will get new IPs in the new range, and keep their old IPs. You will have to delete the old IPs from each member if you don't like to see them in the list.
For example, if you change from `192.168.195.*` to `10.244.*.*`, then the member nodes will have IPs in both ranges, like: 192.16.195.1 and 10.244.1.1
IPs don't actually get applied on the operating system unless the network has a matching managed route.
### How are nodes counted against my subscription?
If the same node is a member of multiple networks, it is counted only once.
Only authorized nodes are counted. You don't need to hide or delete nodes for billing purposes. De-authorized nodes aren't counted.

## SSO/OIDC
### ZeroTier Central configuration
> **NOTE**
> SSO is currently only supported on desktop operating systems such as macOS and Windows. Support for iOS and Android, and better support for authenticating via the command line is still to come.

#### Update clients
- Download and install ZeroTier 1.10.3 or greater on clients that will use SSO.
[https://www.zerotier.com/download/](https://www.zerotier.com/download/)

#### Configure SSO in ZeroTier Central
Visit [https://my.zerotier.com/account](https://my.zerotier.com/account) and complete the SSO configuration toward the bottom of the page. You will need your sso provider's Issuer URL as well as a Client ID.
![20240318091806](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318091806.png)
You can configure multiple OIDC clients for your organization, but only one may be used at a time on an individual network.
#### Configure SSO on individual networks
If you enable this on an existing network, you may accidentally block existing users. Please practice on a test network.
![20240318091853](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318091853.png)
There are three login modes for SSO enabled networks:
1. Standard - If the user can successfully authenticate to your OIDC provider, they will be allowed access to the ZeroTier network
2. [Email Based Access](https://docs.zerotier.com/sso#email-based-network-access) - The user is allowed to access the network if and only if their email address is in the email list provided by the network administrator.
3. [Group/Role Based Access](https://docs.zerotier.com/sso#email-based-network-access) - The user is allowed to access the network if and only if they are assigned one of the proper roles by the OIDC server.
#### Exclude specific devices from SSO requirements
This is useful for routers, servers, embedded devices, etc… You can do this from the wrench icon in the Members list.
![20240318092456](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318092456.png)
### SSO provider configuration
- SSO Provider **must** support [PKCE](https://oauth.net/2/pkce/)
- Requires the following scopes:
  - `openid`
  - `profile`
  - `email`
  - `offline_access`
- Configure the callback URL to `http://localhost:9993/sso`

### Provider Specific Configuration Notes
#### Auth0
Please ensure the following fields are set on your Auth0 application config:
- Application Type: Native
- Token Endpoint Authentication Method: None
- Allowed Callback URL: [http://localhost:9993/sso](http://localhost:9993/sso)
- Under Advanced Settings -> Grant Types, ensure Authorization Code, and Refresh Token are selected.

> **NOTE**
> The OIDC spec is picky about the Issuer URL you enter. It must match what the server configuration metadata endpoint returns.
> In the case of Auth0 specifically, Your Issuer URL MUST end with a `/`. For example, in Auth0's application configuration, show's just the fully qualified domain name. What must be entered in the Issuer field in ZeroTier Central is: `https://your-domain-id.auth0.com/`

#### Authelia
[Authelia](https://www.authelia.com/) is a self hosted SSO solution. ZeroTier uses PKCE, so the field `secret` must be an empty string and `public` must be true.
> **NOTE**
> Use of Authelia requires ZeroTierOne version 1.10.1 or greater. There is an incompatibility between the two in the 1.10.0 release.

Example client configuration:
```
    clients:
        ## The ID is the OpenID Connect ClientID which is used to link an application to a configuration.
      - id: authelia-sso-client

        ## The description to show to users when they end up on the consent screen. Defaults to the ID above.
        description: zerotier

        ## The client secret is a shared secret between Authelia and the consumer of this client.
        secret: ""

        ## Sector Identifiers are occasionally used to generate pairwise subject identifiers. In most cases this is not
        ## necessary. Read the documentation for more information.
        ## The subject identifier must be the host component of a URL, which is a domain name with an optional port.
        # sector_identifier: example.com

        ## Sets the client to public. This should typically not be set, please see the documentation for usage.
        public: true

        ## The policy to require for this client; one_factor or two_factor.
        authorization_policy: one_factor

        ## By default users cannot remember pre-configured consents. Setting this value to a period of time using a
        ## duration notation will enable users to remember consent for this client. The time configured is the amount
        ## of time the pre-configured consent is valid for granting new authorizations to the user.
        # pre_configured_consent_duration:

        ## Audience this client is allowed to request.
        audience: [
          "authelia-test-client"
        ]

        ## Scopes this client is allowed to request.
        scopes:
          - openid
          - groups
          - email
          - profile
          - offline_access

        ## Redirect URI's specifies a list of valid case-sensitive callbacks for this client.
        redirect_uris:
          - http://localhost:9993/sso

        ## Grant Types configures which grants this client can obtain.
        ## It's not recommended to define this unless you know what you're doing.
        grant_types:
          - refresh_token
          - authorization_code

        ## Response Types configures which responses this client can be sent.
        ## It's not recommended to define this unless you know what you're doing.
        response_types:
          - code
          - id_token

        ## Response Modes configures which response modes this client supports.
        response_modes:
          - form_post
          - query
          - fragment

        ## The algorithm used to sign userinfo endpoint responses for this client, either none or RS256.
        userinfo_signing_algorithm: none
```
#### Azure AD
Navigate to your directory in the Azure portal, and select "App Registrations" in the Manage column.
Click "New Registration"
Set the Name of the application. e.g. "ZeroTier Central SSO"
Under Redirect URI:
Platform: Public client/native (mobile & desktop)
Set the Redirect URI to `http://localhost:9993/sso`
Do not use a trailing `/` when you enter the "issuer" url in Central.
#### Google Workspace
Google OAuth2/OIDC is not supported as Google does not support PKCE clients at this time. You can, however, use [Keycloak as a SAML Identity Broker](http://localhost:3000/central/sso#keycloak-as-a-saml-identity-broker) with Google Workspace.
#### Keycloak
Log into your Keycloak administration console, go to the Client configuration and create a new client. Configuration is fairly straightforward, with the following requirements:

- Client Protocol: openid-connect
- Access Type: public
- Standard Flow Enabled: ON
- Root URL: [https://my.zerotier.com](https://my.zerotier.com/)
- Valid Redirect URLS
  - [https://my.zerotier.com/](https://my.zerotier.com/)*
  - [http://localhost/sso](http://localhost/sso)
- Admin URL: [https://my.zerotier.com](https://my.zerotier.com/)
- Web Origins: *
See [here](https://www.keycloak.org/docs/latest/server_admin/index.html#assembly-managing-clients_server_administration_guide) for full documentation for configuring OpenID Connect clients with Keycloak.
##### Keycloak as a SAML Identity Broker
If you have a SAML provider, but not an OpenID Connect provider, [Keycloak](https://www.keycloak.org/) can also be used to bridge the gap. On your keycloak Admin page, go to Identity Providers. From the dropdown, select `SAML v2.0` and create the connection to your SAML provider. Combined with the general Keycloak OIDC client settings above, you now have an OIDC server that authenticates against your SAML provider.
#### Okta
- Application Type: Native
- Token Endpoint Authentication Method: None
- Allowed Callback URL: http://localhost:9993/sso
- Under Advanced Settings -> Grant Types, ensure Implicit, Authorization Code, and Refresh Token are selected.

#### OneLogin
> **NOTE**
> OneLogin requires ZeroTier One v1.10.3+

Log in to your OneLogin admin console. Select "Custom Connectors" from the "Applications" menu. Hit the "New Connector" button. Name your connector, set Sign on method to OpenID Connect, and set the Redirect URI to `https://localhost:9993/sso`. Finally, back on the on Custom Connectors page, hit the "Add App to Connector" link. Adjust the description & logo settings as you see fit, and then save.
Once the above steps are complete, go to the SSO tab for your new OneLogin Application. Set "Application Type" to "Native", and "Token Endpoint" to "None (PKCE)". You'll also find the required Client ID and Issuer URLs to enter into [https://my.zerotier.com/account](https://my.zerotier.com/account).
### Customizing the Final SSO Flow Page
If you wish, you can customize the final view of the sso login process. Create the file `$ZEROTIER_HOME/sso-auth.template.html`.
Note: Any CSS or images must be hosted externally, or placed within the single HTML page itself.
You may customize the page to look however you wish. At this time there are only two template values set by zerotier:
- `networkId`
- `messageText`
Templates must be valid HTML, and the template values must be placed inside `{{ ... }}` blocks like so:
```
{{ networkId }}
{{ messageText }}
```
You may react to errors via the `isError` variable:
```
{% if isError %}
<span style="color: red;">{{ messageText }}</span>
{% else %}
{{ messageText }}
{% endif %}
```
### Email Based Network Access
With Email based network access, a network administrator can limit access to the network to only users whose email addresses are present in the list specified by the network administrator.
![20240318093942](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318093942.png)
### Role Based Network Access
Configuring Role Based Access controls is different across all of the different OIDC providers available. We'll give examples for many of the large ones.
Role/group names are up to the network administrator. Simply add one or more role name to your network configuration, and users will be required to have at least one of those roles assigned in order to access the network.
#### Auth0
Auth0 requires multiple steps to configure.
##### Step 1: Create a Role and Assign Users
Log into your auth0.com management console. Go to User Management and select roles. Hit the `+ Create Role` button in the upper right corner. Give your new role a name & description of your choosing.
![20240318094126](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318094126.png)
Once the new role is created, go to the Users tab. There you will be able to add the users you wish to have access to the role.
##### Step 2: Attach Roles to ID and Access Tokens
In order for ZeroTier to be able to read the roles a user has been assigned, they must be added to the ID and Access tokens, and mapped to the correct name. In the Auth0.com dashboard, go to Actions, and select Flows. Click the Login flow. On the right side of the screen, hit the + next to `Add Action` and select `Build Custom`. Give the action a name such as "Attach ZeroTier Roles" with Trigger set to "Login/Post Login" and Runtime set to "Node 16" (or whatever the latest default is).
Next you will get a screen with a code editor. Delete everything in the buffer and add the following:
```
exports.onExecutePostLogin = async (event, api) => {
    const namespace = "https://my.zerotier.com";
    if (event.authorization) {
        api.idToken.setCustomClaim(`${namespace}/roles`, event.authorization.roles);
        api.accessToken.setCustomClaim(`${namespace}/roles`, event.authorization.roles);
    }
}
```
> **NOTE**
> Auth0 requires roles to be in a namespaced name. To properly integrate with ZeroTier Central, the final claim name set on the token via the script above **must** be `https://my.zerotier.com/roles`. Please enter the login script exactly as shown above to reduce the chances of errors.

Once it is entered, hit `Deploy` on the upper right hand side of the screen.
Hit the `<- Back to flow` link, which will take you back to the Login Flow graph. Under Add Action, select Custom. You'll find your new action listed there. Drag it between `Start` and `Complete` in the login flow and click Apply. When finished, your flow will look something like this:
![20240318094440](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318094440.png)
Your users' assigned roles will now be attached to the tokens required to authorize a user on your ZeroTier networks.
#### Azure AD
https://learn.microsoft.com/en-us/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps
#### Step 1: Create an App Role
In the Azure portal, go to Azure Active Directory and select App Registrations. Select the app you've created for integration with ZeroTier Central. In the Manage menu, select App Roles, and then create a new app role and hit Apply
![20240318094647](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318094647.png)
The `Value` field is what you use as the role name in the network configuration on https://my.zerotier.com
#### Step 2: Assign App Role
Go back to Azure Active Directory in the Azure portal. Select Enterprise Applications from the list of options on the side of the screen. Select the app to be used for ZeroTier Central integration.
Select `Users and Groups` from the Manage menu. Select all of the users you want to assign the App Role and select `Edit Assignment`. Click on the area of the screen that says "Select a Role", then find your newly created app role from step 1, click it and hit the `Select` button.
#### Okta
Okta doesn't have the concept of Roles, but it does have Groups that can be used for the same purpose when authenticating to a ZeroTier network.
> **NOTE**
> Okta role/group based access requires ZeroTier One version 1.10.3+

##### Step 1: Create a Group and Assign Users
From your Okta administrator dashboard, go to Directory and select Groups. Create a group with the name of your choice. Once created, assign people to the group that you want to have access to your ZeroTier network.
##### Step 2: Attach Groups to Tokens
In the Okta administrator dashboard, go to Applications and open the configuration for the Okta Application you've configured for use with ZeroTier. Click the 'Edit' button in the "OpenID Connect ID Token" box. Set "Group claim type" to "Filter", and set the three fields for "Group claim filter" to `roles`, `Matches regex`, and `.*`.
![20240318094935](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318094935.png)
It is possible to limit the groups to only certain groups. See Okta's [group filter documentation](https://developer.okta.com/docs/reference/okta-expression-language/#group-functions) for more information.
#### OneLogin by OneIdentity
Coming Soon
## Add new SSO Seats
If you are already on the Pro plan, to increase your Node, Admin, or SSO entitlements:
Go to the Account page
Click Modify Plan
![20240318160443](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318160443.png)
You will see your current subscription level
![20240318160518](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318160518.png)
Adjust the levels to your new needs
![20240318160555](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318160555.png)
Click checkout
The system will figure out any needed credits or proration.
![20240318160627](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318160627.png)
Click Update Subscription to finish to accept the changes
This will charge your card if needed and update your account to the new subscription levels.
## Authentication, Login, and Email issues
### Not receiving emails from us?
If you are not receiving password reset emails, first check your Spam/Junk Mail folder.
If it's not in your Spam/Junk Mail folder, contact your email administrator to ensure emails from [my@zerotier.com](my@zerotier.com) are not being blocked at the server level.
### Changing Your Email Address
Log into ZeroTier Central at https://my.zerotier.com. Once logged in, there's a "Manage Account" link on the Account page. Click that and then "Personal Info" and you'll be taken to a form where you can change your email address.
### Invalid Authenticator Code
This can be caused by the time being wrong on your computer or phone.
### Lost Authenticator Code
Use the Password Reset flow. Click "forgot password" on the login screen.
> **HISTORY**
> If you have not logged in to your account since 2019, accounts were migrated to a new system in 2019. Your networks are still there!
> Please try to create an account with the same email address you used before.

## Organizations
You've subscribed to my.zerotier.com and want your coworkers to also have the benefits of a Pro account.
Add them to your Organization by going to my.zerotier.com/account
Create an invite by typing in an email address
We will email them the link. You can also paste it into your company chat.
![20240318161100](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318161100.png)
Once they accept the invite, they can be added as Admins to individual networks.
![20240318161120](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318161120.png)
The invited User will see:
![20240318161207](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318161207.png)
![20240318161236](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318161236.png)
### Removing Admins from the Organization
You can click the "remove" button next to their name in the list of admins. They will lose Admin access to the organizations networks.
### Change the Organization owner
There is currently no straight-forward way to do this, and ZeroTier support can't change your organization owner via email/ticket because that would be insecure.
The secure way to do this is to change the login of the organization owner:
- Go to https://my.zerotier.com/account
- click Manage Account
- Click Personal Info
- Change the email address to the new organization owner's email
- Log out and back in
- You may want to change the password as well

Consider using a general email address like "[billing@example.com](billing@example.com)" or "[zerotier@example.com](zerotier@example.com)" when you sign up, or when you change org owners.
#### Can I move or transfer my network(s) to another user?
The only process that moves networks is joining an organization. When you join, the organization absorbs all your existing networks.
If your networks are small; It may be faster to start a new account and new networks.
## Add Network Admin
Professional subscriptions have a shared administration feature.
![20240318161858](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318161858.png)
Toward the bottom of a Network's page. There is a section titled "Administrators"
A user must be a member of your Organization to become a Network Administrator.
### Permissions
You can allow another my.zerotier.com user permission to
- **Read**. They can look at the network but not change anything
- **Authorize**. Allow them to Authorize new members, but not make other changes.
- **Modify**. Allow them to change any settings on this Network.
- **Delete**. Allow them to delete the network.
## Pricing
> ** INFO**
> See our [pricing page](https://www.zerotier.com/pricing/) for current plans.

### Do my networks still work properly if I'm out of compliance?
Yes, everything works fine for the old network members you've set up. Many people use ZeroTier for critical infrastructure and we make it our highest priority to ensure your service isn't interrupted in relation to the new pricing plan. Our enforcement just blocks your ability to add new members to your network until you're in compliance with the new plan.
> **HISTORY**
> On Thursday, 9 June 2022 we updated our product plans and the Free Basic plan has changed. Here are some things to know:
> We have changed the number of nodes available on the Free Basic plan from 50 to 25.
> We have also changed how we count them. We now count total nodes authorized across all the networks you have, not individual members on networks, so while the free limit is now 25, these nodes can now be authorized on multiple networks. Networks are free. For some users with multiple networks, this may amount to a limit increase.
> If you are over the limit, your networks and the nodes already authorized on them will still work, but you will need to purchase node packs to add new nodes.
> Node packs are only $5.00/month for packs of 25 nodes. You can purchase them on your account page in ZeroTier Central.
> For more information about the pricing plan, please read: https://www.zerotier.com/2022/06/09/zerotier-business-sso-is-here-and-so-is-our-new-pricing/

## Invoices, Billing, Unsubscribe
- Go to your [account page](https://my.zerotier.com/account)
- Click Manage Billing
![20240318162449](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318162449.png)
It will take you to this page where you can update information and download invoices:
![20240318162507](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318162507.png)
### I don't see a "Manage Billing" button
Only the Organization Owner can edit billing info. The account page will show who your Organization Owner is, if it is not you.
### If the address is incorrect on your invoice
Please use the Update Information link. If possible, use Address Line 1 for your company name. And Line 2 for your address. If it doesn't fit please contact us.
After changing the address, use the *download invoice* link. If too much time has passed since the invoice was finalized, it won't be updated. Sorry.
### My invoices don't show my Tax ID
ZeroTier's US Tax ID is 47-3126707. We aren't incorporated outside of the US so we don't have a Tax ID for anywhere outside of the US. Our payment processor will not print your Tax ID on invoices for this reason.
If you need a Tax ID in order to purchase ZeroTier, consider reaching out to a local MSP to buy ZeroTier through them.
### How to delete your whole account
Go to your [account page](https://my.zerotier.com/account) and click on the "delete account" button.
This deletes everything about your account, in accordance with various privacy laws.

## Hide or Delete Nodes
There are delete and hide buttons on the right side of the list nodes in the Members section of a ZeroTier network.
![20240318162824](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318162824.png)
### Hiding a Member
The member will be de-authorized and hidden from view in the list.
Hidden members can be revealed by setting the "Hidden" checkbox above the member's list.
### Deleting a Member
The member will be de-authorized and completely removed from the list. It's a little more permanent than hiding.
### Recover a Deleted Member
You've deleted a member from your network, possibly by accident, and you want to get it back:
- Go to the Settings section of the network
- Find the Manually Add Member section
- Paste in the Node ID
![20240318163025](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318163025.png)
If you've lost your Node ID, it can be viewed on the device in the ZeroTier app.
Delete should only be used if the node will never join again. Just de-authorize them instead if you don't want them counted against your node limits.
If you don't see the Delete and Hide Buttons, you have likely turned off your network's access control by setting it to Public.
Public networks have no access control. If you deleted or de-authorized a member it would reappear within a few seconds.
Set your network to back to Private.
![20240318163059](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318163059.png)
## Web Hooks
Subscribers can receive notifications of changes to their networks, network members, and organizations on my.zerotier.com via Web Hooks.
### Configuring Web Hooks
Go to the [Account](https://my.zerotier.com/account) page. Under `Your Organization` there is a section called `Webhooks`.
![20240318163227](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318163227.png)
Enter the URL your HTTP(S) endpoint that will receive the webhook and (optionally) enter a description, then select the event types you wish to receive on the webhook.
See [github.com/zerotier/ztchooks](https://github.com/zerotier/ztchooks) for example code for receiving & processing webhooks.
All hooks fired will contain the following three fields:
```
{
    "hookd_id": ...,
    "org_id": ...,
    "hook_type": ...,
}
```
The rest of the fields vary by `hook_type`. Full definitions of all of the fields can be found [here](https://github.com/zerotier/ztchooks/blob/main/types.go).
### Hook Types
- Network Join (`NETWORK_JOIN`) - Fired when a new member is requesting to join a network. NOTE: This is only fired once when the network controller first receives the join request from the network member.
- Network Auth (`NETWORK_AUTH`) - Fired when an administrator authorizes a new member to a network.
- Network Deauth (`NETWORK_DEAUTH`) - Fired when an administrator revokes authorization from a network.
- Network Created (`NETWORK_CREATED`) - Fired when an administrator creates a new network.
- Network Deleted (`NETWORK_DELETED`) - Fired when an administrator deletes a network.
- Network Configuration Change (`NETWORK_CONFIG_CHANGED`) - Fired when an administrator changes the configuration of a network.
- Network SSO Login (`NETWORK_SSO_LOGIN`) - Fired when a user is authorized/reauthorized to a network via SSO.
- Network SSO Login Error (`NETWORK_SSO_LOGIN_ERROR`) - Fired when there is an error authorizing/reauthorizing to a network via SSO.
- Member Configuration Change (`MEMBER_CONFIG_CHANGED`) - Fired when an administrator makes a configuration change to a network member.
- Member Deleted (`MEMBER_DELETED`) - Fired when an administrator deletes a network member.
- Organization Invite Sent (`ORG_INVITE_SENT`) - Fired when the account owner invites an administrator to the network.
- Organization Invite Accepted (`ORG_INVITE_ACCEPTED`) - Fired when the invitee accepts and joins the organization.
- Organization Invite Rejected (`ORG_INVITE_REJECTED`) - Fired when the invitee rejects the invitation.
- Organization Member Removed (`ORG_MEMBER_REMOVED`) - Fired when the account owner removes a member from the organization.
### Retries
If a hook fails to be sent for any reason, it will be retried up to a maximum of 10 times with an exponential backoff policy. The 2nd attempt will be approximately 1 minute after the first. The third approximately 2 minutes after the 2nd, and so on to a maximum of 1 hour between attempts. If there is still a failure after 10 attempts, the call will be abandoned.
### Web Hook Security
In order enhance security, and to prevent replay attacks, we have implemented a hook signing algorithm so you can verify each hook request as you receive it. We have also provided a [Go Library](https://github.com/zerotier/ztchooks) and a [TypeScript Library](https://github.com/zerotier/ztchooks-ts) for your use in verifying incoming webhooks.
First you must create a Webhook Signing Secret from your [account page](https://my.zerotier.com/account).
![20240318163829](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318163829.png)
The secret is a hexadecimal string. You will need a copy in order to verify webhooks. The signature will be placed in the HTTP header `X-ZTC-Signature`.
#### Go Example
```
import (
    "net/http"
    "github.com/zerotier/ztchooks"
)

const PSK = "<your_signing_secret>"

func hookCatcher(res http.ResponseWriter, req *http.Request) {
    body, err := io.ReadAll(req.Body)
    if err != nil {
        panic(err)
    }

    signature := req.Header.Get("X-ZTC-Signature")
    if err := ztchooks.VerifyHookSignature(PSK, signature, body, ztchooks.DefaultTolerance); err != nil {
        // signature verification failed
        res.WriteHeader(http.StatusInternalServerError)
        w.Write([]byte("500 - Signature Verification Failed"))
        return
    }

    // Signature verification succeded
}

func main() {
    http.HandleFunc("/", hookCatcher)
    http.ListenAndServe(":9999", nil)
}

```
NOTE: A more complete example of verification & processing hooks can be found [here](https://github.com/zerotier/ztchooks/blob/main/example/hook-catcher.go).
It is possible to have multiple signing secrets. Hooks will be signed with all secrets, and the verification method provided by the `ztchooks` library does take this into account automatically.

# Guides
## Docker
Simple example using an interactive shell.
ZeroTier One makes ZeroTier virtual networks available as 'tap' virtual network ports. To do this inside a Docker container requires a few elevated permissions and access to the `/dev/net/tun` device.
Fortunately this is easy:
```
docker run -it --rm \
--cap-add=NET_ADMIN \
--cap-add=SYS_ADMIN \
--device=/dev/net/tun centos:7 [... command ...]
```
Where `[... command ...]` is an optional command, in the example below we'll provide `/bin/bash` to get an interactive shell.
> **INFO**
> `SYS_ADMIN` is needed because `NET_ADMIN` does not include the `ioctl()` required to put `/dev/net/tun` in tap mode. This appears to be a bug in Linux's capability model but it would have to be fixed upstream.

Here's a transcript of an example session where we start a command prompt in a test container, install ZeroTier, start it (must be done manually here because the container does not run init or systemd), join a test network, and ping something.
Starting a container with an interactive shell:
```bash
docker run -it --rm \
--cap-add=NET_ADMIN \
--cap-add=SYS_ADMIN \
--device=/dev/net/tun centos:7 /bin/bash
```
Then, while inside the container we install ZeroTier:
```bash
[root@5b88595860bc /]# curl https://install.zerotier.com/ | bash
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 12243  100 12243    0     0  18523      0 --:--:-- --:--:-- --:--:-- 18550

*** ZeroTier One Quick Install for Unix-like Systems

*** Supported targets for this script:
***    macOS (10.7+) on x86_64 (just installs ZeroTier One.pkg)
***    Linux / Debian (wheezy or newer) on i386, x86_64, and armhf (Raspbian/jessie only)
***    Linux / Ubuntu (trusty or newer) on i386 and x86_64
***    Linux / SuSE (12+) on i386 and x86_64
***    Linux / CentOS (6+) on i386 and x86_64
***    Linux / Fedora (22+) on i386 and x86_64
***    Linux / Amazon (2016.03+) on x86_64

*** Please report problems to contact@zerotier.com and we will try to fix ASAP!

*** Detecting Linux Distribution

*** Found RHEL/CentOS, creating /etc/yum.repos.d/zerotier.repo

*** Installing zerotier-one package...
[ ... snipped a bunch of yum install output ...]

*** Enabling and starting zerotier-one service...
Created symlink from /etc/systemd/system/multi-user.target.wants/zerotier-one.service to /usr/lib/systemd/system/zerotier-one.service.
Failed to get D-Bus connection: Operation not permitted

*** Package installed but cannot start service! You may be in a Docker
*** container or using a non-standard init service.
```
Start ZeroTier and join a network:
```bash
[root@5b88595860bc /]# /usr/sbin/zerotier-one -d
[root@5b88595860bc /]# /usr/sbin/zerotier-cli join 8056c2e21c000001
200 join OK
[root@5b88595860bc /]# /usr/sbin/zerotier-cli listnetworks
200 listnetworks
200 listnetworks 8056c2e21c000001 - 02:e6:10:ab:69:33 REQUESTING_CONFIGURATION PRIVATE zt0 -
[root@5b88595860bc /]# /usr/sbin/zerotier-cli listnetworks
200 listnetworks
200 listnetworks 8056c2e21c000001 earth.zerotier.net 02:e6:10:ab:69:33 OK PUBLIC zt0 fd80:56c2:e21c:0000:0199:93e6:10b7:8bf1/88,28.183.140.10/7
```
Try pinging something:
```bash
[root@5b88595860bc /]# ping <some_other_zerotier_nodes_ip>
```
Exit
```bash
[root@5b88595860bc /]# exit
```
## DNS
> **BETA**
> This feature is still in beta. This will soon be integrated into ZeroTier 2.0, but for now, it is segregated to allow us to iterate quickly.
### Conceptual Prerequisites
- When ZeroTier joins a network, it creates a virtual network interface.
- When ZeroTier joins multiple networks, there will be multiple network interfaces.
- When ZeroNSD starts, it binds to a ZeroTier network interface.
- When ZeroTier is joined to multiple networks, it needs multiple ZeroNSDs, one for each interface.

This means:
- ZeroNSD will be accessible from the node it is running on.
- ZeroNSD will be accessible from other nodes on the ZeroTier network.
- ZeroNSD will be isolated from other networks the node might be on.

### Technical Prerequisites
This Quickstart was written using two machines - one Ubuntu virtual machine on Digital Ocean, and one macOS laptop on a residential ISP. To follow along step by step, you'll need to provision equivalent infrastructure. If you use different platforms, you should be able to figure out what to do with minimal effort.
### Create a ZeroTier Network
You may do this manually through the [ZeroTier Central WebUI](https://my.zerotier.com/),
![20240318182436](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318182436.png)
### Install ZeroTier
ZeroTier must be installed and joined to the network you intend to provide DNS service to. The following should work from the CLI on most platforms. Windows users may download the MSI from the [ZeroTier Downloads](https://www.zerotier.com/download/) page. For the remainder of this document, please replace the example network `af78bf94364e2035` with a network ID your own.
```
notroot@ubuntu:~$ curl -s https://install.zerotier.com | sudo bash
notroot@ubuntu:~$ sudo zerotier-cli join af78bf94364e2035
notroot@ubuntu:~$ sudo zerotier-cli  set af78bf94364e2035 allowDNS=1
```
### Authorize the Nodes
Authorize the node to the network by clicking the "Auth" button in the Members section in the [ZeroTier Central WebUI](https://my.zerotier.com/).
![20240318182525](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318182525.png)
First, [create a Central API token](https://docs.zerotier.com/api/tokens).
Next, you will need to stash this in a file for ZeroNSD to read.
```bash
sudo bash -c "echo ZEROTIER_CENTRAL_TOKEN > /var/lib/zerotier-one/token"
sudo chown zerotier-one:zerotier-one /var/lib/zerotier-one/token
sudo chmod 600 /var/lib/zerotier-one/token
```
### ZeroTier Systemd Manager
zerotier-systemd-manager publishes rpm and deb packages available at https://github.com/zerotier/zerotier-systemd-manager/releases
```bash
wget https://github.com/zerotier/zerotier-systemd-manager/releases/download/v0.1.9/zerotier-systemd-manager_0.1.9_linux_amd64.deb
sudo dpkg -i zerotier-systemd-manager_0.1.9_linux_amd64.deb
```
Finally, restart all the ZeroTier services.
```bash
sudo systemctl daemon-reload
sudo systemctl restart zerotier-one
sudo systemctl enable zerotier-systemd-manager.timer
sudo systemctl start zerotier-systemd-manager.timer
```
### Install ZeroNSD
ZeroNSD should only run on one node per network. Latency for DNS really matters, so try to place it as close to the clients as possible.
#### Packages
ZeroNSD publishes rpm, deb, and msi packages, available [here](https://github.com/zerotier/zeronsd/releases).
*The latest release is **not** reflected below. Go to the link above to get a link!*
```bash
wget https://github.com/zerotier/zeronsd/releases/download/v0.1.7/zeronsd_0.1.7_amd64.deb
sudo dpkg -i zeronsd_0.1.7_amd64.deb
```
#### Cargo
If we don't have packages for your platform, you can still install it with cargo.
```bash
sudo /usr/bin/apt-get -y install net-tools librust-openssl-dev pkg-config cargo
sudo /usr/bin/cargo install zeronsd --root /usr/local
```
#### Serve DNS
For each network you want to serve DNS to, do the following (replace `af78bf94364e2035` with *your* network ID)
```bash
sudo zeronsd supervise -t /var/lib/zerotier-one/token -w -d beyond.corp af78bf94364e2035
sudo systemctl start zeronsd-af78bf94364e2035
sudo systemctl enable zeronsd-af78bf94364e2035
```
#### Verify functionality
You should be able to ping the laptop via it's DNS name (or any preceding subdomain, since we've set the wildcard flag)
```bash
notroot@ubuntu:~$ ping laptop.beyond.corp
PING laptop.beyond.corp (172.22.192.177) 56(84) bytes of data.
64 bytes from 172.22.192.177 (172.22.192.177): icmp_seq=1 ttl=64 time=50.1 ms
64 bytes from 172.22.192.177 (172.22.192.177): icmp_seq=2 ttl=64 time=49.5 ms
64 bytes from 172.22.192.177 (172.22.192.177): icmp_seq=3 ttl=64 time=48.6 ms
```
or
```bash
notroot@ubuntu:~$ ping laptop.beyond.corp
PING travel.laptop.beyond.corp (172.22.192.177) 56(84) bytes of data.
64 bytes from 172.22.192.177 (172.22.192.177): icmp_seq=1 ttl=64 time=50.1 ms
64 bytes from 172.22.192.177 (172.22.192.177): icmp_seq=2 ttl=64 time=49.5 ms
64 bytes from 172.22.192.177 (172.22.192.177): icmp_seq=3 ttl=64 time=48.6 ms
```
#### Update flag settings
In order to change the settings (such as the TLD), do the following (replace `af78bf94364e2035` with your network ID)
```bash
sudo zeronsd supervise -t /var/lib/zerotier-one/token -w -d beyond.corp af78bf94364e2035
sudo systemctl daemon-reload
sudo systemctl enable zeronsd-af78bf94364e2035
```
Most Linux distributions, by default, do not have per-interface DNS resolution out of the box. To test DNS queries against ZeroNSD without `zerotier-systemd-manager`, find the IP address that ZeroNSD has bound itself to, and run queries against it explicitly.
```bash
sudo lsof -i -n | grep ^zeronsd | grep UDP | awk '{ print $9 }' | cut -f1 -d:
172.22.245.70
```
Query the DNS server directly with the dig command
The Ubuntu machine can be queried with:
```bash
dig +short @172.22.245.70 zt-3513e8b98d.beyond.corp
172.22.245.70
dig +short @172.22.245.70 server.beyond.corp
172.22.245.70
```
The macOS laptop can be queried with:
```bash
dig +short @172.22.245.70 zt-eff05def90.beyond.corp
172.22.245.70
dig +short @172.22.245.70 laptop.beyond.corp
172.22.192.177
```
Add a line to `/etc/hosts` and query again.
```bash
bash -c 'echo "1.2.3.4 test" >> /etc/hosts'
dig +short @172.22.245.70 test.beyond.corp
1.2.3.4
```
Query a domain on the public DNS to verify fall through
```bash
dig +short @172.22.245.70 example.com
93.184.216.34
```
#### macOS
macOS uses `dns-sd` for DNS resolution. Unfortunately, `nslookup`,`host`, and `dig` are broken on macOS. `ping` works.
```
user@osx:~$ ping server.beyond.corp
PING server.beyond.corp (172.22.245.70): 56 data bytes
64 bytes from 172.22.245.70: icmp_seq=0 ttl=64 time=37.361 ms
64 bytes from 172.22.245.70: icmp_seq=1 ttl=64 time=38.129 ms
64 bytes from 172.22.245.70: icmp_seq=2 ttl=64 time=37.569 ms
```
To check out the system resolver settings, use: `scutil --dns`.
The Ubuntu machine can be queried with
`dns-sd -G v4 server.beyond.corp` `dns-sd -G v4 zt-3513e8b98d.beyond.corp`
The macOS machine be queried with
`dns-sd -G v4 laptop.beyond.corp` `dns-sd -G v4 zt-eff05def90.beyond.corp`
#### Windows
Are you a Windows user? Does this work out of the box? Does nslookup behave properly? Let us know... feedback and pull requests welcome =)
#### Serving non-ZeroTier records
**NOTE** this portion of the document is largely intended for advanced users who want to get more out of `zeronsd`'s service.
`zeronsd` will also serve non-zerotier records in two situations: It will forward `/etc/resolv.conf`'s nameservers on a TLD mismatch. This behavior is similar to `dnsmasq`, a popular DNS server on Linux.
Additionally, to serve custom records you can supply the `-f` flag with a file in [hosts format](https://man7.org/linux/man-pages/man5/hosts.5.html) it will service records from that file under the provided TLD, *merged in* with the zerotier nodes. Example below.
**NOTE**: if you followed the steps above, you will want to `systemctl stop zeronsd-<network id>`, and `zeronsd unsupervise <network id>` your network, before continuing.
Make a file called `hosts` and put this in it:
```bash
1.1.1.1 cloudflare-dns
```
Then, let's start a temporary server for now. We'll just use the `start` subcommand of `zeronsd`. This will run in the foreground, so start a new terminal or `&` it.
```bash
$ zeronsd start -t /var/lib/zerotier-one/token -f ./hosts -d beyond.corp <network id>
Welcome to ZeroNS!
Your IP is 1.2.3.4
```
Finally, we can lookup `cloudflare-dns.beyond.corp` to find CloudFlare's DNS server really really fast!
```bash
$ host cloudflare-dns.beyond.corp 1.2.3.4
cloudflare-dns.beyond.corp has address 1.1.1.1
```
> **TIP**
> [See community threads about DNS](https://discuss.zerotier.com/search?q=dns)

## VPN Exit Node
Route all your Internet traffic through a ZeroTier node.
> **IN THIS TUTORIAL**
> - [Create a ZeroTier Network](https://docs.zerotier.com/start)
> - [Set up an `exit node`](https://docs.zerotier.com/exitnode#setup) that handles all your internet traffic
> - Join the `exit node` and a personal node to your ZeroTier network
> - Tell your personal node to use your `exit node`

ZeroTier creates imaginary LANs. Default route override means that computers on your imaginary LAN will be routing Internet traffic through it. Just like a real LAN, your imaginary LAN is going to now need a gateway. Setup here is almost identical to what you'd do to configure a NAT gateway for a conventional wired LAN.
### First, create your exit node
This is the node that will handle all of your internet traffic. It can be something as small as a $5 VPS or a Raspberry Pi.
- If you haven't already, go [create a network](https://docs.zerotier.com/start) and come back
- Install ZeroTier on it
- Join the exit node to your ZeroTier network:
```bash
sudo zerotier-cli join <nwid>
```

### Enable IPv4 Forwarding on the exit node
On the computer acting as the VPN exit node:
```bash
sudo nano /etc/sysctl.conf
```
Add the line:
```bash
/etc/sysctl.conf
net.ipv4.ip_forward = 1
```
Tell the kernel to reload settings:
```bash
sudo sysctl -p
```
Check that settings were correctly applied:
```bash
sudo sysctl net.ipv4.ip_forward
```
If you've joined (and authorized!) your exit node to your network then you are ready to begin configuring it. Start by setting a few environment variables that we will use later:
Get the name of your ZeroTier interface, it will start with `zt`:
```bash
ip link show
```
```bash
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
    link/ether 96:00:02:8d:28:c7 brd ff:ff:ff:ff:ff:ff
    altname enp1s0
3: zthnhhqofq: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 2800 qdisc fq_codel state UNKNOWN mode DEFAULT group default qlen 1000
    link/ether 86:61:c6:fe:ed:dc brd ff:ff:ff:ff:ff:ff
```
In our case it was `zthnhhqofq`. Let's set the environment variable:
```bash
export ZT_IF=zthnhhqofq
```
Next, get the name of the regular WAN interface to the internet:
In our case it was `eth0`
```bash
export WAN_IF=eth0
```
Enable NAT and IP masquerading:
```bash
sudo iptables -t nat -A POSTROUTING -o $WAN_IF -j MASQUERADE
```
Allow traffic forwarding:
```bash
sudo iptables -A FORWARD -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
```
Allow traffic forwarding from the ZeroTier interface to the WAN interface:
```bash
sudo iptables -A FORWARD -i $ZT_IF -o $WAN_IF -j ACCEPT
```
Make `iptables` rules persist after reboot:
```bash
sudo apt-get install iptables-persistent
```
Save your new `iptables` rules:
```bash
sudo netfilter-persistent save
```
> **RESTART**
> At this point it would be a good idea to restart your exit node and verify that the routing rules have persisted.

```bash
sudo iptables-save
-----------------------------------------------------------------------------------------------
...

-A FORWARD -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
-A FORWARD -i zthnhhqofq -o eth0 -j ACCEPT

...

-A POSTROUTING -o eth0 -j MASQUERADE

...
```
Your exit node is complete, now we need to configure your network.
### Configure your network
Now that your exit node is set up we need to configure your ZeroTier network to advertise a Default Route so that other nodes know that the exit node can route traffic to the internet.
Central > Network >
### Tell other nodes to use exit node
> **~/.BASHRC**
> Try adding something like the following for simple tunnel on/off control:
> ```
> tunnel()
> {
>  sudo zerotier-cli set $nwid allowDefault=1
> }
> 
> notunnel()
> {
>  sudo zerotier-cli set $nwid allowDefault=0
> }
> ```

## Cloud Deployments
### Terraform
> **ADVANCED**
> For the multi-cloud edition of this guide, click [here](https://docs.zerotier.com/terraform-multicloud)

#### Welcome
Managing large numbers of settings in a webUI can be a total bummer. It'd be much nicer if we could describe our ZeroTier networks and membership settings as code. That would let us keep them in version control, and integrate them into our software delivery pipelines.
Now we can!
Terraform allows us to talk to the ZeroTier Central API and describe our network infrastructure, as code. This tutorial will walk you though how to get started.
To follow along step by step, you will need:
- A [Github](https://github.com/) account,
- A [ZeroTier Central](https://my.zerotier.com/) account,
- A [Terraform Cloud](https://my.zerotier.com/) account.

It should take you about 10 minutes to through this tutorial. It will be done in browser without touching the command line at all.
#### Import the Quickstart repo
Use Github's [Import](https://github.com/new/import) feature to create a private copy of the [ZeroTier Terraform Quickstart repo](https://github.com/zerotier/terraform-quickstart). We will hook this up to a Terraform runner on [Terraform Cloud](https://app.terraform.io/). After that, we will use Github's in-browser editing feature to drive the tutorial.
![20240318180825](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318180825.png)
#### Create a Terraform workspace
Next, we create a Terraform workspace and attach it to our private Github repository. Be sure to select ***version control workflow***, select the correct Github account, (we want the private copy, not the original), and give it a unique name.
![20240318180949](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318180949.png)
![20240318181001](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318181001.png)
![20240318181011](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318181011.png)
![20240318181020](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318181020.png)
![20240318181029](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318181029.png)
#### Create a Central API Token
Next, we create an API token that Terraform will use to drive the ZeroTier Central API. Navigate to `Account` -> `API Access Tokens`.
![20240318181104](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318181104.png)
#### Add token as a Workspace Environment Variable
Add the token as an Environment Variable to our workspace. This will let the [ZeroTier Terraform Provider](https://github.com/zerotier/terraform-provider-zerotier) authenticate to the API. The variable must be named `ZEROTIER_CENTRAL_TOKEN`. Be sure to check the `Sensitive` box.
![20240318181210](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318181210.png)
#### Hello World
This example is probably the simplest thing you can do with ZeroTier. It creates a single network, then joins two members. The `member_id` settings in the repository are made up, which is good enough to demonstrate how to drive the API with Terraform. Feel free to replace them with real Node IDs of any devices you may wish to join to the networks.
In your Github repo, click on `hello.tf`. There will be a little "edit" icon around the section with the code. Uncomment the Terraform resources and click the green "commit changes" button.
![20240318181312](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318181312.png)
![20240318181322](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318181322.png)
```json
resource "zerotier_network" "hello" {
  name        = "hello"
  description = "Hello World"
  assignment_pool {
    start = "192.168.42.1"
    end   = "192.168.42.254"
  }
  route {
    target = "192.168.42.0/24"
  }
}

resource "zerotier_member" "alice" {
  name        = "alice"
  member_id   = "a11c3411ce"
  description = "Alice's laptop"
  network_id  = zerotier_network.hello.id
}

resource "zerotier_member" "bob" {
  name        = "bob"
  member_id   = "b0bd0bb0bb"
  description = "Bob's laptop"
  network_id  = zerotier_network.hello.id
}
```
Queue a plan then "Confirm and Apply".
![20240318181358](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318181358.png)
![20240318181408](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318181408.png)
After Terraform applies the plan, check out the ZeroTier Cental webUI to confirm it was created.
![20240318181434](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318181434.png)
![20240318181445](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318181445.png)
#### Bridging Networks
The next example manipulates the `allow_ethernet_bridging` settings on the Member objects. When running on machines with multiple physical Ethernet interfaces, ZeroTier can be configured to pass layer2 traffic such as ARP, NDP, multicast, mDNS, etc.
To make this work, you'll need to go into your router's OS and configure a bridge between a physical interface and the ZeroTier interface.
The [ZeroTier Network](https://registry.terraform.io/modules/zerotier/network/zerotier/) Terraform module provides a slightly nicer interface, letting us use CIDRs for our subnets.
Repeat the steps from "Hello World" with `bridging.tf`
![20240318181548](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318181548.png)
```json
module "bridgenet" {
  source      = "zerotier/network/zerotier"
  version     = "1.0.0"
  name        = "bridgenet"
  description = "bridging example"
  subnets     = ["10.10.0.0/16"]
  flow_rules  = "accept;"
}

resource "zerotier_member" "router1" {
  name                    = "router1"
  member_id               = "71c71c71c1"
  description             = "Alice's router"
  ip_assignments          = ["10.10.1.1"]
  allow_ethernet_bridging = true
  network_id              = module.bridgenet.id
}

resource "zerotier_member" "router2" {
  name                    = "router2"
  member_id               = "71c71c71c2"
  description             = "Bob's router"
  ip_assignments          = ["10.10.2.1"]
  allow_ethernet_bridging = true
  network_id              = module.bridgenet.id
}
```
![20240318181612](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318181612.png)
![20240318181620](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318181620.png)
After Terraform applies the plan, check out the ZeroTier Cental webUI to confirm it was created.
![20240318181637](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318181637.png)
#### Network Segmentation
The next example creates the networks, `red`, `green`, and `yellow`. We define two groups. The red team gets access to the `red` network, and the green team gets access to the green network. Red and green make `yellow`.
Repeat the steps from "Hello World" with `groups.tf`
![20240318181737](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318181737.png)
```json
variable "segments" {
  default = {
    red = {
      description = "red"
      subnets     = ["10.1.0.0/16"]
      flow_rules  = "accept;"
    }
    green = {
      description = "green"
      subnets     = ["10.2.0.0/16"]
      flow_rules  = "accept;"
    }
    yellow = {
      description = "yellow"
      subnets     = ["10.3.0.0/16"]
      flow_rules  = "accept;"
    }
  }
}

variable "members" {
  default = {
    red = {
      eve = {
        description = "Eve's Laptop"
        member_id   = "34b34b34b3"
      }
      steve = {
        description = "Steve's Laptop"
        member_id   = "573b3573b3"
      }
    }
    green = {
      cletus = {
        description = "Cletus' Laptop"
        member_id   = "c133715b0b"
      }
      brandie = {
        description = "Brandie's Laptop"
        member_id   = "b33fb33fff"
      }
    }
  }
}

module "segments" {
  for_each    = var.segments
  source      = "zerotier/network/zerotier"
  version     = "1.0.0"
  name        = each.key
  description = each.value.description
  subnets     = each.value.subnets
  flow_rules  = each.value.flow_rules
}

resource "zerotier_member" "red" {
  for_each   = { for k, v in var.members.red : (k) => v }
  name       = each.key
  member_id  = each.value.member_id
  network_id = module.segments["red"].id
}

resource "zerotier_member" "green" {
  for_each   = { for k, v in var.members.green : (k) => v }
  name       = each.key
  member_id  = each.value.member_id
  network_id = module.segments["green"].id
}

resource "zerotier_member" "yellow" {
  for_each   = { for k, v in merge(var.members.red, var.members.green) : (k) => v }
  name       = each.key
  member_id  = each.value.member_id
  network_id = module.segments["yellow"].id
}
```
![20240318181809](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318181809.png)
After Terraform applies the plan, check out the ZeroTier Cental webUI to confirm it was created.
![20240318181827](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318181827.png)
#### Many to Many
In the last example, show how to assign many members to many networks. This example used the Terraform [setproduct](https://www.terraform.io/docs/language/functions/setproduct.html) function to find all possible combinations of two sets.

The `zerotier_identity` resource is a distant cousin of the Terraform [tls_private_key](https://registry.terraform.io/providers/hashicorp/tls/latest/docs/resources/private_key) resource. This resource would normally be used to inject secrets into cloud instances via `cloudinit`. We encourage you to use the [Terraform Cloud](https://app.terraform.io/) to keep your Terraform state safe.

Repeat the steps from "Hello World" with `many2many.tf`
![20240318182052](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318182052.png)
```json
variable "letters" {
  default = ["alfa", "bravo", "charlie"]
}

variable "shapes" {
  default = ["circle", "square", "diamond" ]
}

resource "zerotier_identity" "letters" {
  for_each = { for i in var.letters : i => i }
}

module "shapes" {
  for_each    = { for i, k in var.shapes : (k) => i }
  source      = "zerotier/network/zerotier"
  version     = "1.0.0"
  name        = each.key
  description = each.key
  subnets     = [cidrsubnet("10.11.0.0/16", 8, each.value)]
  flow_rules  = "accept;"
}

resource "zerotier_member" "shape-letters" {
  for_each    = { for i in setproduct(var.letters, keys(module.shapes)) : join("-", flatten(i)) => i }
  name        = each.key
  member_id   = zerotier_identity.letters[each.value[0]].id
  description = module.shapes[each.value[1]].description
  network_id  = module.shapes[each.value[1]].id
}
```
![20240318182119](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318182119.png)
After Terraform applies the plan, check out the ZeroTier Cental webUI to confirm it was created.
![20240318182136](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318182136.png)
#### Cleaning up
When you're done experimenting with ZeroTier and Terraform, tear everything down by queueing a destroy plan.
![20240318182157](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318182157.png)
![20240318182210](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318182210.png)
#### That's all folks
If you like this tutorial, check out the [ZeroTier Multicloud Terraform Quickstart](https://docs.zerotier.com/terraform/multicloud-quickstart) next!
-s
### cloud-init
#### Set up ZeroTier using `cloud-init`
##### What is `cloud-init`?
cloud-init is a convenient and cross-platform way to initialize cloud instances. It is supported by all major cloud providers. You can use it to configure OS settings, install packages, or even start up specific instances of ZeroTier.
##### How do you use it?
Typically a cloud-init file can be uploaded to a cloud provider or added as an additional payload via an API.
##### Example
```conf
#cloud-config

hostname: "devbox"
users:
  - name: wrankles

shell: /bin/bash

disable_root: 1
ssh_pwauth:   0
packages:
  - htop
  - tcpdump

runcmd:
  - apt-get update
  #- mkdir -p /var/lib/zerotier-one
  #- echo "1234567890:0:..." > /var/lib/zerotier-one/identity.secret
  #- echo "1234567890:0:..." > /var/lib/zerotier-one/identity.public
  - curl -s https://install.zerotier.com | bash
  - zerotier-cli join cfb8bf9836c2fc3a
  - systemctl restart ssh
```
##### Pre-populate ZeroTier identities(optional)
Uncomment the `runcmd` lines starting with `mkdir` and `echo` if you want to write known ZeroTier identities to disk before startup. This is useful if you've already authorized or scripted around a specific node ID and want it to start up the same each time.
> **CAUTION**
> If you place your ZeroTier node's secret key in your cloud init file it is possible for someone to impersonate your node if they get ahold of this cloud-init file.

##### Password-less SSH (optional)
It's usually recommended that you disable the root account and disable password-based ssh authentication and rely solely on key-based authentication. [Digital Ocean has some really great documentation on the subject](https://www.digitalocean.com/community/tutorials/how-to-use-cloud-config-for-your-initial-server-setup).
To make getting into your cloud instance as easy as possible you can add entries to `authorized_keys`:
```ssh
ssh-authorized-keys:
    - ssh-rsa AAAAB38fwi3756q238if75dh6awd476r3bg78f56ghfaa7fdh63qf5dq378f5632gha3875j3f498da7hfhjkfawejtfsktfr89ew4jftsjrf8t9rhg7tjser8tsre7yjgvased89rfdcsh67rewhg8tq7tsge546w4
```
To make getting into other instances *from this instance* easier, you can add a pre-generated private key to `ssh_keys`:
```ssh
ssh_deletekeys: false
ssh_keys:
    ed25519_private: |
        -----BEGIN OPENSSH PRIVATE KEY-----
        $#HYBES$Y%GW$Gfs9g8ES$Y%GW$Gfs9g867ZDI1NTE5AAAAIHQbjzWgsNOt/8gH+QI/Eob
        fs9g867q34tqGT$#HYBES$mZ+AW4OZPnTPI89ZPmVMLuayrD2cE86Z/il8b+867q34tqGT
        fs9g867q34ZDI1NTE5AAAAIHQbjzWgsNOt/8gH+QI/Eob6BES$Y%$#HYBES$Y%GW$Gfs9g
        fs9g867q34tqGT$#HYBES$YZDI1NTE5AAAAIHQbjzWgsNOt/8gH+QI/Eob6Z/8ZXbHHdGx
        fs9g867mZ+AW4OZPnTPI89ZPmVMLuayrD2cE86Z/il8b+gw3s9g867q4g
        -----END OPENSSH PRIVATE KEY-----

    ed25519_public: ssh-ed25519 AAAAB3NzaC1yc2EAAAABIwAAAQEAklOUpqxiX1nKhXpHAZsMciLq8V6RjsHDTYW7hdI4 devbox
```
[Read more about SSH keys here](https://wiki.archlinux.org/title/SSH_keys)
## Advanced Networking
### Multipath
Multipath allows the simultaneous (or conditional) aggregation of multiple physical links into a bond for increased total throughput, load balancing, redundancy, and fault tolerance. There is a set of standard bonding policies available that can be used right out of the box with no configuration. These policies are inspired by [the policies offered by the Linux kernel](https://www.kernel.org/doc/Documentation/networking/bonding.txt). A bonding policy can be used easily without specifying any additional parameters. For example:
```conf
Example local.conf
-------------------------------------------------------------------------------------
{ "settings": { "defaultBondingPolicy": "active-backup" } }
```
#### Standard policies
- [active-backup](https://docs.zerotier.com/multipath#active-backup): Use only one primary link at a time and failover to another designated link.
- [broadcast](https://docs.zerotier.com/multipath#broadcast): Duplicate traffic across all available links at all times.
- [balance-rr](https://docs.zerotier.com/multipath#balance-rr): Stripe packets across multiple links (not for use with TCP.)
- [balance-xor](https://docs.zerotier.com/multipath#balance-xor): Hash flows to specific links.
- [balance-aware](https://docs.zerotier.com/multipath#balance-aware): Auto-balance flows across links.

#### Custom policies
To customize a bonding policy simply specify a standard policy as `basePolicy` and override its parameters. Note that a modified custom policy *cannot* have the same name as a standard policy. (Note how we renamed the following example to `rapid-active-backup`). For example, to create a more rapid `active-backup` policy that will failover `1 second` after it detects a link failure:
```conf
local.conf
------------------------------------------------------------------------------------
{
  "settings":
  {
    "defaultBondingPolicy": "rapid-active-backup",
    "policies":
    {
      "rapid-active-backup":
      {
        "basePolicy": "active-backup",
        "failoverInterval": 1000
      }
    }
  }
}
```
You can only override parameters that are valid for the chosen `basePolicy`. The list below shows parameters that can apply to any policy. Parameters that are unique to a given policy type are described in that policy's section.
```json
Example policy settings (not valid JSON)
------------------------------------------------------------------------------------
{
  "settings":
  {
    "defaultBondingPolicy": "rapid-active-backup",
    "policies":
    {
      "rapid-active-backup":
      {
        "basePolicy": "active-backup",
        "failoverInterval": 1000,
        "upDelay":0-65535,
        "downDelay":0-65535,
      }
    }
  }
}
```
#### Physical interfaces
Bonds are composed of multiple links (known paths over system network interfaces). By default if no interfaces are specified ZeroTier will attempt to use all network interfaces, including your expensive wireless links. In order to avoid this you should tell ZeroTier which interfaces are okay to talk on and also under what conditions. If a set of links is defined, ZeroTier will use only those links and ignore everything else.
```json
Example local.conf: To specify that ZeroTier should only use eth1 but then failover to eth2 and that it should prefer IPv4 over IPv6 except on eth2 where only IPv6 is allowed.
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
{
  "settings": {
    "defaultBondingPolicy": "rapid-active-backup",
    "policies": {
      "rapid-active-backup": {
        "basePolicy": "active-backup",
        "failoverInterval": 1000,
        "links":
        {
          "eth0":
          {
            "ipvPref": 46,
            "failoverTo": "eth1"
          },
          "eth1":
          {
            "ipvPref": 46,
            "failoverTo": "eth2"
          },
          "eth2":
          {
            "ipvPref": 6
          }
        }
      }
    }
  }
}
```
#### Link settings
The behavior of links can be changed by setting various properties:
```json
Customizable link parameters (not valid JSON)
-------------------------------------------------------------------------------------
...

"links":
{
  "eth0": /* Network interface system name */
  {
    "ipvPref": [0,4,6,46,64], /* (optional) IP version preference for paths on a link. */
    "capacity": 0-1000000, /* (optional) Arbitrary units of link "capacity". Can be used to manually allocate traffic. */
    "failoverTo": "failoverInterfaceName", /* (optional) Which link should be used next after a failure of this link. */
    "mode": "primary"|"spare" /* (optional) Whether this link is used by default or only after all other links fail. */
  },
  ...
}

...
```
#### Spare links
A warm spare is a link that is not actively used to process traffic until all other available links are down. Note however that some ambient probing traffic is still sent to guarantee its performance. Spare links are not available when using the `broadcast` policy. Cold spares are left as an exercise for the reader.
```
Example local.conf: To specify that traffic should be hashed over two links eth0 and eth1 and that expensive_lte0 should only be used when the other two are down.
---------------------------------------------------------------------------------------------------------------------------------------------------
{
  "settings": {
    "defaultBondingPolicy": "xor-with-a-spare",
    "policies": {
      "xor-with-a-spare": {
        "basePolicy": "balance-xor",
        "links":
        {
          "eth0":
          {
            "mode": "primary"
          },
          "eth1":
          {
            "mode": "primary"
          },
          "expensive_lte0":
          {
            "mode": "spare"
          }
        }
      }
    }
  }
}
```
#### Peer-specific bonds
It is possible to use a policy for one peer and another policy for a different peer. For instance, if one were to want `active-backup` for all peers by default but for certain peers to be bonded with a custom load-balanced bond such as `my-custom-balance-xor` one could do the following:
```json
Example local.conf
-----------------------------------------------
{
  "settings":
  {
    "defaultBondingPolicy": "active-backup",
    "policies":
    {
      "my-custom-balance-xor":
      {
        "failoverInterval": 2000,
        "upDelay": 5000,
        "basePolicy": "balance-xor"
      }
    },
    "peerSpecificBonds":
    {
      "f6203a2db3":"my-custom-balance-xor",
      "45b0301da2":"my-custom-balance-xor",
      "a92cb526fa":"my-custom-balance-xor"
    }
  }
}
```
#### active-backup
```json
Example local.conf
{ "settings": { "defaultBondingPolicy": "active-backup" } }
```
Traffic is sent on only one link at any time. A different link becomes active if the current link fails. This mode provides fault tolerance with a nearly immediate failover depending on the `failoverInterval` you set. This mode does not increase total throughput. If no `primary` and `spare` links are defined ZeroTier will attempt to pick the best one.
- `mode`: `primary|spare` Link option which specifies which link is the primary device. The specified device is intended to always be the active link while it is available. There are exceptions to this behavior when using different `linkSelectMethod` modes. There can only be one `primary` link in this bonding policy.
- `linkSelectMethod`: Specifies the selection policy for the active link during failure and/or recovery events. This is similar to the Linux Kernel's `primary_reselect` option but with a minor extension:
  - `optimize`: **(default if user provides no failover guidance)** The primary link can change periodically if a superior link is detected.
  - `always`: **(default when links are explicitly specified)**: Primary link regains status as active link whenever it comes back up.
  - `better`: Primary link regains status as active link when it comes back up and (if) it is better than the currently-active link.
  - `failure`: Primary link regains status as active link only if the currently-active link fails.
```conf
Example local.conf
------------------------------------------------------------------------------
{
  "settings":
  {
    "defaultBondingPolicy": "custom-active-backup",
    "custom-active-backup":
    {
      "basePolicy": "active-backup",
      "linkSelectMethod": "always",
      "failoverInterval": 5000,
      "links":
      {
        "eth0": { "failoverTo": "eth1", "mode": "primary" },
        "eth1": { "mode": "spare" },
        "eth2": { "mode": "spare" },
        "eth3": { "mode": "spare" }
      }
    }
  }
}
```
#### broadcast
```json
Example local.conf
-------------------------------------------------------
{ "settings": { "defaultBondingPolicy": "broadcast" } }
```
Traffic is sent on *all* available links simultaneously. This mode provides fault tolerance and effectively immediate failover due to transmission redundancy. This mode is a poor utilization of throughput resources and will **not** increase throughput but can prevent packet loss during a link failure.

#### balance-rr
```json
Example local.conf
--------------------------------------------------------------------
{ "settings": { "defaultBondingPolicy": "balance-rr" } }
```
Traffic is striped across multiple links. This mode offers partial fault tolerance immediately, full fault tolerance eventually. This policy is unaware of protocols and is primarily intended for use with protocols that are not sensitive to reordering delays. The only option available for this policy is `packetsPerLink` which specifies the number of packets to transmit via a link before moving to the next link in the round-robin sequence. When set to `0` a link is chosen at random for each outgoing packet (doing so would be inefficient). The default value is `64`, lower values can begin to add overhead to packet processing. This mode is **not** suitable for traffic that is sensitive to re-ordering such as TCP.
#### balance-xor
```json
Example local.conf
------------------------------------------------------------
{ "settings": { "defaultBondingPolicy": "balance-xor" } }
```
Traffic is categorized into *flows* based on *source port*, *destination port*, and *protocol type* these flows are then hashed onto available links where they will persist for the duration of their life. Traffic that does not have an assigned port (such as ICMP pings) will be randomly distributed across links. This mode offers partial fault tolerance immediately, full fault tolerance eventually. This mode is suitable for traffic that is sensitive to re-ordering such as TCP.
#### balance-aware
```json
Example local.conf
------------------------------------------------------
{ "settings": { "defaultBondingPolicy": "balance-aware" } }
```
This mode operates similarly to `balance-xor` in that it hashes flows onto specific links. However, it may reassign flows mid-conversation and perform other types of optimizations. This mode may surprise you more often than `balance-xor` by causing re-ordering delays for certain flows but it should lead to a better total experience when all flows are considered. This policy allows you to specify not only [relative link capacities](https://docs.zerotier.com/multipath#asymmetric-links) but also a [notion of quality](https://docs.zerotier.com/multipath#link-quality) expressed as a weighted vector with a sum of `1.0`. See below:
```json
Example local.conf: A maximum acceptable value for latency and packet delay variance are given along with weights that tell ZeroTier how important that limit is. Additionally, link capacities are given as hints to enable proportional traffic allocation.
------------------------------------------------------------------------------------------------------------------------------------------------------------------
{
  "settings":
  {
    "defaultBondingPolicy": "custom-balance-aware",
    "policies":
    {
      "custom-balance-aware":
      {
        "basePolicy": "balance-aware",
        "failoverInterval": 5000,
        "linkQuality": {
          "lat_max" : 400.0,
          "pdv_max" : 20.0,
          "lat_weight" : 0.5,
          "pdv_weight" : 0.5
        },
        "links": {
          "wlan0": { "capacity": 250 },
          "eth0": { "capacity": 1000  }
        }
      }
    }
  }
}
```
In the above example if any links begin to show signs of saturation (for instance if latency increases beyond `400ms`) flows will be moved from it until it is no longer judged to be saturated.
#### Link Quality
As seen in the [balance-aware](https://docs.zerotier.com/multipath#balance-aware) example configuration, you can provide hints to ZeroTier as to when a link is no longer suitable for use. You can set limits on the following:
- `lat_max`: Maximum (mean) latency observed over many samples
- `pdv_max`: Maximum packet delay variance (similar to jitter)
Then, weights must also be provided to tell ZeroTier how important your limits are (as a reminder, the weights must sum to `1.0`):
```
"linkQuality": {
  "lat_max" : 80.0,
  "pdv_max" : 20.0,
  "lat_weight" : 0.5,
  "pdv_weight" : 0.5
}
```
If any one of these limits are violated ZeroTier will attempt to avoid assigning new flows to the link in question as well as begin moving flows from that link to other higher quality links. ZeroTier will first try to find a link that doesn't violate any of your limits but if it is unable to do so it will pick the next best according to a quality ranking derived from your weights. There is no guarantee that links will be entirely avoided or that all of their flows will be moved if there are no better links to move to so these limits are merely strong suggestions to ZeroTier.
#### Asymmetric links
Any set of physical links regardless of their relative performance may be combined using any of the policies. However if the relative performance among the links vary too wildly (for instance a fiber link and an LTE link) you may find that blindly striping or hashing traffic across them doesn't turn out so well. For these scenarios it is recommended that you use [balance-aware](https://docs.zerotier.com/multipath#balance-aware) and provide `capacity` and/or `linkQuality` hints to ZeroTier.
##### Capacity
```conf
Example capacities (not a valid config by itself)
------------------------------------------------------
"links": {
  "wlan0": { "capacity": 250 },
  "eth0": { "capacity": 1000  }
}
```
The term is meant to be a one-size-fits-all numerical value which represents the diameter of the pipe so to speak. That is, how much traffic it can process before it becomes clogged and congestion controls kick in. This will suggest to ZeroTier that it should assign traffic flows proportionally.
##### Quality
Providing [quality](https://docs.zerotier.com/multipath#link-quality) hints to ZeroTier can be done in conjunction with the aforementioned `capacity` property without issue. See the [balance-aware](https://docs.zerotier.com/multipath#balance-aware) section for an example of how to do this.
```conf
Example local.conf: How to use arbitrary (but relative) capacity values
--------------------------------------------------------------------------
{
  "settings":
  {
    "defaultBondingPolicy": "custom-balance-aware",
    "policies":
    {
      "custom-balance-aware":
      {
        "basePolicy": "balance-aware",
        "links": {
          "eth0": { "capacity": 10000 },
          "eth1": { "capacity": 1000  },
          "eth2": { "capacity": 100   }
        }
      }
    }
  }
}
```
The above configuration will roughly result in the following allocation:
```bash
10000 + 1000 + 100 = 11100 total "capacity units" across all links

eth0 10000 / 11100 = ~0.900 %
eth1 1000  / 11100 = ~0.090 %
eth2 100   / 11100 = ~0.009 %
```
A good rule of thumb would be to express your known link speeds in units of `Mbit/s`. This isn't strictly necessary as long as the quantities make sense relative to each other.
#### Using the CLI
Currently most configuration is handled via manual editing of each node's `local.conf`. There are only a few available CLI commands.
To view bonds to all peers:
```
zerotier-cli bond list
--------------------------------------------------------------
    <peer>                        <bondtype>     <links>
2f33b459c1                       balance-xor         18/18
6af835ae71                     active-backup         2/2
77dcbe7120                     active-backup         5/5
cbae04eb89                     active-backup         5/5
bafe99feb9                     active-backup         3/3
```
> **TIP**
> If you'd like to ingest this data into your own monitoring solution use `zerotier-cli -j bond list` to emit a JSON blob instead.

To see the current state of a bond to a given peer:
```bash
zerotier-cli bond 77dcbe7120 show
---------------------------------------------------------------------------------------------------------------------------------------------
Peer                   : 77dcbe7120
Bond                   : balance-rr
Link Select Method     : 0
Links                  : 4/4
Failover Interval (ms) : 500
Up Delay (ms)          : 0
Down Delay (ms)        : 0
Packets Per Link       : 64

idx                  interface                                  path               socket
----------------------------------------------------------------------------------------------------
 0:                     enp5s0                                192.168.88.155/57605 000055e03aa4d5a0
 1:                     enp5s0                                192.168.88.155/57605 000055e03aa4dc80
 9:            wlxc07a002fb470                                192.168.88.155/57605 000055e03aa4d440
10:            wlxc07a002fb470                                192.168.88.155/57605 000055e03aa4db20


idx     lat      pdv     plr     per    speed  alloc      rx_age      tx_age  eligible  bonded
----------------------------------------------------------------------------------------------------
 0:     0.00     0.00  0.0000  0.0000    1000   0.07          62          66         1       1
 1:    10.00    21.00  0.0000  0.0000    1000   0.04          61          66         1       1
 9:     6.00     1.00  0.0000  0.0000    5000   0.06         227         567         1       1
10:     0.00     0.00  0.0000  0.0000    5000   0.07          60          48         1       1
```
The reported allocation percentages have an uncertainty value of `+/- 0.4%`.
To forcibly rotate to a different link in an `active-backup` bond:
```bash
zerotier-cli bond 77dcbe7120 rotate
-------------------------------------------------------------------------------------------------------------------------
active link rotated from 000055c8aa8392f0-enp5s0/104.175.36.67/9993 to 000055c8aa838210-enp5s0/104.175.36.67/9993
```
To set a custom MTU for a bonded link:
```bash
zerotier-cli bond setmtu 1300 enp5s0 192.168.88.155
---------------------------------------------------------------------------------------------------------
200 setmtu OK
```
### Layer 2 Bridge
Do you have devices that can't run ZeroTier that you want to access remotely? You can use a small Linux PC as a bridge between ZeroTier and physical networks.
> **NOTE**
> This topic is related to but different from using ZeroTier as a Layer 5 [Service Proxy](https://docs.zerotier.com/proxy).

#### Assumptions
- You're doing this on your home network and can log in to your router and find the DHCP settings.
- You have a keyboard, monitor, and ethernet cable plugged into your Pi. Chances are high we'll break networking and lose access to the Pi.
- You're somewhat familiar with the command line and ssh.
- We're going to use systemd networking for this. You could probably adapt the concepts to a different Linux network configuration system if you have opinions about systemd.
- We used a Raspberry Pi 2 while writing this, but a Pi 3 or 4 should work fine. Anything running a Debian 10+ based distro should be fine. It doesn't have to be a Raspberry Pi, but some of these instructions might be Raspbian specific.

#### What you'll need
- Physical LAN Subnet
- Physical LAN DHCP Range
- ZeroTier Auto-Assign Range
- Default Gateway IP Address (the router)
- Bridge IP Address (will be statically assigned)
- Create a new ZeroTier network and get the ID. Keep an old network around for secondary way to connect any devices already using ZeroTier.
- The DHCP range and ZeroTier Auto-Assign range should be in the same subnet, but not overlap. You'd probably base this off what is already configured on your router.

##### An example plan
**Name**|**Value**|**Referred to below as**
----|----|----
Physical LAN Subnet	|192.168.192.0/24	|
Physical LAN DHCP RANGE	|192.168.192.65 through 192.168.192.126	|
ZeroTier Auto-Assign Range	|192.168.192.129 through 192.168.192.190	|$ZT_POOL
ZeroTier Managed Route	|192.168.192.0/23	|$ZT_ROUTE
Default Gateway IP Address	|192.168.192.1	|$GW_ADDR
Bridge IP Address	|192.168.192.2/24 (or use DHCP)	|$BR_ADDR
ZeroTier Network ID	|d5e04297a19bbd70	|$NETWORK_ID
ZeroTier Network Interface Name	|zt3jnwghuq	|$ZT_IF

#### Get your bridge device up and running
##### Follow the Raspberry Pi install instructions
- [Install Raspbian OS](https://www.raspberrypi.org/downloads/raspbian/)
- [Enable SSH](https://www.raspberrypi.org/documentation/remote-access/ssh/)

#### SSH into the Pi
It's easier to login via ssh now and copy/paste commands from the comfort of your own PC.
The DNS name might just work for you:
`ssh pi@raspberrypi.local` Or `ssh pi@<ip-address-of-pi>`
#### Update the Operating System
```bash
sudo apt update && sudo apt -y full-upgrade && sudo reboot
```
Log back in after it's done
#### Install ZeroTier
```bash
https://www.ZeroTier.com/download/
-------------------------------------------------------------------------
curl -s https://install.zerotier.com | sudo bash
```
#### Let's set some shell variables now
```
NETWORK_ID=<your-network-id>
BR_IF="br0"
BR_ADDR=<your-bridge-address>
GW_ADDR=<your-gateway-address>
```
#### Join ZeroTier Network
```bash
sudo zerotier-cli join $NETWORK_ID
```
We don't want ZeroTier to manage addresses or routes on `$ZT_IF`. We're doing it statically below, on the bridge interface.
```bash
sudo zerotier-cli set $NETWORK_ID allowManaged=0
```
Set one more variable
```bash
ZT_IF=<your-zt-interface-name>
```
Copy the `dev` name from the `listnetworks` output for `$ZT_IF`. It will be something like: zt3jvirser
#### Configure the device in ZeroTier Central
- Go to the Members section of the Network
- Open the Wrench Icon for advanced settings and check
- Check Allow Bridging
- Check Do Not Auto Assign
- Authorize the member

#### Switch to systemd networking
Remove existing network stuff
```bash
sudo apt remove --purge --auto-remove dhcpcd5 fake-hwclock ifupdown isc-dhcp-client isc-dhcp-common openresolv
```
Enable systemd-networkd
```bash
sudo ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf;
sudo systemctl enable systemd-networkd;
sudo systemctl enable systemd-resolved;
sudo systemctl enable systemd-timesyncd;
```
Configure interfaces
```bash
sudo zerotier-cli set $NETWORK_ID allowManaged=0
```
Write Network Configuration files. Puts ethernet and zerotier into the bridge and configures the bridge with a static IP. See below for DHCP configuration on the bridge.
```bash
cat << EOF | sudo tee /etc/systemd/network/25-bridge-br0.network
[Match]
Name=$BR_IF

[Network]
Address=$BR_ADDR
Gateway=$GW_ADDR
DNS=1.1.1.1
EOF

cat << EOF | sudo tee /etc/systemd/network/br0.netdev
[NetDev]
Name=$BR_IF
Kind=bridge
EOF

cat << EOF | sudo tee /etc/systemd/network/25-bridge-br0-zt.network
[Match]
Name=$ZT_IF

[Network]
Bridge=$BR_IF
EOF

cat << EOF | sudo tee /etc/systemd/network/25-bridge-br0-en.network
[Match]
Name=eth0 # might be en*

[Network]
Bridge=$BR_IF
EOF
```
Review configuration
```bash
tail -n+0 /etc/systemd/network/*
```
If needed, edit the files with the editor of your preference.
If it looks good:
```bash
sudo reboot
```
You should be able to, from the physical LAN, connect to the Pi via `$BR_ADDR`
#### If it takes a long time waiting for the network during boot
Sometimes the physical interface turns out to be a long "predictable interface name" like: "enb827eb0d4176", sometimes it's just `eth0`, depending on Raspbian version(???).
https://wiki.debian.org/NetworkConfiguration#Network_Interface_Names
Hook up a keyboard and monitor and check with ip addr then edit `/etc/systemd/network/25-bridge-br0-en.network` to match.
#### Configure ZeroTier Network
At `my.zerotier.com/network/$NETWORK_ID`->`Settings`->`Advanced`
- Delete the default Managed Route. Add the new Managed Route `$ZT_ROUTE`
- Change IPV4 Auto-Assign to Advanced
- Remove existing Pool. Create new Pool with start and end from `$ZT_POOL`
- For documentation purposes, assign `$BR_ADDR` to the ZeroTier bridge member
It should be working now.

#### Next steps
Either it worked, and you can ssh back in to `$BR_ADDR` after a minute, or it didn't work and the Pi isn't on the network anymore and you need to use the keyboard and monitor to figure out what went wrong.
> **TIP**
> Make a backup of the sd card so you don't have to repeat these steps

#### Appendix
Configure bridge with DHCP
```bash
cat << EOF | sudo tee /etc/systemd/network/25-bridge-br0.network
[Match]
Name=$BR_IF

[Network]
DHCP=yes
EOF
```
#### FAQ
##### I can ping the bridge, but not behind it
Sometimes, iptables rules apply: `echo "0" > /proc/sys/net/bridge/bridge-nf-call-iptables` or `iptables -A FORWARD -p all -i br0 -j ACCEPT`
See: https://serverfault.com/questions/162366/iptables-bridge-and-forward-chain
##### Why is the Managed Route /23 and the LAN subnet /24?
Say you have a laptop that is on the ZeroTier network and you bring it home. Now its WiFi address and ZeroTier address are in the same subnet. Which interface/address should your laptop use for internet access? https://en.wikipedia.org/wiki/Longest_prefix_match
##### Why is an app on my phone not working over ZeroTier?
Unfortunately the iOS and Android VPN APIs won't let ZeroTier use multicast/broadcast. These are typically how apps auto-discover services on the LAN. 😭 Stay tuned for an article on bridging a ZeroTier network and a WiFi access point.
#### References
- https://systemd.network/systemd.network.html
- https://hackaday.io/project/162164/instructions

### Layer 5 Proxy (Pylon)
[zerotier/pylon](https://github.com/zerotier/pylon) is a tool built using [libzt](https://github.com/zerotier/libzt) that allows you to run a SOCKS5 Proxy that connects services and apps to and from your secure ZeroTier network without installing ZeroTier and without bringing up any new network interfaces. Pylon can be run as one of two personalities that can work alone or together depending on your needs:
**Name**|**What does this do**|**Is this a ZeroTier Node?**
----|----|----
`pylon refract`	|This bridges traffic to and from your LAN	|Yes
`pylon reflect`	|This relays traffic over `TCP/443`	|No
In a nutshell you run `pylon refract` on some physical device on your LAN. Then devices and services that cannot install ZeroTier will connect to it on a port that you specify. Now services or devices on your LAN can talk to things on your ZeroTier network and vice versa. You can optionally run a `pylon reflect` if you're behind some [tricky NAT](https://docs.zerotier.com/nat) and need to relay over TCP/443.
See the repo for more thorough instructions: [zerotier/pylon](https://github.com/zerotier/pylon)
> **NOTE**
> This topic is related to but different from Bridging. Proxy mode is useful if you're unable to install our normal client on your devices.

### code-server + ZeroTier
![20240318195025](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318195025.png)
#### What
`code-server` allows you to run an instance of VSCode in the browser and edit code on remote machines. Combining this with ZeroTier lets you do this securely across your virtual network. See their project page: [github.com/coder/code-server](https://github.com/coder/code-server)
#### Install
```bash
curl -fsSL https://code-server.dev/install.sh | sh
```
#### Configure
By default `code-server` will listen on `0.0.0.0`. We of course do not want to do this since we'd like to only access it over our secure ZeroTier network. To do this, open the config file:
```bash
nano /home/$USER/.config/code-server/config.yaml
```
Change your `bind-addr` to the ip address of the ZeroTier network interface for the network you'd like to access code-server over.
You can get this via [Central](https://my.zerotier.com/) or from the `zerotier-cli`. For instance:
```bash
sudo zerotier-cli listnetworks
200 listnetworks <nwid> <name> <mac> <status> <type> <dev> <ZT assigned ips>
200 listnetworks 8156abe27c21623c intranet.joseph.com 31:26:27:43:19:fb OK PRIVATE ztcjyorbnc fd80:76c1:124c:2268:1da9:9bf1:14d:ab3e/88,10.241.208.61/16
```
```bash
bind-addr: 10.241.208.61:8080
auth: password
password: correcthorsebatterystaple
cert: false
```
#### Run
```bash
code-server
```
#### Access Remotely
Now from another computer *that is also joined to the same ZeroTier network* open a browser and type in the node's address and port that is running code-server, you should be prompted to enter your password:
![20240318200012](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240318200012.png)
You're done!
> **CAUTION**
> VS Code may state: `code-server is being accessed in an insecure context. Web views, the clipboard, and other functionality may not work as expected.`, this is true but as long as you've correctly set your `bind-addr` in the previous step your connection even over HTTP is secure since they are mediated over ZeroTier. Though we do recommend defense in depth so for sensitive situations we always suggest HTTPS.

### Route between ZeroTier and Physical Networks
This seems to be the simplest pattern for getting remote access to your LAN. It doesn't require access to the LAN's router or have some of the pitfalls of bridging. This requires a Linux PC or VM, something that runs iptables, on your LAN. A Raspberry Pi works. This is a NAT/Masquerade setup.
If you have a router that can run zerotier, you should use that instead of this article. Many router vendors and operating systems have zerotier packages.
> **POSSIBLE DISADVANTAGES**
> No broadcast/multicast across networks (but the mobile OS's don't allow this anyways).
> Can't initiate connections from the LAN to an external ZeroTier client.

#### Summary
- Install ZeroTier
- Add a managed route to the ZeroTier network (at my.zerotier.com)
- Enable IP Forwarding
- Configure iptables

#### Required information
For Example:

**Info**|**Example**|**Shorthand Name Below**
----|----|----
ZeroTier Network ID	|d5e04297a19bbd70	|$NETWORK_ID
ZeroTier Interface Name	|zt7nnig26	|$ZT_IFACE
Physical Interface Name	|eth0	|$PHY_IFACE
ZeroTier subnet	|172.27.0.0/16	
Physical subnet	|192.168.100.0/24	|$PHY_SUB
ZeroTier IP Address of "Router"	|172.27.0.1	|$ZT_ADDR

#### Install ZeroTier
https://www.zerotier.com/download/
```bash
sudo zerotier-cli join $NETWORK_ID
sudo zerotier-cli listnetworks
```
Authorize it at `my.zerotier.com/network/$NETWORK_ID`
The listnetworks output has the ZeroTier Interface name under `<dev>`
#### Configure the ZeroTier managed route
At `my.zerotier.com/network/$NETWORK_ID`->`Settings`->`Managed Routes`
This adds another route to every device joined to the ZeroTier network.

**Destination**|**(Via)**
----|----
$PHY_SUB	|$ZT_ADDR

For example:

**Destination**|**(Via)**
----|----
192.168.100.0/23	|172.27.0.1

Configure the destination route as slightly **larger** than the actual physical subnet, here */23* instead of */24* (a smaller number is a bigger subnet in this notation) This makes devices that are on both the physical and the ZeroTier network prefer the physical connection.
#### Enable IP forwarding
This can vary depending on linux distribution. Typically:
Edit `/etc/sysctl.conf` to uncomment `net.ipv4.ip_forward`. This enables forwarding at boot.
To enable it now
```bash
sudo sysctl -w net.ipv4.ip_forward=1
```
#### Configure iptables
Assign some shell variables (personalize these)
```bash
PHY_IFACE=eth0; ZT_IFACE=zt7nnig26
```
Add rules to iptables
```bash
sudo iptables -t nat -A POSTROUTING -o $PHY_IFACE -j MASQUERADE
sudo iptables -A FORWARD -i $PHY_IFACE -o $ZT_IFACE -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i $ZT_IFACE -o $PHY_IFACE -j ACCEPT
```
Save iptables rules for next boot
```bash
sudo apt install iptables-persistent
sudo bash -c iptables-save > /etc/iptables/rules.v4
```
#### Test
- Turn off wifi on your phone
- Join it to the zerotier network, authorize it
- Try to access something on the physical LAN

### Network Microsegmentation
#### Create a netwrok for each role
Devices can join multiple networks at once. Networks are free on my.zerotier.com. Each network can have its own [Network Flow Rules](https://docs.zerotier.com/rules). "Network A allows only RDP traffic." for example.
##### Pros
- Easy
- Automatic authorization of nodes with SSO/OIDC

##### Cons
- Multiple sets of subnets, IP addresses, etc… to maintain. Can be automated with Terraform.
- Mobile devices can connect to only 1 network at a time

##### Summary
- Create a ZeroTier network for each role: *Red, Green, and Blue*. Or: *Sales, HR, IT*. Or: *Dev, Prod, Staging*. Or: *Customer A, Customer B*
- Join shared resources to multiple networks
- Join users to the networks they need access to
![20240319095759](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240319095759.png)

#### ZeroTier Network Flow Rules
Tag network members with roles.
##### Pros
- Fine grained, low-level access control
- One network config and set of members to maintain

##### Cons
- Tricky to build rule sets
- Rules not integrated with OIDC yet

##### Summary
- Create a network
- Use the Flow Rules to segment the network

Here is the simplest possible [ZeroTier Flow Rules](https://docs.zerotier.com/rules) example. More complex rules can be mixed in with these. See the docs or contact us for help.
Replace the default rules with:
```bash
tag role id 1
    default 0
    flag 0 red
    flag 1 green
    flag 2 blue
;

drop tand role 0;

accept;
```
Devices will be able to talk only if they have at least one overlapping role. The tagging system is based on bitwise math, which we won't try to explain here. Basically: Rename "red" "green" and "blue" with your real role names. Add more roles by adding flags in increasing order: *flag 3 yellow, flag 4 indigo*
After saving a rule set with tags. A tag interface appears below:
![20240319100037](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240319100037.png)
### NAT
#### How to characterize NAT
When debugging it is often nice to be able to gather information about NAT type and behavior. ZeroTier does not use STUN (for various reasons), but many STUN implementations contain some helpful code for doing this. It's helpful to use an external utility since it's "out of band" and therefore independent of ZeroTier.
```bash
sudo apt-get install stun
sudo stun stun.callwithus.com 0
```
This will perform the basic NAT characterization test from the STUN RFC. The STUN server can be any public STUN server.
It will generate output like:
```bash
STUN client version 0.96
running test number 0
Primary: Independent Mapping, Port Dependent Filter, preserves ports, no hairpin
Return value is 0x000017
```
### Integrating with Physical Networks
ZeroTier creates networks interfaces, IP addresses, and routes on your computers. Because of this, you can use all the standard networking tools and techniques with your ZeroTier networks.
There are 2 main ways to connect your ZeroTier networks to your Physical networks: Routing and Bridging. Yes, they are technically different things. Bridging has its downsides, including that it's tricky to set up.
#### Routing or Bridging?
Most of the time, choose routing.
Do you have devices that can't install zerotier, use broadcast or multicast, and need to talk to ZeroTier nodes? Then consider bridging.
![20240319110507](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240319110507.png)
#### Routing
There are many ways to set up routing.
We have only a [Masquerade Tutorial](https://docs.zerotier.com/route-between-phys-and-virt) so far, but the steps are the same for any of these set ups; just skip the IPTables Masquerade step.
Here are some of the common examples:
##### Router can run ZeroTier
This is the best case. Since the router/firewall is the default gateway, no additional routing config needs to be done. It already knows the routes to the ZeroTier networks it's joined to.
Some examples:
- Mikrotik
- Teltonika Networks
- OpenWRT
- OPNsense

If you aren't already using existing routers, consider one of these!
![20240319110649](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240319110649.png)
##### Router can't run ZeroTier
You'll have to run ZeroTier on a device or virtual machine in your LAN. You should use Linux if at all possible. This will act as the "router" between ZeroTier and Physical networks.
##### Static Routes on Gateway
Add a Static Route to your router/gateway:
`[ZeroTier subnet] via [LAN IP of ZeroTier "router"]`
For example:
`10.147.20.0/24 via 10.0.0.2`
![20240319110829](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240319110829.png)
##### Masquerade
This is the same method that your home router uses to route between Internet and your home LAN. See the [Masquerade Tutorial](https://docs.zerotier.com/route-between-phys-and-virt)
![20240319110923](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240319110923.png)
#### Bridging
See the [Bridging Tutorial](https://docs.zerotier.com/bridging)
![20240319111011](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240319111011.png)
Some things that use broadcast:
##### Wake on LAN
Since ZeroTier is a program that runs on your computer, if the computer is off, it won't be able to receive ZeroTier packets. Wake on LAN packets need to be on the physical network. You still might not need Bridging if this is your only requirement: Run ZeroTier on a different computer on your LAN, and send the WoL packet from that computer.
##### Auto-discovery
Does your equipment come with a companion desktop or mobile application that auto-discovers the hardware on the network? This process usually uses broadcast. If the app lets you type in an IP address in the settings instead of using auto-discovery, that might be easier.
Mobile Operating Systems don't let you do broadcast over a "VPN" connection. You need run a bridge wherever the equipment is, *and* where the phone or tablet is.
##### Embedded equipment
- Industrial
- Audio / Video / Lighting equipment
- Cameras

## Self-Hosting
Take control by self-hosting your own ZeroTier infrastructure.
### Network Controller
> **INFO**
> [Network Controller Reference Documentation](https://docs.zerotier.com/what-is-a-controller)

#### Tutorial
First, skim the [README](https://github.com/zerotier/ZeroTierOne/tree/master/controller).
We're going to use `curl` to set up an example ZeroTier network. An easy way to get `curl` in Windows is to install [the latest version of Git](https://git-scm.com/downloads), which comes with bash, curl, and other tools.
> **OPENAPI**
> The setup described here uses the local [ZeroTierOne service API](https://docs.zerotier.com/api/service) to provision and manage networks. You can [browse](https://docs.zerotier.com/service/v1#tag/controller) the OpenAPI docs for the local controller API for more detail on this interface.

This is a low tech way to setup a controller for example purposes. You'd likely build yourself something fancier around this API.
##### Authtoken
You'll need to find the current local auth token on your system using the instructions in the [API Tokens](https://docs.zerotier.com/api/tokens#zerotierone-service-token) guide.
##### Get your Node ID
> **NOTE**
> Network IDs are based on the Node ID of the Controller. Controllers are nodes! When you join a network, your node finds the controller like it does with other nodes: by its Node ID.

###### with zerotier-cli
(may need sudo)
```bash
zerotier-cli info
```
###### with curl
```bash
curl "http://localhost:9993/status" -H "X-ZT1-AUTH: ${TOKEN}"
```
It's the "Address" in the above's output.
Let's save the Node ID to an environment variable too:
```bash
NODEID=your-node-id
```
or try:
```bash
NODEID=$(zerotier-cli info | cut -d " " -f 3)
```
##### Create a Network
```bash
curl -X POST "http://localhost:9993/controller/network/${NODEID}______" -H "X-ZT1-AUTH: ${TOKEN}" -d {}
```
This should return JSON for a fresh network.
> **NOTE**
> When you post to `/network/${NODEID}______` the controller generates a random Network ID for you. 
> See the "id" of your newly created network.

Let's save the new Network ID to an environment variable
```bash
NWID=your-network-id
```
##### List Networks
```bash
curl "http://localhost:9993/controller/network" -H "X-ZT1-AUTH: ${TOKEN}"
```
This returns a list of Network IDs. It should include the ID returned by the create command we did in the previous step.
##### Get Network Info
```bash
curl "http://localhost:9993/controller/network/${NWID}" -H "X-ZT1-AUTH: ${TOKEN}"
```
##### List Network Members
You'll need another node to join your network first, or this will be empty. You can use a phone, or another PC, or a VM, or a VPS...
```bash
curl "http://localhost:9993/controller/network/${NWID}/member" -H "X-ZT1-AUTH: ${TOKEN}"
```
I guess you could join the controller node to its own network, for demonstration purposes.
Save the Node ID of one of your Network Members in an env var
```bash
MEMID=a-member's-node-id
```
##### Get Member Info
```bash
curl "http://localhost:9993/controller/network/${NWID}/member/${MEMID}" -H "X-ZT1-AUTH: ${TOKEN}"
```
##### Configure the Network
For Nodes to talk, we need to add a Managed Route and IP Auto-Assign Range on the network. Let's make it a Private network too. Prefer Private networks.
```bash
curl -X POST "http://localhost:9993/controller/network/${NWID}" -H "X-ZT1-AUTH: ${TOKEN}" \
    -d '{"ipAssignmentPools": [{"ipRangeStart": "192.168.192.1", "ipRangeEnd": "192.168.192.254"}], "routes": [{"target": "192.168.192.0/24", "via": null}], "v4AssignMode": "zt", "private": true }'
```
##### Authorize a member
```bash
curl -X POST "http://localhost:9993/controller/network/${NWID}/member/${MEMID}" -H "X-ZT1-AUTH: ${TOKEN}" -d '{"authorized": true}'
```
##### Network Info Again
```bash
curl "http://localhost:9993/controller/network/${NWID}" -H "X-ZT1-AUTH: ${TOKEN}"
```
##### Member Info Again
```bash
curl "http://localhost:9993/controller/network/${NWID}/member/${MEMID}" -H "X-ZT1-AUTH: ${TOKEN}"
```
##### Confirm from your nodes
```bash
sudo zerotier-cli listnetworks
```
It should say "OK PRIVATE" and have an IP address.
##### Deauthorize Member
```bash
curl -X POST "http://localhost:9993/controller/network/${NWID}/member/${MEMID}" -H "X-ZT1-AUTH: ${TOKEN}" -d '{"authorized": false}'
```
##### Delete Member
```bash
curl -X DELETE "http://localhost:9993/controller/network/${NWID}/member/${MEMID}" -H "X-ZT1-AUTH: ${TOKEN}"
```
> **CAUTION**
> You can "delete" a member, but they will show up in the output of "list member" again if the node is still online and trying to join. You should make sure to deauthorize before deleting.

##### Backup your controller
If you want to keep these networks, copy the ZeroTier Home directory somewhere. Most importantly, the identity.secret and the controller.d directory.
##### Clean up networks
You may want to delete these networks now that you're done testing. You could use the API to delete every network.
Or you can delete the controller.d directory.
###### Linux
stop zerotier (If you're ssh'd in over zerotier, this will break your connection):
```bash
systemctl stop zerotier-one
```
delete controller configs:
```bash
cd /var/lib/zerotier-one/
# rm -rf ./controller.d/
```
start zerotier:
```bash
systemctl start zerotier-one
```
###### macOS
stop zerotier:
```bash
sudo launchctl unload /Library/LaunchDaemons/com.zerotier.one.plist
```
delete controller configs:
```bash
cd "/Library/Application Support/ZeroTier/One"
# sudo rm -rf ./controller.d
```
start zerotier:
```bash
sudo launchctl load /Library/LaunchDaemons/com.zerotier.one.plist
```
### Private Root Servers
#### Creating Your Own Roots (a.k.a. Moons)
All ZeroTier nodes on a planet effectively inhabit a single data center. This makes it easy to directly connect devices anywhere, but it has the disadvantage of not working without an Internet connection. Network connections are far from perfectly reliable, and sometimes for security reasons a user may wish to "air gap" a set of nodes from the rest of the Internet entirely.
In 1.2.0 we introduced the ability to add your own user-defined roots. Since the data center we inhabit is the planet, a user-defined set of roots is called a **moon**. When a node "orbits" a moon, it adds the moon's roots to its root server set. Nodes orbiting moons will still use planetary roots, but they'll use the moon's roots if they look faster or if nothing else is available.
The first step in creating a moon is to deploy a set of root servers. In most cases we recommend two. These are regular ZeroTier nodes, but ones that are always on and have static (physical) IP addresses. These static IPs could be global Internet IPs or physical intranet IPs that are only reachable internally. In the latter case your moon's roots won't work outside your office, but that doesn't matter. Roaming nodes will just use planetary roots instead.
We recommend that root servers do not act as network controllers, join networks, or perform any other overlapping functions. They need good reliable network connections but otherwise require very little RAM, storage, or CPU. A root could be a small VM, VPS, or cloud instance, or a small device like a [Raspberry Pi](https://www.raspberrypi.org/). If you provision your roots as VMs, take care that they do not all reside on the same physical hardware. This would defeat the purpose of having two.
The next step is to create a world definition using `zerotier-idtool`. You will need the `identity.public` files from each of your root servers. Pick one root (doesn't matter which) and run `zerotier-idtool initmoon <identity.public of one root> >>moon.json`. The `zerotier-idtool`command will output a JSON version of your world definition to *stdout*, so we redirect it to `moon.json`.
Examine this file. It will contain something like:
```json

    {
      "id": "deadbeef00",
      "objtype": "world",
      "roots": [
        {
          "identity": "deadbeef00:0:34031483094...",
          "stableEndpoints": []
        }
      ],
      "signingKey": "b324d84cec708d1b51d5ac03e75afba501a12e2124705ec34a614bf8f9b2c800f44d9824ad3ab2e3da1ac52ecb39ac052ce3f54e58d8944b52632eb6d671d0e0",
      "signingKey_SECRET": "ffc5dd0b2baf1c9b220d1c9cb39633f9e2151cf350a6d0e67c913f8952bafaf3671d2226388e1406e7670dc645851bf7d3643da701fd4599fedb9914c3918db3",
      "updatesMustBeSignedBy": "b324d84cec708d1b51d5ac03e75afba501a12e2124705ec34a614bf8f9b2c800f44d9824ad3ab2e3da1ac52ecb39ac052ce3f54e58d8944b52632eb6d671d0e0",
      "worldType": "moon"
    }
```
The world ID is technically arbitrary and could be any random 64-bit value. By convention we use the address of one of the roots.
The world definition JSON file **contains secrets**, so it's important to keep it in a safe place. The `signingKey_SECRET` is what will allow you to update your world definition automatically in the future.
Now add your other root(s) and define their stable endpoints. You'll end up with something that looks like:
```JSON
    {
      "id": "deadbeef00",
      "objtype": "world",
      "roots": [
        {
          "identity": "deadbeef00:0:34031483094...",
          "stableEndpoints": [ "10.0.0.2/9993","2001:abcd:abcd::1/9993" ]
        },
        {
          "identity": "feedbeef11:0:83588158384...",
          "stableEndpoints": [ "10.0.0.3/9993","2001:abcd:abcd::3/9993" ]
        }
      ],
      "signingKey": "b324d84cec708d1b51d5ac03e75afba501a12e2124705ec34a614bf8f9b2c800f44d9824ad3ab2e3da1ac52ecb39ac052ce3f54e58d8944b52632eb6d671d0e0",
      "signingKey_SECRET": "ffc5dd0b2baf1c9b220d1c9cb39633f9e2151cf350a6d0e67c913f8952bafaf3671d2226388e1406e7670dc645851bf7d3643da701fd4599fedb9914c3918db3",
      "updatesMustBeSignedBy": "b324d84cec708d1b51d5ac03e75afba501a12e2124705ec34a614bf8f9b2c800f44d9824ad3ab2e3da1ac52ecb39ac052ce3f54e58d8944b52632eb6d671d0e0",
      "worldType": "moon"
    }
```
The static IP addresses you use must be reachable from all the places you want these roots to serve. If you're deploying these for use at a physical location, use internal IPs. If you want them to be usable off-site, use public IPs from your DMZ or host them at a cloud provider with a data center presence close to you. Low-cost cloud hosts that provide simple static direct IP addressing and dual-stack IPv4/IPv6 support like [Digital Ocean](https://digitalocean.com/), [Vultr](https://vultr.com/), and [Linode](https://linode.com/) make ideal places to host roots. The lowest priced instances at these providers are more than sufficient in most cases.
The third step is to generate the actual signed world with `zerotier-idtool genmoon moon.json`. In this case this will generate a file called `000000deadbeef00.moon`. This does not contain secret keys but is signed by the secret from the JSON file.
Now go to your roots, create (if it does not exist) a subdirectory of their working directories (usually `/var/lib/zerotier-one` on Linux) called `moons.d`, and copy the signed moon file there. Now restart the roots and they should be ready.
You can add these roots to regular nodes in one of two ways: by placing the same world definition file in their `moons.d` directories or by using the zerotier-cli orbit command: `zerotier-cli orbit deadbeef00 deadbeef00`. The first argument is the world ID (which we can shorten by removing the two leading zeroes) and the second is the address of any of its roots. This will contact the root and obtain the full world definition from it if it's online and reachable.
Once you've "orbited" your moon, try `zerotier-cli listpeers`. You should see the roots you've created listed as `MOON` instead of `LEAF`. They will now be used as alternative root servers.

# Development
The ZeroTier agent and [Central](https://my.zerotier.com/) both provide APIs that allow you to query information about your networks and peers, as well as managing the configuration and membership of your networks.
## Central API
[Central](https://docs.zerotier.com/central) is our hosted control plane for ZeroTierOne networks.
In addition to the web-based GUI, you can use the [Central API](https://docs.zerotier.com/central/v1) to view and manage your networks. You will need an [API token](https://docs.zerotier.com/api/tokens#zerotier-central-token) for access.
You can write your own client code to connect with Central using our [examples](https://docs.zerotier.com/api/central/examples) as a starting point, or use an existing integration like our [Terraform provider](https://docs.zerotier.com/terraform).
### API Reference (V1)
https://docs.zerotier.com/central/v1/
### Central APO Examples
In the examples below use the following placeholder variables to match commonly-needed parameters:
- $ZT_TOKEN: an API token associated with an active account on [Central](https://my.zerotier.com/)
- $NWID: an active network ID

> **INFO**
> See the [Central API Tokens](https://docs.zerotier.com/api/tokens) guide for an explanation of how to create and manage API tokens.

**Exporting Data from the Central API**
The examples below are intended to run in a system terminal, and require the following command-line tools:
- [curl](https://curl.so/)
- [jq](https://jqlang.github.io/jq/)

Each of them will fetch network information and produce CSV as output. You can then import that CSV into your choice of database, spreadsheet, or configuration-management tool(s).
#### List current networks
```bash
curl -s -H "Authorization: token $ZT_TOKEN" \
  "https://api.zerotier.com/api/v1/network" \
  | jq '.[] | [
    .id,
    .config.name,
    .config.description,
    .totalMemberCount,
    .config.creationTime,
    .config.ipAssignmentPools[0].ipRangeStart,
    .config.ipAssignmentPools[0].ipRangeEnd
  ]' \
  | jq -rs '.[] | @csv'
```
#### List network members
```bash
curl -H "Authorization: token $ZT_TOKEN" \
  "https://api.zerotier.com/api/v1/network/$NWID/member" \
  | jq '.[] | [
    .id,
    .lastSeen,
    .physicalAddress,
    .ipAssignments[0],
    .name
  ]' \
  | jq -rs '.[] | @csv
```
## Service API
The ZeroTierOne service provides an API which is used by the [ZeroTierOne CLI](https://docs.zerotier.com/cli) and other clients to manage settings on your local instance of ZeroTier. This administration API is restricted to `localhost` by default, and requires authentication using an [API token](https://docs.zerotier.com/api/tokens#zerotierone-service-token).
Once you have your local token, you can access [local node, network, and peer information](https://docs.zerotier.com/service/v1), as well as configuring a [standalone controller](https://docs.zerotier.com/controller).
### API Reference (V1)
https://docs.zerotier.com/service/v1/
## Using API Token
Access to the ZeroTier APIs requires an authentication token. This guide describes the methods and tools needed to create and manage these tokens.
#### ZeroTier Central Token
To use the Central API, you need a token associated with your account. To create one, log into [my.zerotier.com](https://my.zerotier.com/) and create a named token in the [Account](https://my.zerotier.com/account) tab. Pick a memorable name that shows the intended use of the token: for example, "Terraform automation token" or "internal dev key".
![20240319140950](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240319140950.png)
Each token you create is associated with your user account, so it will allow the same level of access to manage and query networks that you have when working in the Central UI.
> **CAUTION**
> The token is displayed only once at the time you create it, so you should save it somewhere safe before clicking 'Done'.

#### ZeroTierOne Service Token
The local service API token is store in the `authtoken.secret` file in the ZeroTier service state directory. You'll need it to make API calls to the local service.
> **NOTE**
> ZeroTier generates the token at random the first time it starts. If you delete the file and restart the ZeroTierOne service a new token will be created, and the existing token will no longer be usable to access the API.

You can store the current auth token value in an environment variable for use in scripting and local development using the following terminal commands:
*Linux*: `TOKEN=$(sudo cat /var/lib/zerotier-one/authtoken.secret)`
*macOS*: `TOKEN=$(sudo cat "/Library/Application Support/ZeroTier/One/authtoken.secret")`
*Windows*: `$env.TOKEN = Get-Content C:\ProgramData\ZeroTier\One\authtoken.secret`
## Sockets(libzt)
Encrypted P2P connections for your app or service.
This guide explains how to use the ZeroTier SDK Socket API. It is meant to be read linearly and progresses from beginner topics to advanced topics. We will start by creating a simple [pingable node](https://docs.zerotier.com/sockets#pingable-node-part-1) while skipping over most of the gritty details. Then we'll move on to a full [client-server socket application](https://docs.zerotier.com/sockets#client-and-server-part-2) where we will take the occasional tangent to learn more about how all of this works. Source code for the examples can be found here: [libzt/examples](https://github.com/zerotier/libzt/tree/main/examples). For API reference documentation see the sidebar to the left. To read more more about how ZeroTier works in general, see our [Design Whitepaper](https://docs.zerotier.com/zerotier/manual). If you find an error on this page or you just need help getting something to work please open a [GitHub issue](https://github.com/zerotier/libzt/issues).
### Install
*C/C++*: `brew install zerotier/tap/libzt`
*Rust*: `# See https://crates.io/crates/libzt`
*Python*: `pip install libzt`
*C#*: `Install-Package ZeroTier.Sockets`
*Java*: `Install-Package ZeroTier.Sockets`
Alternatively, build from source: [github.com/zerotier/libzt](https://www.github.com/zerotier/libzt)
> **INFO**
> For the purposes of this guide it is recommended that you first [create an account](https://my.zerotier.com/) and a private network to test on, but it is not strictly necessary if you're comfortable joining public networks or setting up your own network controller.

### Usage summary
*C/C++*: 
```C
#include <ZeroTierSockets.h>

// ...
zts_node_start();
zts_net_join(0x1234567890abcdef);
// ...
int s = zts_socket(ZTS_AF_INET, ZTS_SOCK_STREAM, 0);
zts_connect(s, in4, adddrlen);
// ...
zts_node_stop();
```
*Rust*: 
```rust
use libzt;
use libzt::tcp::{TcpListener, TcpStream};
// ...
let node = libzt::node::ZeroTierNode {};
node.start();
node.net_join(net_id);
// ...
TcpStream::connect(remote_addr);
// ...
node.stop();
```
*Python*: 
```python
import libzt

# ...
node = libzt.ZeroTierNode()
node.node_start()
# ...
node.net_join(0x1234567890abcdef)
# ...
client = libzt.socket(libzt.ZTS_AF_INET, libzt.ZTS_SOCK_STREAM, 0)
client.connect((remote_ip, remote_port))
```
*C#*: 
```C#
using ZeroTier;
// ...
ZeroTier.Core.Node node = new ZeroTier.Core.Node();
node.Start();
// ...
node.Join(0x1234567890abcdef);
// ...
ZeroTier.Sockets.Socket sender = new ZeroTier.Sockets.Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
sender.Connect(remoteServerEndPoint);
// ...
node.Stop();
```
*Java*: 
```java
import com.zerotier.sockets.*;

// ...
ZeroTierNode node = new ZeroTierNode();
node.start();
// ...
node.join(0x1234567890abcdef);
// ...
ZeroTierSocket socket = new ZeroTierSocket(remoteAddr, port);
```
### Pingable Node (Part1)
To start things off we will:
- Set up a node
- Join it to your network
- Assign it a globally-available static IPv4 and/or IPv6 address
- Ping it

We'll be skipping over a lot of details in this first example, but we'll cover those in the [next example](https://docs.zerotier.com/sockets#client-and-server-part-2). It's also worth noting that in this first example we will be using the sequential API for simplicity but there is an [event](https://docs.zerotier.com/sockets#events) notification system that can be used as well.
#### Starting a node
Starting a node is easy and if you don't provide a configuration ZeroTier will generate a new identity automatically, but don't worry about that yet:
*C/C++*:
```c
// No "node object" creation necessary in C
zts_node_start();
```
*Rust*:
```rust
let node = libzt::node::ZeroTierNode {};
node.start();
```
*Python*:
```python
node = libzt.ZeroTierNode()
node.start()
```
*C#*:
```c#
ZeroTier.Core.Node node = new ZeroTier.Core.Node();
node.Start();
```
*Java*:
```java
ZeroTierNode node = new ZeroTierNode();
node.start();
```
Before we can join any networks or use sockets we need to know when the node has successfully come online. We will use a rather crude wait loop but as mentioned before, there are [event](https://docs.zerotier.com/sockets#events)-based methods for accomplishing this that we will discuss later:
*C/C++*:
```c
while (! zts_node_is_online()) {
  zts_util_delay(1000);
}
```
*Rust*:
```rust
while !node.is_online() {
    node.delay(1000);
}
```
*Python*:
```python
while not node.is_online():
  time.sleep(1)
```
*C#*:
```c#
while (! node.Online) {
  Thread.Sleep(1000);
}
```
*Java*:
```java
while (! node.isOnline()) {
  ZeroTierNative.zts_util_delay(1000);
}
```
Once we've broken from this loop we can be confident that the node has a valid identity and has made contact with at least one root server. At this point let's print our node's identity. This is the `public short form` of your identity that you can share with people, we will discuss how to get the `secret` portion later on:
*C/C++*: `printf("%llx\n", (long long int)zts_node_get_id());`
*Rust*: `println!("{:#06x}", node.id());`
*Python*: `print(node.get_id())`
*C#*: `Console.WriteLine(node.Id.ToString("x16"));`
*Java*: `System.out.println(Long.toHexString(node.getId()));`
#### Join a network
Now we're ready to join a network:
*C/C++*: `zts_net_join(0x1234567890abcdef);`
*Rust*: `node.net_join(0x1234567890abcdef);`
*Python*: `node.net_join(0x1234567890abcdef)`
*C#*: `node.Join(0x1234567890abcdef);`
*Java*: `node.join(0x1234567890abcdef);`
The network join request will contact our network controllers (or yours!) and if the network is `public` your node will be allowed to join immediately. If however the network is `private` the node will not be allowed to join until the network owner authorizes the node.
A node is not assigned an IP address until it has successfully joined the network. This process usually takes a few seconds, thus your application must wait until the network config is received and it has been assigned an address:
*C/C++*:
```c
while (! zts_net_transport_is_ready(net_id)) {
  zts_util_delay(1000);
}
```
*Rust*:
```rust
while !node.net_transport_is_ready(net_id) {
    node.delay(1000);
}
```
*Python*:
```python
while not node.net_transport_is_ready(net_id):
  time.sleep(1)
```
*C#*:
```c#
while (! node.IsNetworkTransportReady(networkId)) {
  Thread.Sleep(1000);
}
```
*Java*:
```java
while (! node.isNetworkTransportReady(networkId)) {
  ZeroTierNative.zts_util_delay(1000);
}
```
The transport readiness check above essentially just makes sure we have been authorized on the network and that we also have an assigned address and a managed route.
#### Getting your IP address
> **NOTE**
> Currently, only one address of each family type may be assigned (per network) to a libzt node at any time. That means one IPv4 and one IPv6 address. This is a technical limitation that will be removed in future versions.

*C/C++*:
```c
char ipstr[ZTS_IP_MAX_STR_LEN] = { 0 };
zts_addr_get_str(net_id, ZTS_AF_INET, ipstr, ZTS_IP_MAX_STR_LEN);
printf("%s\n", ipstr);
```
*Rust*:
```rust
let addr = node.addr_get(net_id).unwrap();
println!("{}", addr);
```
*Python*:
```python
print(node.addr_get_ipv4(net_id))
print(node.addr_get_ipv6(net_id))
```
*C#*:
```c#
foreach (IPAddress addr in node.GetNetworkAddresses(networkId)) {
  Console.WriteLine(addr);
}
```
*Java*:
```java
System.out.println(node.getIPv4Address(networkId).getHostAddress());
System.out.println(node.getIPv6Address(networkId).getHostAddress());
```
From another machine (or the same machine, whatever you're into), use our regular client [zerotier-one](https://www.zerotier.com/download/) (or another libzt instance) to join the same network and ping the address displayed by your new node above. You should see some jittery initial responses as our transport-triggered link is established and then once a VL2 P2P link is established the response time will stabilize.
🎉 Congratulations you've just done something cool, I guess. Ready for something with sockets?
Oh crap, I forgot. We need to stop the node when we're done:
*C/C++*: `zts_node_stop();`
*Rust*: `node.stop();`
*Python*: `node.stop()`
*C#*: `node.Stop()`
*Java*: `node.stop()`
Full example source code can be found here: [libzt/examples](https://github.com/zerotier/libzt/tree/main/examples)
### Sockets
> **TIP**
> If you're new to socket programming I highly recommend at least perusing [Beej's Guide to Network Programming](https://beej.us/guide/bgnet/). This is the best reference material you'll find on the subject. It greatly helped in my own personal understanding. A portion of our C API is merely redirected calls to [lwIP's C API](https://savannah.nongnu.org/projects/lwip/) and is thus directly compatible with what you'll learn in his guide.

Before we move onto the next section we need to quickly discuss how the socket API is structured. For each supported language ZeroTier provides a socket interface that attempts to be as idiomatic as possible. If you are using one of these non-C bindings the following text about the `C API` may not apply to you, but it's still good to know. [I don't care about this](https://docs.zerotier.com/sockets#client-and-server-part-2)
ZeroTier sockets can be controlled via `zts_bsd_` functions that operate nearly identically to normal BSD-style sockets, and `zts_` functions that provide simplified arguments and reduce the need to use things like `struct sockaddr`. These functions can be used on the same socket interchangeably without issue.
For instance:
- `zts_bsd_connect()` will behave in the same way as an ordinary `connect()` call (but over ZeroTier of course)
- `zts_connect()` will perform a little magic behind the scenes to deal with transport-triggered link provisioning. (i.e. re-attempting for you)
- `zts_tcp_client()` will wrap all of the common socket calls (including a `zts_connect()`) into a single call.

You are allowed to use each type of call and mix their usage among sockets as you please, as long as what you're doing makes sense at the protocol level. For instance the following is legal:
```c
int sock = zts_bsd_socket(ZTS_AF_INET, ZTS_SOCK_STREAM, 0);
zts_connect(sock, addr, addrlen);
zts_bsd_write(sock, buf, buflen);
```
### Client and Server (Part2)
In this section we will:

- Use most of the same setup code as the [Pingable Node](https://docs.zerotier.com/sockets#pingable-node-part-1) example
- Set up a TCP server and client
- Send a short message between the two

To see full source code of the following with proper error and exception handling, see [libzt/examples](https://github.com/zerotier/libzt/tree/main/examples) and [libzt/test](https://github.com/zerotier/libzt/tree/main/test).
Let's start our node:
*C/C++*:
```c
zts_init_from_storage(storage_path);

zts_node_start();

while (! zts_node_is_online()) {
  zts_util_delay(1000);
}

printf("Node ID: %llx\n", zts_node_get_id());

zts_net_join(net_id);

while (! zts_net_transport_is_ready(net_id)) {
  zts_util_delay(1000);
}

char ipstr[ZTS_IP_MAX_STR_LEN] = { 0 };
zts_addr_get_str(net_id, family, ipstr, ZTS_IP_MAX_STR_LEN);
printf("IP address: %s\n", net_id, ipstr);
```
*Rust*:
```rust
let node = libzt::node::ZeroTierNode {};
node.init_from_storage(&storage_path);

node.start();

while !node.is_online() {
    node.delay(1000);
}

println!("Node ID = {:#06x}", node.id());

node.net_join(net_id);

while !node.net_transport_is_ready(net_id) {
    node.delay(1000);
}
let addr = node.addr_get(net_id).unwrap();
println!("IP address: {}", addr);
```
*Python*:
```python
node = libzt.ZeroTierNode()
node.init_from_storage(storage_path)

node.node_start()

while not node.node_is_online():
  time.sleep(1)

print("Node ID:", node.node_id())

node.net_join(net_id)

while not node.net_transport_is_ready(net_id):
  time.sleep(1)

print("IP address: ", node.addr_get_ipv4(net_id))
```
*C#*:
```c#
node = new ZeroTier.Core.Node();
node.InitFromStorage(configFilePath);

node.Start();

while (! node.Online) {
  Thread.Sleep(1000);
}

Console.WriteLine("Node ID:" + node.Id)

node.Join(networkId);

while (! node.IsNetworkTransportReady(networkId)) {
  Thread.Sleep(1000);
}

foreach (IPAddress addr in node.GetNetworkAddresses(networkId)) {
  Console.WriteLine("IP address: " + addr);
}
```
*Java*:
```java
ZeroTierNode node = new ZeroTierNode();
node.initFromStorage(storagePath);

node.start();

while (! node.isOnline()) {
  ZeroTierNative.zts_util_delay(1000);
}

System.out.println("Node ID: " + Long.toHexString(node.getId()));

node.join(networkId);

while (! node.isNetworkTransportReady(networkId)) {
  ZeroTierNative.zts_util_delay(1000);
}

System.out.println("IP address: " + node.getIPv4Address(networkId).getHostAddress());
```
Notice that in the above code we use a new type of initialization function. In this case we are using a location on storage to retrieve and store our identities. We do this since we don't want to generate a unique identity for each application run. It's computationally-expensive and wasteful, think of the tardigrades.
#### TCP Server
Below is a simple blocking server that will open a listening socket, wait for a message and print what it receives. To see full source code of the following with proper error and exception handling, see [libzt/examples](https://github.com/zerotier/libzt/tree/main/examples) and[libzt/test](https://github.com/zerotier/libzt/tree/main/test).
*C/C++*:
```c
char remote_addr[ZTS_INET6_ADDRSTRLEN] = { 0 };
int remote_port = 0;
int len = ZTS_INET6_ADDRSTRLEN;

// Set up listen socket
// Note: We could also use standard zts_bsd_socket, zts_bsd_bind, etc
int fd = zts_tcp_server(local_addr, 8000, remote_addr, len, &remote_port)
printf("Accepted connection from %s:%d\n", remote_addr, remote_port);

// RX
char recvBuf[128] = { 0 };
zts_read(fd, recvBuf, sizeof(recvBuf));
printf("RX: %s\n", recvBuf);

// Cleanup
zts_close(fd);
zts_node_stop();
```
*Rust*:
```rust
// Set up listen socket
let listener = TcpListener::bind("0.0.0.0:8000").unwrap();
for stream in listener.incoming() {
    match stream {
        Ok(stream) => {
            println!("Accepted connection from: {}", stream.peer_addr().unwrap());
            thread::spawn(move || {
                handle_client(stream)
            });
        }
        Err(e) => {
            println!("Error: {}", e);
        }
    }
}
drop(listener);
```
*Python*:
```python
# Set up listen socket
serv = libzt.socket(libzt.ZTS_AF_INET, libzt.ZTS_SOCK_STREAM, 0)
serv.bind(("0.0.0.0", 8000))
serv.listen(1)
conn, addr = serv.accept()
print("Accepted connection from: ", addr)

# RX
data = conn.recv(128)
print("RX: ", data)

# Cleanup
conn.close()
node.node_stop()
```
*C#*:
```c#
// Set up listen socket
ZeroTier.Sockets.Socket listener =
  new ZeroTier.Sockets.Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
listener.Bind(localEndPoint);
listener.Listen(1);
ZeroTier.Sockets.Socket handler;
handler = listener.Accept();
Console.WriteLine("Accepted connection from: " + handler.RemoteEndPoint.ToString());

// RX
string data = null;
byte[] bytes = new Byte[128];
int bytesReceived = handler.Receive(bytes);
data = Encoding.ASCII.GetString(bytes, 0, bytesReceived);
Console.WriteLine("RX: " + data);

// Cleanup
handler.Shutdown(SocketShutdown.Both);
handler.Close();
node.Stop();
```
*Java*:
```java
// Set up listen socket
ZeroTierServerSocket listener = new ZeroTierServerSocket(port);
ZeroTierSocket conn = listener.accept();
ZeroTierInputStream inputStream = conn.getInputStream();
DataInputStream dataInputStream = new DataInputStream(inputStream);

// RX
String message = dataInputStream.readUTF();
System.out.println("RX: " + message);

// Cleanup
listener.close();
conn.close();
node.stop();
```
#### TCP Client
Below is a simple client that will connect to a remote host and send a message. To see full source code of the following with proper error and exception handling, see [libzt/examples](https://github.com/zerotier/libzt/tree/main/examples) and [libzt/test](https://github.com/zerotier/libzt/tree/main/test).
*C/C++*:
```c
char* msgStr = (char*)"Hello, network!";
int bytes = 0, fd;

// Set up connection socket
// Note: We could also use standard zts_bsd_socket, zts_bsd_connect, etc
fd = zts_tcp_client(remote_addr, remote_port);

// TX
zts_write(fd, msgStr, strlen(msgStr))

// Cleanup
zts_close(fd);
zts_node_stop();
```
*Rust*:
```rust
// Set up connection socket
match TcpStream::connect(remote_addr) {
    Ok(mut stream) => {
        // TX
        let msg = b"Hello, network!";
        stream.write(msg).unwrap();
    }
    Err(e) => {
        // Failed to connect
    }
}
```
*Python*:
```python
# Set up connection socket
client = libzt.socket(libzt.ZTS_AF_INET, libzt.ZTS_SOCK_STREAM, 0)
client.connect((remote_ip, remote_port))

# TX
data = "Hello, network!"
client.send(data.encode("utf-8"))

# Cleanup
client.close()
node.node_stop()
```
*C#*:
```c#
// Set up connection socket
byte[] bytes = new byte[128];
ZeroTier.Sockets.Socket sender =
  new ZeroTier.Sockets.Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
sender.Connect(remoteServerEndPoint);

// TX
byte[] msg = Encoding.ASCII.GetBytes("Hello, network!");
int bytesSent = sender.Send(msg);

// Cleanup
sender.Shutdown(SocketShutdown.Both);
sender.Close();
node.Stop();
```
*Java*:
```java
// Set up connection socket
ZeroTierSocket socket = new ZeroTierSocket(remoteAddr, port);
ZeroTierOutputStream outputStream = socket.getOutputStream();
DataOutputStream dataOutputStream = new DataOutputStream(outputStream);

// TX
dataOutputStream.writeUTF("Hello, network!");

// Cleanup
socket.close();
node.stop();
```

# OS / Platform Notes
## Linux
### Linux FAQ
### Snap Package
### Docker
### Layer 2 Bridge
### Amazon VPC Gateway
### NAT
### 6PLANE with Docker
### code-server + ZeroTier
### cloud-init
### Route between ZeroTier and Physical Networks
## macOS
### macOS FAQ
## Windows
### Windows FAQ
### Chocolatey
### WinGet
### LAN Game Announcements
### Unknown Node ID
## Android
### Android FAQ
## iOS / iPadOS
### iOS or iPadOS FAQ
## Routers
### Router Config Tips
### Corporate Firewalls
### Teltonika Networks
### Mikro Tik
### OpenWRT
### OPNsense
### Ubiquiti
### Route between ZeroTier and Physical Networks
## NAS
### FreeNAS
### ASUSTOR
### QNAP
### Synology
### Western Digital
## IoT
### Low Bandwidth Mode
### OS and Device Compatibility
### Layer 2 Bridge
### Layer 5 Proxy (Pylon)
### Security
### Route between ZeroTier and Physical Networks
## FreeBSD
### FreeBSD FAQ

# Service Providers
## Rules FAQ
## Security
## SSO / OIDC











