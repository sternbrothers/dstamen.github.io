---
layout: post
title: 'VCP6-DCV Section 2: Configure and Administer Advanced vSphere Networking'
date: 2015-04-06
categories:
- Certification
- VMware
status: publish
type: post
published: true
---
---Here is Section 2 of the VCP6-DCV Blueprint. Since wordpress doesn't like a bunch of indentations the formatting is strange. Here is a link to the updated PDF with both Section 1 and Section 2.

VCP6-DCV Study Guide - Section 2 (http://davidstamen.com/wp-content/uploads/2015/04/VCP6-DCV-Study-Guide-Section-2.pdf)


** Objective 2.1: Configure Advanced Policies/Features and Verify Network Virtualization Implementation
------------------------------------------------------------
* Identify vSphere Distributed Switch (vDS) capabilities
+ Consistent virtual switch and port group configuration across all hosts attached to the vDS
+ NetFlow support
+ LACP Support
+ Load balancing on physical NIC load
+ Network IO Control (NIOC)
+ Cisco Discovery Protocol (CDP) and Link Layer Discovery Protocol (LLDP) support
+ Ingress and egress traffic shaping
+ Route based on physical NIC load
+ Health check
+ vDS Configuration Export and Restore
+ Traffic filtering and marking
+ Port blocking
* Create/Delete a vSphere Distributed Switch
+ Using the vSphere Web Client navigate to the data center view. Right click and select New Distributed Switch
* Add/Remove ESXi hosts from a vSphere Distributed Switch
+ Using the vSphere Web Client navigate to the vDS. From the Actions menu select Add and Manage Hosts. Click on New Hosts and select the hosts you want to add, and then select which physical NIC’s you wish to add to the vDS.
* Add/Configure/Remove dvPort groups
+ Using the vSphere Web Client navigate to the vDS. Right click the distributed switch and select New distributed port group, fill in the name and associated configuration settings.
* Add/Remove uplink adapters to dvUplink groups
+ Using the vSphere Web Client navigate to the vDS. From the Actions menu select Add and Manage Hosts. Select Manage host networking and select Attached Hosts. From here you can manage the physical adapters to select or unselect the associated adapters.
* Configure vSphere Distributed Switch general and dvPort group settings
+ Static binding:
o Assign a port to a virtual machine when the virtual machine connects to the distributed port group.
+ Dynamic binding:
o Dynamic binding has been deprecated since ESXi 5.0.
+ Ephemeral:
o No port binding. You can assign a virtual machine to a distributed port group with ephemeral port binding also when connected to the host.
+
* Create/Configure/Remove virtual adapters
+ Using the vSphere Web Client navigate to the host. Under manage, select networking and then VMkernel adapters. Click on Add host networking and select VMKernel Adapter, from the Select an existing network select a dvPortGroup and input the configuration settings you wish.
* Migrate virtual machines to/from a vSphere Distributed Switch
+ Using the vSphere Web Client navigate to the datacenter. Right click the datacenter and select Migrate VM to another Network. From here select a source and destination and select Finish.
* Configure LACP on Uplink port groups
+ Using the vSphere Web Client navigate to the vDS. Select Manage and then settings and then LACP. Here you can configure LACP with the following settings.
o LAG Mode:
# Passive
@ LAG ports respond to LACP packets they receive but do not initiate LACP negotiations.
# Active
@ LAG ports in active mode. LAG ports initiate negotiations with LACP Port Channel.
# LAG load balancing mode:
@ Source and destination IP address, TCP/UDP port and VLAN
@ Source and destination IP address and VLAN
@ Source and destination MAC address
@ Source and destination TCP/UDP port
@ Source port ID
@ VLAN
# Describe vDS Security Polices/Settings
@ Promiscuous Mode – Default: Reject
- Reject — Placing a guest adapter in promiscuous mode has no effect on which frames are received by the adapter.
- Accept — Placing a guest adapter in promiscuous mode causes it to detect all frames passed on the vSphere standard switch that are allowed under the VLAN policy for the port group that the adapter is connected to.
@ Mac Address Changes – Default: Reject
- Reject — If you set the MAC Address Changes to Reject and the guest operating system changes the MAC address of the adapter to anything other than what is in the .vmx configuration file, all inbound frames are dropped.
- Accept — Changing the MAC address from the Guest OS has the intended effect: frames to the new MAC address are received.
@ Forged Transmits – Default: Reject
- Reject — Any outbound frame with a source MAC address that is different from the one currently set on the adapter are dropped.
- Accept — No filtering is performed and all outbound frames are passed.
@ Configure dvPort group blocking policies
- Port blocking policies allow you to selectively block ports from sending or receiving data.
= Using the vSphere Web client navigate to the vDS. Right cick the vDS and select Manage Distributed Port Groups. Select the miscellaneous check box and you can enable or disable blocking all ports.
= To configure this for a single port you can navigate to the dvPort Group and then select the manage tab on the ports view and block a single port.
- Configure load balancing and failover policies
= You can configure various load balancing algorithms on a virtual switch to determine how network traffic is distributed between the physical NICs in a team.
* Route based on IP hash
+ The virtual switch selects uplinks for virtual machines based on the source and destination IP address of each packet.
* Route based on source MAC hash
+ The virtual switch selects an uplink for a virtual machine based on the virtual machine MAC address. To calculate an uplink for a virtual machine, the virtual switch uses the virtual machine MAC address and the number of uplinks in the NIC team.
* Route based on originating virtual port
+ The virtual switch selects uplinks based on the virtual machine port IDs on the vSphere Standard Switch or vSphere Distributed Switch.
* Use explicit failover order
+ No actual load balancing is available with this policy. The virtual switch always uses the uplink that stands first in the list of Active adapters from the failover order and that passes failover detection criteria. If no uplinks in the Active list are available, the virtual switch uses the uplinks from the Standby list.
* Route based on physical NIC load
+ Route Based on Physical NIC Load is based on Route Based on Originating Virtual Port, where the virtual switch checks the actual load of the uplinks and takes steps to reduce it on overloaded uplinks. Available only for vSphere Distributed Switch.
* Configure VLAN/PVLAN settings
+ There are 4 types of options you can set here.
o None
# Do not use VLAN.
@ Use this option in case of External Switch Tagging
# VLAN
@ Tag traffic with the ID from the VLAN ID field.
- Type a number between 1 and 4094 for Virtual Switch Tagging.
@ VLAN Trunking
- Pass VLAN traffic with ID within the VLAN trunk range to guest operating system. You can set multiple ranges and individual VLANs by using a comma-separated list.
= Use this option for Virtual Guest Tagging
- Private VLAN
= Associate the traffic with a private VLAN created on the distributed switch. There are three types of Private VLANs
* Promiscuous
+ Can talk to everything
* Community
+ Can only talk to promiscuous ports and VMs within the same community.
* Isolated
+ Can only talk to promiscuous
* Configure traffic shaping policies
+ A traffic shaping policy is defined by average bandwidth, peak bandwidth, and burst size. You can establish a traffic shaping policy for each port group and each distributed port or distributed port group.
+ To configure traffic shaping, navigate to the host and then select manage, networking and then virtual switches, under the traffic shaping policy you can configure it based on below settings.
o Average Bandwidth
# Establishes the number of bits per second to allow across a port, averaged over time. This number is the allowed average load.
o Peak Bandwidth
# Maximum number of bits per second to allow across a port when it is sending or receiving a burst of traffic. This number limits the bandwidth that a port uses when it is using its burst bonus.
o Burst Size
# Maximum number of bytes to allow in a burst. If this parameter is set, a port might gain a burst bonus if it does not use all its allocated bandwidth. When the port needs more bandwidth than specified by the average bandwidth, it might be allowed to temporarily transmit data at a higher speed if a burst bonus is available. This parameter limits the number of bytes that have accumulated in the burst bonus and transfers traffic at a higher speed.
o Enable TCP Segmentation Offload support for a virtual machine
# Use TCP Segmentation Offload (TSO) in VMkernel network adapters and virtual machines to improve the network performance in workloads that have severe latency requirements.
@ This can only be done using the command line and cab be done as follows.
- Enable the software simulation of TSO in the VMkernel.
= esxcli network nic software set --ipv4tso=0 -n vmnicX
= esxcli network nic software set --ipv6tso=0 -n vmnicX
- Disable the software simulation of TSO in the VMkernel.
= esxcli network nic software set --ipv4tso=1 -n vmnicX
= esxcli network nic software set --ipv6tso=1 -n vmnicX
- Enable Jumbo Frames support on appropriate components
= Jumbo frames let ESXi hosts send larger frames out onto the physical network. The network must support jumbo frames end-to-end that includes physical network adapters, physical switches, and storage devices.
* Using the vSphere client navigate to the vDS. Select the manage tab and then settings and properties. Edit the vDS and under Advanced, MTU set a number higher than 9000 bytes.
= Determine appropriate VLAN configuration for a vSphere implementation
* vSphere supports three modes of VLAN tagging in ESXi: External Switch Tagging (EST), Virtual Switch Tagging (VST), and Virtual Guest Tagging (VGT).
+ EST
o The physical switch performs the VLAN tagging. The host network adapters are connected to access ports on the physical switch.
+ VST
o The virtual switch performs the VLAN tagging before the packets leave the host. The host network adapters must be connected to trunk ports on the physical switch.
+ VGT
o The virtual machine performs the VLAN tagging. The virtual switch preserves the VLAN tags when it forwards the packets between the virtual machine networking stack and external switch. The host network adapters must be connected to trunk ports on the physical switch. The vSphere Distributed Switch supports a modification of VGT. For security reasons, you can configure a distributed switch to pass only packets that belong to particular VLANs.
# NOTE For VGT you must have an 802.1Q VLAN trunking driver installed on the guest operating system of the virtual machine.


** Objective 2.2: Configure Network I/O Control (NIOC)
------------------------------------------------------------
* Identify Network I/O Control requirements
+ Verify that the vSphere Distributed Switch is version 6.0.0.
+ Verify that the Network I/O Control feature of the distributed switch is version 2.
+ Verify that you have the dvPort group.Modify privilege on the distributed port groups on the switch.
+ Verify that all hosts on the switch are connected to vCenter Server.
* Identify Network I/O Control capabilities
+ Bandwidth shares, reservation, and limits for the following classes of traffic:
o Management traffic
o VM traffic
o NFS traffic
o Virtual SAN traffic
o iSCSI traffic
o vMotion traffic
o vSphere replication (VR) traffic
o Fault tolerance (FT) traffic
o vSphere data protection backup traffic
+ Bandwidth reservations can be used to isolate network resources for a class of traffic. As an example, consider a Virtual SAN cluster where the same device may be used for all of the above classes of traffic. In this case, a chunk of bandwidth may be reserved for Virtual SAN traffic to ensure steady Virtual SAN performance, independent of what other factors are driving traffic in the Virtual SAN cluster.
+ Bandwidth shares, reservation, and limit per VM vNIC. This feature allows network resource isolation for VM vNICs.
+ Load balancing of VMs by DRS during VM power on. This DRS feature will take into account network bandwidth reservations in addition to CPU and memory reservations while recommending which host a VM should be powered on. However, if DRS is not enabled in the cluster, you will not be able to power on those VMs whose bandwidth reservations cannot be met due to bandwidth reservations on competing VMs on the same host.
+ Load balancing of powered on VMs when bandwidth reservations are violated. If the network reservation of a powered on VM is reconfigured, this DRS feature migrates the VM to a different host where the bandwidth reservation can now be met.
o Note: NetIOC is only available on a distributed virtual switch (DVS). The features of load balancing require that you enable DRS.
+ Enable/Disable Network I/O Control
o Using the vSphere Web client navigate to the vDS. From the Actions menu, select Edit Settings, from the Network I/O Control drop down select Enable/Disable.
+ Monitor Network I/O Control
o Network I/O Control can be monitored in the vSphere Web Client.
# Using the vSphere Web Client nagivate to the vDS, then select Manage, Resource Allocation
@ From here you can see the status, Version, number of included adapters and the minimum link speed among the current available bandwidth.


** Objective 2.3: Configure vSS and vDS Policies
------------------------------------------------------------
* Identify common vSS and vDS policies
+ vSS
o General
# # of ports
# MTU
o Security
# Promiscuous Mode
# MAC Address Changes
# Forged Transmits
o Traffic Shaping (Outbound)
# Status (Enable or Disable)
# Average Bandwidth
# Peak Bandwidth
# Burst Size
o NIC Teaming
# Load Balancing
# Network Failover Detection
# Notify Switches
# Failback
# Failover Order
o vDS
# General
@ Name
@ # of ports
@ Port Binding
# Security
@ Promiscuous Mode
@ MAC Address Changes
@ Forged Transmits
# Traffic Shaping (Inbound and Outbound)
@ Status (Enable or Disable)
@ Average Bandwidth
@ Peak Bandwidth
@ Burst Size
# VLAN
@ VLAN Type for vDS
- None
- VLAN
- VLAN Trunking
@ Teaming and Failover
- Load Balancing
- Network Failover Detection
- Notify Switches
- Failback
- Failover Order
@ Resource Allocation
- NIOC Pool
@ Monitoring
- NetFlow Configuration
@ Miscellaneous
- Block All Ports
@ Advanced
- Configure Override settings
@ Describe vDS Security Polices/Settings
- Promiscuous Mode – Default: Reject
= Reject — Placing a guest adapter in promiscuous mode has no effect on which frames are received by the adapter.
= Accept — Placing a guest adapter in promiscuous mode causes it to detect all frames passed on the vSphere standard switch that are allowed under the VLAN policy for the port group that the adapter is connected to.
- Mac Address Changes – Default: Reject
= Reject — If you set the MAC Address Changes to Reject and the guest operating system changes the MAC address of the adapter to anything other than what is in the .vmx configuration file, all inbound frames are dropped.
= Accept — Changing the MAC address from the Guest OS has the intended effect: frames to the new MAC address are received.
- Forged Transmits – Default: Reject
= Reject — Any outbound frame with a source MAC address that is different from the one currently set on the adapter are dropped.
= Accept — No filtering is performed and all outbound frames are passed.
- Configure dvPort group blocking policies
= Port blocking policies allow you to selectively block ports from sending or receiving data.
* Using the vSphere Web client navigate to the vDS. Right cick the vDS and select Manage Distributed Port Groups. Select the miscellaneous check box and you can enable or disable blocking all ports.
* To configure this for a single port you can navigate to the dvPort Group and then select the manage tab on the ports view and block a single port.
= Configure load balancing and failover policies
* Route based on IP hash
+ The virtual switch selects uplinks for virtual machines based on the source and destination IP address of each packet.
* Route based on source MAC hash
+ The virtual switch selects an uplink for a virtual machine based on the virtual machine MAC address. To calculate an uplink for a virtual machine, the virtual switch uses the virtual machine MAC address and the number of uplinks in the NIC team.
* Route based on originating virtual port
+ The virtual switch selects uplinks based on the virtual machine port IDs on the vSphere Standard Switch or vSphere Distributed Switch.
* Use explicit failover order
+ No actual load balancing is available with this policy. The virtual switch always uses the uplink that stands first in the list of Active adapters from the failover order and that passes failover detection criteria. If no uplinks in the Active list are available, the virtual switch uses the uplinks from the Standby list.
* Route based on physical NIC load
+ Route Based on Physical NIC Load is based on Route Based on Originating Virtual Port, where the virtual switch checks the actual load of the uplinks and takes steps to reduce it on overloaded uplinks. Available only for vSphere Distributed Switch.
* Configure VLAN/PVLAN settings
+ There are 4 types of options you can set here.
o None
# Do not use VLAN.
@ Use this option in case of External Switch Tagging
# VLAN
@ Tag traffic with the ID from the VLAN ID field.
- Type a number between 1 and 4094 for Virtual Switch Tagging.
@ VLAN Trunking
- Pass VLAN traffic with ID within the VLAN trunk range to guest operating system. You can set multiple ranges and individual VLANs by using a comma-separated list.
= Use this option for Virtual Guest Tagging
- Private VLAN
= Associate the traffic with a private VLAN created on the distributed switch. There are three types of Private VLANs
* Promiscuous
+ Can talk to everything
* Community
+ Can only talk to promiscuous ports and VMs within the same community.
* Isolated
+ Can only talk to promiscuous
* Configure traffic shaping policies
+ A traffic shaping policy is defined by average bandwidth, peak bandwidth, and burst size. You can establish a traffic shaping policy for each port group and each distributed port or distributed port group.
+ To configure traffic shaping, navigate to the host and then select manage, networking and then virtual switches, under the traffic shaping policy you can configure it based on below settings.
o Average Bandwidth
# Establishes the number of bits per second to allow across a port, averaged over time. This number is the allowed average load.
o Peak Bandwidth
# Maximum number of bits per second to allow across a port when it is sending or receiving a burst of traffic. This number limits the bandwidth that a port uses when it is using its burst bonus.
o Burst Size
# Maximum number of bytes to allow in a burst. If this parameter is set, a port might gain a burst bonus if it does not use all its allocated bandwidth. When the port needs more bandwidth than specified by the average bandwidth, it might be allowed to temporarily transmit data at a higher speed if a burst bonus is available. This parameter limits the number of bytes that have accumulated in the burst bonus and transfers traffic at a higher speed.
o Enable TCP Segmentation Offload support for a virtual machine
# Use TCP Segmentation Offload (TSO) in VMkernel network adapters and virtual machines to improve the network performance in workloads that have severe latency requirements.
@ This can only be done using the command line and cab be done as follows.
- Enable the software simulation of TSO in the VMkernel.
= esxcli network nic software set --ipv4tso=0 -n vmnicX
= esxcli network nic software set --ipv6tso=0 -n vmnicX
- Disable the software simulation of TSO in the VMkernel.
= esxcli network nic software set --ipv4tso=1 -n vmnicX
= esxcli network nic software set --ipv6tso=1 -n vmnicX
- Enable Jumbo Frames support on appropriate components
= Jumbo frames let ESXi hosts send larger frames out onto the physical network. The network must support jumbo frames end-to-end that includes physical network adapters, physical switches, and storage devices.
* Using the vSphere client navigate to the vDS. Select the manage tab and then settings and properties. Edit the vDS and under Advanced, MTU set a number higher than 9000 bytes.
= Determine appropriate VLAN configuration for a vSphere implementation
* vSphere supports three modes of VLAN tagging in ESXi: External Switch Tagging (EST), Virtual Switch Tagging (VST), and Virtual Guest Tagging (VGT).
+ EST
o The physical switch performs the VLAN tagging. The host network adapters are connected to access ports on the physical switch.
+ VST
o The virtual switch performs the VLAN tagging before the packets leave the host. The host network adapters must be connected to trunk ports on the physical switch.
+ VGT
o The virtual machine performs the VLAN tagging. The virtual switch preserves the VLAN tags when it forwards the packets between the virtual machine networking stack and external switch. The host network adapters must be connected to trunk ports on the physical switch. The vSphere Distributed Switch supports a modification of VGT. For security reasons, you can configure a distributed switch to pass only packets that belong to particular VLANs.
+ NOTE For VGT you must have an 802.1Q VLAN trunking driver installed on the guest operating system of the virtual machine.
