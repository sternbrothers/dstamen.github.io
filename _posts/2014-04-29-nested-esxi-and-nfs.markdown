---
layout: post
title: Nested ESXi and NFS
date: 2014-04-29
categories:
- VMware
status: publish
type: post
published: true
---
I was working today on configuring NFS/Openfiler in my lab and came across an issue that my nested ESXi host's couldn't talk over the VSS (Standard Switch) I created. Upon further research when using nested ESXi you need to enable "Promiscuous Mode" on the VSS to allow the traffic to pass.
