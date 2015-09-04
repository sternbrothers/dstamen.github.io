---
layout: post
title: 'VCP6-DCV Section 1: Configure and Administer vSphere Security'
date: 2015-04-03
categories:
- Certification
- VMware
status: publish
type: post
published: true
---
So everyone seemed to enjoy my post (http://davidstamen.com/2015/04/01/vcp6-dcv-beta-exam/)  where I created a PDF blueprint. Here i will also be posting updating information as I build out content for the blueprint. For this first post we will focus on Section 1, some of the content here is similar to 5.5 but for 6.0 there are a few new entries such as managing the VMCA and PSC and other entries were very hard to find even using the suggested "Tools" links to the documentation. I hope you find this helpful and any feedback is appreciated. At the bottom of the post you will see an updated version of the VCP6-DCV PDF with this content.


** Section 1: Configure and Administer vSphere Security
------------------------------------------------------------


** Objective 1.1: Configure and Administer Role-based Access Control
------------------------------------------------------------
* Identify common vCenter Server privileges and roles
+ Administrator
o Used to provide Full Administrative access to all vCenter objects
+ Read-Only
o Used to provide Read-Only access to all vCenter objects
+ No Access
o Used to Deny Access to vCenter objects
+ Tagging Admin
o Used to Create, Edit, Modify and Delete vCenter Tags
+ Virtual machine power user
o Modify existing Virtual Machines
+ Network administrator
o Assign network
+ Datastore consumer
o Allocate Datastore space
+ Describe how permissions are applied and inherited in vCenter Server
o You can use global permissions to give a user or group privileges for all objects in all inventory hierarchies in your deployment. Use this with care; verify you really want to assign permission to all inventory hierarchies.
# Log in to the vSphere Web Client and Select Administration and Global Permissions. Select the + Sign and you can add users and assign the associated role.
o View/Sort/Export user and group lists
# You can view, sort and export lists of a host’s local users to a file that is HTML, XML, Excel or CSV Format.
@ Log in to ESXi using the vSphere client. Click on Local Users & Groups and select Users. Right click and select Export List.
# Add/Modify/Remove permissions for users and groups on vCenter Server inventory objects
@ Unlike Global Permissions you can assign permissions on individual objects
- Log in to the vSphere Web Client and Browse to the associated object you wish to assign permissions to. Select Manage and then Permissions, from here you can assign the user or group the associated permission.
@ Create/Clone/Edit vCenter Server Roles
- You can make a copy of an existing role, rename it and edit it. When you make a copy the new role is not applied to any users or groups and objects. You must assign it.
= To manage roles, you can log in to the vSphere Web Client and choose Administration and then Roles. You cannot edit any of the default roles, but you can clone them and make your changes here.
- Determine the correct roles/privileges needed to integrate vCenter Server with other VMware products
= Some environments you may not want to provide full administrative access to a service account, using that account and custom roles and permissions you can limit to only the needed roles/privileges. This settings can commonly be found in the products documentation
- Determine the appropriate set of privileges for common tasks in vCenter Server
= You can easily identify what privileges are needed for common tasks by creating a new role and reviewing the permission tree. Permissions are grouped by All/Alarms/AutoDeploy/Certificates/Content Library/Datacenter/Datastore/Datastore Cluster/Distributed Switch/ESX Agent Manager/Extension/Folder/Global/Host/Host profile/Inventory Service/Network/Performance/Permissions/Profile-driven storage/Resource/Scheduled task/Sessions/Storage views/Tasks/Transfer service/VRMPolicy/Virtual machine/dvPort Group/vApp and vService. Based on this granularity you can easily limit specific access to almost any vCenter object.


** Objective 1.2: Secure ESXi, vCenter Server, and vSphere Virtual Machines
------------------------------------------------------------
* Enable/Configure/Disable services in the ESXi firewall
+ You can configure firewall properties to allow or deny access for a service or management agent.
o This can currently be set via the vSphere Web Client, vSphere client or command line.
# Select a host from the inventory view and select configuration and then Security Profile.
o Enable Lockdown Mode
# Enable Lockdown Mode to require that all configuration changes go through the vCenter server. You can also enable or disable lockdown mode through the direct console user interface.
@ Select a host from the inventory view and select configuration and then Security Profile. Click edit next to lockdown mode and select Enable.
# Configure network security policies
@ Networking security policies determine how the adapter filters inbound and outbound frames.
@ 3 types are as follows
- Promiscuous Mode
= Reject — Placing a guest adapter in promiscuous mode has no effect on which frames are received by the adapter.
= Accept — Placing a guest adapter in promiscuous mode causes it to detect all frames passed on the vSphere standard switch that are allowed under the VLAN policy for the port group that the adapter is connected to.
- Mac Address Changes
= Reject — If you set the MAC Address Changes to Reject and the guest operating system changes the MAC address of the adapter to anything other than what is in the .vmx configuration file, all inbound frames are dropped.
= Accept — Changing the MAC address from the Guest OS has the intended effect: frames to the new MAC address are received.
- Forged Transmits
= Reject — Any outbound frame with a source MAC address that is different from the one currently set on the adapter are dropped.
= Accept — No filtering is performed and all outbound frames are passed.
- Add an ESXi Host to a directory service
= You can configure a host to use a directory service such as Active Directory to manage users and groups.
= When you add an ESXi host to Active Directory all user and group accounts are assigned full administrative access to the host if the group ESX Admins exists.
- Apply permissions to ESXi Hosts using Host Profiles
= When you join a host to an Active Directory domain, you must define roles on the host for a user or group in that domain. Otherwise, the host is not accessible to Active Directory users or groups. You can use host profiles to set a required role for a user or group and to apply the change to one or more hosts.
* In the vSphere client select Management and then Host Profiles you can either use an existing or new host profile.
+ Expand the profile tree and select Security Configuration, Permission Rules, Add Profile, Permission and then within configuration details use require a permission rule and add the appropriate settings. You can then select propagate permission and select ok.
* Configure virtual machine security policies
+ The guest operating system that runs in the virtual machine is subject to the same security risks as a physical system. Secure virtual machines as you would secure physical machines.
* Create/Manage vCenter Server Security Certificates
+ The vSphere Certificate Manager utility allows you to perform most certificate management tasks interactively from the command line. vSphere Certificate Manager prompts you for the task to perform, for certificate locations and other information as needed, and then stops and starts services and replaces certificates for you
o http://pubs.vmware.com/vsphere-60/topic/com.vmware.ICbase/PDF/vsphere-esxi-vcenter-server-60-security-guide.pdf Page 17



** Objective 1.3: Enable SSO and Active Directory Integration
------------------------------------------------------------
* Configure/Manage Active Directory Authentication
+ Active Directory authentication is configured from the Single Sign-On administration page, here you will add an additional identify source for Active Directory.
o If using Microsoft Active Directory, select Active Directory (Integrated Windows Authentication).It will autopopulate the root domain in the forest. If using Open LDAP, select and configure it.
+ Configure/Manage Platform Services Controller (PSC)
o The PSC includes common services used across VMware vCloud® Suite. This includes VMware vCenter Single Sign-On™, licensing, and certificate management.
# This is managed and configured by using the vSphere Web Client and selecting Administration and then System configuration and choosing the appropriate node.
o Configure/Manage VMware Certificate Authority (VMCA)
# VMCA is included in each Platform Services Controller and in each embedded deployment. VMCA provisions each node, each vCenter Server solution user, and each ESXi host with a certificate that is signed by VMCA as the certificate authority. vCenter Server solution users are groups of vCenter Server services. See vSphere Security for a list of solution users. You can replace the default certificates. For vCenter Server components, you can use a set of command-line tools included in your installation. You have several options.
@ You can either use the VMCA as a certificate authority or configure it to be an intermediate CA.
- http://pubs.vmware.com/vsphere-60/topic/com.vmware.ICbase/PDF/vsphere-esxi-vcenter-server-60-installation-setup-guide.pdf page 18
@ Enable/Disable Single Sign-On (SSO)
- You configure vCenter Single Sign-On from the vSphere Web Client. To configure vCenter Single Sign-On, you must have vCenter Single Sign-On administrator privileges. Having vCenter Single Sign-On administrator privileges is different from having the Administrator role on vCenter Server or ESXi. By default, only the user administrator@vsphere.local has administrator privileges on the vCenter Single SignOn server in a new installation Identify available authentication methods with VMware vCenter

[VCP6-DCV Study Guide - Section 1](http://davidstamen.com/wp-content/uploads/2015/04/VCP6-DCV-Study-Guide-Section-1.pdf)
