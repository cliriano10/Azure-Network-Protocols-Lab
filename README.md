# Azure-Network-Protocols-Lab
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups

![68747470733a2f2f692e696d6775722e636f6d2f55613775646f532e706e67](https://github.com/cliriano10/Azure-Network-Protocols-Lab/assets/159824067/75eed1dc-8b76-4652-8752-29bee95d7fe9)


Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines

In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups.
Environments and Technologies Used
Microsoft Azure (Virtual Machines)
Remote Desktop
Various Command-Line Tools
Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
Wireshark (Protocol Analyzer)

Operating Systems Used
Windows 10 (21H2)
Ubuntu Server 20.04

Actions and Observations
Welcome to my tutorial on Network Security Groups and Inspecting Network Protocols. First you will need to create two VMs on Azure. One machine will be a Linux machine and the other will be a Windows 10 machine. Both will have two cpus and they must be on the same VNET. Once that is done go on the Windows machine and download Wireshark. I will attatch a link to the wireshark download. https://www.wireshark.org/download.html Once installed open Wireshark and filter for ICMP Traffic only. ICMP is a network layer protocol that relays messages concerning network connection issues. Ping uses this protocol, ping tests connectivity between hosts. When we filter wirehsark to only capture ICMP packets and ping the private IP address of our linux machine we can visually see the packets on wireshark.

![68747470733a2f2f692e696d6775722e636f6d2f494955536878702e706e67](https://github.com/cliriano10/Azure-Network-Protocols-Lab/assets/159824067/c28a9019-2b11-40ce-b752-41525415a7d2)



We can inspect each individual packet and see the actual data that is being sent in each ping. the picture below demonstrates just that.

![68747470733a2f2f692e696d6775722e636f6d2f474c78534947332e706e67](https://github.com/cliriano10/Azure-Network-Protocols-Lab/assets/159824067/9433ce16-784c-4735-b787-3f64de9e91a7)



In the next portion of the lab we will perpetually ping the Linux machine with the command ping -t. This will continually ping the machine until we decide to stop it, while the Windows machine is pinging the Linux machine we will go to the Linux machine and block inbound ICMP traffic on it's firewall. Once we do that we will stop recieving echo replys from the Linux machine. We will block ICMP by creating a new Network Security Group on the Linux machine that will be set to block ICMP. We can allow the traffic by allowing ICMP on the Linux Network Security Groups page on Azure.



![68747470733a2f2f692e696d6775722e636f6d2f3576584f3735522e706e67](https://github.com/cliriano10/Azure-Network-Protocols-Lab/assets/159824067/415398cb-e300-4282-985c-313e7dec79d4)

![68747470733a2f2f692e696d6775722e636f6d2f41736c3830744e2e706e67](https://github.com/cliriano10/Azure-Network-Protocols-Lab/assets/159824067/1bccd190-ff9a-4c2e-b58f-9c1654e87683)


Next we will use our Windows machine to SSH to the Linux machine. SSH has no GUI it just gives the user access to the machines CLI. We will set the wireshark filter to capture SSH packets only. When we ssh into the Linux machine with the command prompt "ssh labuser@10.0.0.5" we can see that wireshark starts to immediately capture SSH packets.

![68747470733a2f2f692e696d6775722e636f6d2f7a7465523431722e706e67](https://github.com/cliriano10/Azure-Network-Protocols-Lab/assets/159824067/d8b67420-7929-4009-905f-36e479827e6f)




Now we will use wireshark to filter for DHCP. DHCP is the Dynamic Host Configuration Protocol this works on ports 67/68. It is used to assign IP addresses to machines. We will request a new ip address with the command "ipconfig /renew". Once we enter the command wireshark will capture DHCP traffic.

![68747470733a2f2f692e696d6775722e636f6d2f765538667051662e706e67](https://github.com/cliriano10/Azure-Network-Protocols-Lab/assets/159824067/8039cdb8-2fc6-48fc-b911-e96c48159d3a)



Time to filter DNS traffic. We will set wireshark to filter DNS traffic. We will initiate DNS traffic by typing in the command "nslookup www.google.com" this command essentially asks our DNS server what is google's IP address.
![68747470733a2f2f692e696d6775722e636f6d2f564d63776d734f2e706e67](https://github.com/cliriano10/Azure-Network-Protocols-Lab/assets/159824067/56b0866d-ae52-458a-8af2-52506d9b7f81)




Lastly we will filter for RDP traffic. When we enter tcp.port==3389 traffic is spammed non stop because we are using Remote Desktop Protocol to connect to our Virtual Machine.


![68747470733a2f2f692e696d6775722e636f6d2f567858477636582e706e67](https://github.com/cliriano10/Azure-Network-Protocols-Lab/assets/159824067/e0b2435b-659b-49e2-b19d-bf83f789af6a)
