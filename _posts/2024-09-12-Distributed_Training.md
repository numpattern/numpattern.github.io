---
layout: post
title: Distributed Training
#subtitle: An essential part of Resources Evaluation. GSLIB Cell Based Method.
tags: [Distributed Training]
bigimg: /img/per010rz.jpg
share-img: /img/abstract_bg_cuda.png.PNG
---

Distributed training is the process of partitioning the workload 
across multiple processing units to speed up the training especially
in ML.

The Internet Protocol (IP) addresses are unique labels assigned to each device connected to a network. IPs
 are the rules to govern how data is sent or received across the internet or a local network. IPv4 is the 
 fourth version, IPv6 is newer and not fully adopted. In IPv4, the network ID 192.168.subnetwork.device is denotes a range 
 of IP addresses for private use within local networks to communicate. Public addresses do not start with 192.168.
 
After you log into a computer, the code below summarizes the network interfaces. An interface in this context 
refers to hardware and software components that allow a device to communicate over a network. 

Hardware Interface:
Network Interface Card (NIC): A physical hardware component, like an Ethernet card or Wi-Fi adapter, that connects a computer to a network.
Cables: Physical connections, such as Ethernet cables to link devices to a network.

Software Interface:
Network Interface: A software abstraction that represents the hardware interface in the operating system. It handles the communication protocols and data transfer.
Virtual Interfaces: Software-based interfaces, such as virtual network adapters used in virtual machines or VPN connections.

{% highlight python linenos %}
import socket
import subprocess

def get_ip_addresses():
    hostname = socket.gethostname()
    local_ip = socket.gethostbyname(hostname)
    result = subprocess.run(['arp', '-a'], capture_output=True, text=True)  
    ip_list = [line for line in result.stdout.split('\n')]
    
    return ip_list

for ip in get_ip_addresses():
    print(ip)

ip_addresses = get_ip_addresses()
for ip in ip_addresses:
    print(ip)
{% endhighlight %}

The interface is described as IP 192.168.1.5 and the hexadecimal 0x6 refers to the network interface. 
Physical address are the MAC addresses (Media Access Control from a device) that correspond to each IP. Type is the ARP entry.
MAC: Media Access Control, aims to reach the correct device when the data is sent over. 
**Dynamic IP:** if you unplug a device with dynamic IP, it might receive a different IP address after reconnecting
**Static IP:** If you unplug printer and plug it back, it will still have same IP as long as the settings haven’t been changed.

{% highlight python linenos %}
Interface: 192.168.1.5 --- 0x6
  Internet Address      Physical Address      Type
  192.168.1.1           00-84-1e-76-26-b3     dynamic   
  192.168.2.3           e8-7c-25-50-b0-bt     static   
{% endhighlight %}  

If two devices are in two different sub-networks, this can limit the communication, as devices on different sub-networks typically require a router to facilitate communication.
Multiple Network Interfaces: The presence of different hexadecimal identifiers suggests that your device has multiple network interfaces, each with its own IP address. This is common in systems with both wired and wireless connections, or in servers with multiple network cards.
Run ipconfig to see if the description of the Network Interface says Ethernet (wired) or WIFI (wireless).





e.g each printer will have a unique MAC address even if there are from same brand and model.