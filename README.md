# VLAN configuration Homelab

This homelab simulates a small office network architecture using Cisco Packet Tracer. The goal was to implement VLAN segmentation to isolate traffic between departments (Office, HR, IT) and integrate a dedicated Voice VLAN for IP phones. The topology consists of a core switch connected to two access switches, each managing multiple PCs and IP phones. Connectivity between the devices was tested to ensure proper isolation and functionality.

This project helped reinforce my understanding of VLAN configuration, trunking, and port-based network segmentation in a structured enterprise-like setup.

 Objectives
Design a logical network using four VLANs:

VLAN 10 – Office Staff

VLAN 20 – HR Department

VLAN 30 – IT Department

VLAN 40 – Voice (IP Phones)

Implement trunking between core and access switches

Assign static IPs to end devices

Test VLAN isolation and inter-device communication

Tools & Software
Cisco Packet Tracer
Used for simulating the full network environment, including switch configuration and end device interaction.

Lab Topology Setup
Devices Used:

3 Cisco Switches:

CoreSwitch (Central aggregation switch)

Access1 (Hosts Office and HR VLANs)

Access2 (Hosts IT VLAN)

5 PCs

2 IP Phones

Connectivity Overview:

PCs and IP Phones connect to their respective access switches

IP Phones are daisy-chained with PCs using integrated switch ports

Trunk links connect Access1 and Access2 to CoreSwitch

Configuration
```
Configure the Core Switch
1. Click on Switch-2, go to the CLI tab.
2. Configure the hostname for the core switch
Switch-2> enable
Switch-2# configure terminal

Switch-2(config)# hostname CoreSwitch
CoreSwitch(config)#
3. Create VLANs for the following: 10 - Office, 20 - HR, 30 - IT , 40 - Voice
CoreSwitch(config)# vlan 10
CoreSwitch(config-vlan)# name Office
CoreSwitch(config-vlan)# vlan 20
CoreSwitch(config-vlan)# name HR
CoreSwitch(config-vlan)# vlan 30
CoreSwitch(config-vlan)# name IT
CoreSwitch(config-vlan)# vlan 40
CoreSwitch(config-vlan)# name Voice
CoreSwitch(config-vlan)# exit
4. Configure Trunk ports on Core Switch
Trunk ports allow multiple VLANs to pass through a single link between
switches.
Setup Trunk port for Access Switch 1
CoreSwitch(config)# int gi0/1
CoreSwitch(config-if)#switchport mode trunk
CoreSwitch(config-if)#switchport trunk allowed vlan
all

Setup Trunk port for Access Switch 2
CoreSwitch(config)# int gi0/2
CoreSwitch(config-if)#switchport mode trunk
CoreSwitch(config-if)#switchport trunk allowed vlan
all
5. Save the configuration:
CoreSwitch# write memory
Step 4: Configure VLANs on Access Switch 1
1. Click on Switch-0, go to the CLI tab.
2. Configure the hostname for the core switch
Switch-0> enable
Switch-0# configure terminal
Switch-0(config)# hostname Access1
3. Create VLANs for the following: 10 - Office, 20 - HR , 40 - Voice
CoreSwitch(config)# vlan 10
CoreSwitch(config-vlan)# name Office
CoreSwitch(config-vlan)# vlan 20
CoreSwitch(config-vlan)# name HR
CoreSwitch(config-vlan)# vlan 40
CoreSwitch(config-vlan)# name Voice
CoreSwitch(config-vlan)# exit

4. Configure the uplink to the core switch as a trunk:
Access1(config)# int gi0/1
Access1(config-if)#switchport mode trunk
Access1(config-if)#switchport trunk allowed vlan
10,20,40
5. Assign access and voice ports to VLAN 10 for office computers:
Access1(config)# int range fa0/1-5
Access1(config-if)#switchport mode access
Access1(config-if)#switchport access vlan 10
Access1(config-if)#switchport voice vlan 40
6. Assign access and voice ports to VLAN 20 for HR computers:
Access1(config)# int range fa0/6-10
Access1(config-if)#switchport mode access
Access1(config-if)#switchport access vlan 20
Access1(config-if)#switchport voice vlan 40
7. Save the configuration:
Access1# write memory

Step 5: Configure VLANs on Access Switch 2
1. Click on Switch-0, go to the CLI tab.
2. Configure the hostname for the core switch
Switch-1> enable
Switch-1# configure terminal
Switch-1(config)# hostname Access2
3. Create VLANs for the following: 30 - IT
Access2(config)# vlan 30
Access2(config-vlan)# name IT
Access2(config-vlan)# exit
4. Configure the uplink to the core switch as a trunk:
Access2(config)# int gi0/2
Access2(config-if)#switchport mode trunk
Access2(config-if)#switchport trunk allowed vlan 30
5. Assign access and voice ports to VLAN 30 for IT computers:
Access2(config)# int range fa0/1-5
Access2(config-if)#switchport mode access
Access2(config-if)#switchport access vlan 30

6. Save the configuration:
Access2# do wr mem
```
 IP Configuration
Each PC was manually assigned a static IP within the correct subnet corresponding to its VLAN. This setup ensured precise control over address management during the lab.

Connectivity Testing
Same VLAN:
Devices in the same VLAN were able to ping each other successfully.

Different VLANs:
As expected, devices from different VLANs were unable to communicate directly, confirming proper network isolation and segmentation.

Key Learnings & Takeaways:
VLAN Trunking is essential for allowing inter-switch VLAN traffic over a single link.

Implementing Voice VLANs ensures separation of VoIP traffic from data traffic while sharing the same physical port.

A properly configured homelab environment using Packet Tracer can effectively simulate real-world enterprise LAN setups.

Understanding port roles (access vs trunk) is critical for achieving both isolation and connectivity across layers.

Why This Project Stands Out
I didn’t just follow a checklist—I understood how VLANs impact security, traffic flow, and scalability.
My setup reflects real business needs: separation of duties, voice and data traffic optimization, and network resilience.
I approached the problem with troubleshooting in mind—testing scenarios for both success and failure.
Documentation and structure were priorities throughout, just like in any real-world IT environment.

