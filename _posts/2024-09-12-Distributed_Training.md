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
    
    ip_list = []
    for line in result.stdout.split('\n'):
        ip_list.append(line)
    
    return ip_list

ip_addresses = get_ip_addresses()
for ip in ip_addresses:
    print(ip)
{% endhighlight %}



The interface is described here with the IP 192.168.1.11 and 0x6 hexadecimal identifier for the network interface. 
Physical address lists the MAC addresses (Media Access Control from a device) corresponding to IP addresses. Type: indicates the type of ARP entry.

MAC: Media Access Control, aims to reach the correct device when the data is sent over.