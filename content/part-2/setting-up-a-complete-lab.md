---
title: "Setting up a complete lab"
date: 2024-09-13T13:05:47+02:00
weight: 2
draft: false
---

{{% notice info %}}
This public preview offers a glimpse of upcoming content. Please note that the content may be subject to change without prior notice as I continue to develop this site.
{{% /notice %}}

In the chapter "[Setting Up a Basic Lab](https://huntbook.predefender.com/part-2/setting-up-a-basic-lab/)," we covered how to install a simple Threat Hunting lab using OpenSearch on Alma Linux in VirtualBox. Now, in this chapter, we'll focus on building a more comprehensive lab environment with additional tools. Our main goal here is to automate the installation process so the environment can be easily set up and torn down as needed.

To make things easier for you, I’ve turned my personal threat hunting workstation into a series of installation files you can use. Here’s an overview of the setup:

* **Host OS**: Windows 11 Pro, 1TB SSD, 32GB RAM, Intel Core i7 (Lenovo T14). This to gives you a rough idea on my workstation specs.
* **Virtualization**: VirtualBox
* **Guest main OS**: Alma Linux
* **Docker**:
    * OpenSearch (for analyzing incoming logs)
    * XWiki (for documentation)
    * CyberChef (a versatile toolset)
    * Portainer (for managing Docker containers)

I’ve created an installation routine to guide you through setting up the environment, starting from a fresh install of Alma Linux (right after the first login). But before diving in, let’s talk a bit more about this project named "Threat Hunting Workstation".

## About the Threat Hunting Workstation

The [__Threat Hunting Workstation__](https://github.com/rjohnsen/threathunting-workstation) is meant to be a complete toolkit designed to help teams set up a dedicated environment for efficient threat hunting - as well as an environment you can teardown and setup time whenever required. It includes a SIEM tool for log importing and analysis, a wiki for documentation, and a collection of essential utilities for threat hunting and incident management. Delivered as an installation script for a virtualized Alma Linux instance, this workstation provides a centralized, powerful solution to improve team collaboration and streamline security operations.

This installation routine is based on Ansible - please have a look at the [**setup-ansible.yml**](https://github.com/rjohnsen/threathunting-workstation/blob/main/setup-ansible.yml) YAML file to see each step involved in setting up this workstation. 

## Architecture

The workstation architecture looks like this:

{{<mermaid align="center">}}
flowchart LR
 B[Virtualbox]
 C[Alma Linux]
 D[Docker]
 E[OpenSearch]
 F[XWiki]
 G[Portainer]
 H[CyberChef]
 I[DFIR-IRIS]

 subgraph A[Threathunter Workstation]
    subgraph B[Virtualbox]
        C-->D
        D-->E
        D-->F
        D-->G
        D-->H
        D-. Future .->I
    end
 end
 {{< /mermaid >}}

## Installation

The great thing about this installation is that you need only to worry about one single script. That's right, no need to Git clone a repo or similar. In general, the installation consists of two steps:

1. Install Alma Linux in Virtualbox. Not covered in this installation instruction. 
2. Install the Threathunting Workstation toolset using the script covered in this installation instruction.


Obtain the installation script:

```bash
curl https://raw.githubusercontent.com/rjohnsen/threathunting-workstation/main/install-workstation.sh -o install-workstation.sh
```

Run setup script:

```bash
chmod +x install-workstation.sh
./install-workstation.sh
```

Then follow directions.

{{% notice info %}}
The installation script may take a long time to run. This is due to two factors: the speed of your Internet line and the general speed of Docker Container Image repositories. During testing I have noticed that downloading Docker containers may take a very long time, sadly. 
{{% /notice %}} 

## Service Ports

The following table displays the communication ports for each intalled service. This table also serves as a reference for setting up port forwarding for NAT Networks in Virtualbox:

| Service | Protocol | Port |
| ------- | -------- | ---- |
| OpenSearch Dashboards (main SIEM interface) |  TCP | 5601 |
| OpenSearch Logstasth | TCP | 5044 |
| OpenSearch Node Communication and Transport | TCP | 9300 |
| OpenSearch Perfomance Analyzer | TCP | 9600 |
| OpenSearch REST API | TCP | 9200 |
| Portainer | TCP | 9443 |
| SSH | TCP | TCP | 22 |
| XWiki | TCP | 8080 |
| Cyberchef | TCP | 8000 |  

## XWiki initial setup

First time reaching XWiki over the web it will kickstart its installer. The installer will guide you through four steps:

1. **Admin user** - make sure to create a user with administrative right
2. **Flavor** - install or update the flavor of this wiki. Make sure to install and use "XWiki Standard Flavor" as presented in the GUI.
3. **Orphaned dependencies** - make sure orphaned extension dependencies are either removed or made top level.
4. **Extensions** - update the installed extensions

## Alma Linux Default Credentials

The Ansible script will create a user belonging to the "wheel" group. This user can run "sudo". Its default credentials is "hunter:hunter"
