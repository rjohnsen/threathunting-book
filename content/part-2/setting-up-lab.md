---
title: "Setting Up Lab"
date: 2024-08-03T14:25:28+02:00
draft: false
weight: 1
---

## Introduction

Being a successful threat hunter means having access to the right tools. One such tool is a system for log querying, which is crucial for detecting and analyzing potential security threats. There are many systems out there that fit the bill, for example:

- Elastic
- QRadar
- Splunk
- OSSIM
- WAZUH
- Datadog

And there are surely many other alternatives too. In this chapter, we are going to set up a tool called [OpenSearch](https://opensearch.org/). OpenSearch is an open-source search and analytics suite derived from [Elasticsearch](https://www.elastic.co/) 7.10 and Kibana 7.10. It provides a powerful, community-driven platform for log querying and analysis, enabling threat hunters to efficiently search, visualize, and analyze large volumes of data.

OpenSearch will be install in [Alma Linux](https://almalinux.org/) (Redhat Linux clone) running on the [VirtualBox](https://www.virtualbox.org/) hypervisor. Before we start on the installation procedure, lets talk some about the various components of this stack. 

### What is OpenSearch?

![OpenSearch](/images/Opensearch_Logo.svg.png)

OpenSearch was created by Amazon Web Services (AWS) after Elasticsearch changed its license from Apache 2.0 to Server Side Public License (SSPL). OpenSearch retains the core functionalities of Elasticsearch and Kibana but remains fully open-source under the Apache 2.0 License. This ensures that it remains free to use, modify, and distribute, making it an attractive option for organizations seeking a robust, community-supported solution without the constraints of more restrictive licenses.

#### Key Features of OpenSearch

OpenSearch offers a range of features that make it ideal for threat hunting and log analysis:
- **Full-Text Search**: Allows for fast and comprehensive searching across large datasets.
- **Real-Time Data Ingestion**: Supports real-time data ingestion and indexing, crucial for up-to-the-minute threat detection.
- **Advanced Analytics**: Provides tools for complex data analysis, including aggregations, histograms, and geospatial data.
- **Visualization**: Integrated with OpenSearch Dashboards, it offers rich visualization capabilities to create intuitive charts, graphs, and maps.
- **Security**: Built-in security features such as fine-grained access control, encryption, and audit logging ensure that your data remains protected.
- **Scalability**: Designed to scale horizontally, OpenSearch can handle large volumes of data across distributed systems.

#### Community and Ecosystem

OpenSearch benefits from a vibrant and growing community of contributors and users. AWS, along with other organizations, actively supports the project, driving innovation and ensuring regular updates and enhancements. This community-driven approach ensures that OpenSearch evolves to meet the needs of its users, fostering an ecosystem of shared knowledge and best practices.

#### Integration and Use Cases

OpenSearch integrates seamlessly with a variety of data sources and tools, making it versatile and adaptable to different environments. Whether you're aggregating logs from cloud services, network devices, or application servers, OpenSearch can handle the data ingestion and provide valuable insights through its powerful querying and visualization capabilities.

In this chapter, we will guide you through the setup of OpenSearch, from installation to configuration and initial usage. By the end, you'll have a robust log querying tool at your disposal, tailored to enhance your threat-hunting capabilities.

### What is Alma Linux?

![OpenSearch](/images/alma-logo.svg)

AlmaLinux is a free, open-source Linux distribution designed to provide a stable and reliable operating system for enterprises, servers, and workstations. Created as a community-driven fork of CentOS, it ensures binary compatibility with Red Hat Enterprise Linux (RHEL), making it an ideal replacement for CentOS users seeking a consistent and secure environment. Backed by the AlmaLinux OS Foundation, this distribution is maintained by a vibrant community committed to delivering long-term support and regular updates, ensuring it meets the diverse needs of its users.

## Lab installation

The following installation routine is based on the following online tutorials.

* [How to Install Docker on Linux (AlmaLinux)](https://www.liquidweb.com/blog/install-docker-on-linux-almalinux/)
* [Installing OpenSearch using Docker](https://opensearch.org/docs/latest/install-and-configure/install-opensearch/docker/)

This installation routine doesn't cover installation of Virtualbox and Alma Linux since installation of these are straight forward (click-click-next).

### VirtualBox NAT Network settings

Prior to installation of Alma, we should make sure we have a decent NAT network at hand for our lab environment. In In Virtualbox "Network Manager" we set up a NAT Network dedicated for this lab, using the following settings: 

| Setting | Value | Comment |
| ------- | ----- | ------- |
| Network name | ThreatHuntingNetwork | |
| IPv4 Prefix | 10.0.4.0/24 | |
| Enable DHCP | On | Depending on taste. You could use static IP if you prefer. |

We will return finishing setting up the NAT Network settings later in this chapter.

### VirtualBox Machine Settings

For our Alma Linux guest we will use the following settings (consider these as bare minumum):

| Setting | Value | Comment | 
| ------- | ----- | ------- |
| Base memory (RAM) | 8192MB | More is better |
| CPU | 4 | | 
| Disk Size | 150 GB | More is better |
| Video Memory | 128 MB (max) | This ensures that Almas installer runs smoothly, and you are well prepared if you want GUI later on. | 
| Network Adapter 1 | Nat Network |
| Nat Network | ThreatHuntingNetwork |

### Notes on Alma Linux Installation

We will base our lab on AlmaLinux, a free, community-driven, enterprise-grade Linux distribution that is seamlessly compatible with Red Hat Enterprise Linux (RHEL). While we could have used other distributions such as Ubuntu or OpenSUSE, AlmaLinux offers a stable and robust environment that is ideal for our needs.

To get started, you will need the "Minimal ISO," which can be obtained from the official AlmaLinux website: [https://almalinux.org/get-almalinux/](https://almalinux.org/get-almalinux/).

#### Disk Encryption

For a threat hunting lab, enabling disk encryption is not mandatory since you won't be working with customer data. However, if you do need to work with customer data, it is best practice to enable disk encryption during installation. This can be done under the "Installation Destination" section in the installer.

#### Enable SSHD

For easier administration (through Putty for example), enable SSHD:

```bash
sudo systemctl enable sshd
sudo systemctl start sshd
```

## Installing OpenSearch

### Installing Docker environment

We are going to run OpenSearch in Docker. The following guide is based upon this page: https://www.liquidweb.com/blog/install-docker-on-linux-almalinux/

Ensure Alma Linux is fully updated before proceeding: 

```bash
sudo dnf --refresh update
sudo dnf upgrade
```

Enable Docker Resitory:

```bash
sudo dnf install yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

Installing Docker

```bash
sudo dnf install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

Start and enable Docker:

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

### OpenSearch setup

Installation follows this tutorial closely: https://opensearch.org/docs/latest/install-and-configure/install-opensearch/docker/

For a Linux environment, run the following commands:

Disable memory paging and swapping performance on the host to improve performance.

```bash
sudo swapoff -a
```

Increase the number of memory maps available to OpenSearch.

```bash
# Edit the sysctl config file
sudo vi /etc/sysctl.conf

# Add a line to define the desired value
# or change the value if the key exists,
# and then save your changes.
vm.max_map_count=262144

# Reload the kernel parameters using sysctl
sudo sysctl -p

# Verify that the change was applied by checking the value
cat /proc/sys/vm/max_map_count
```

Before proceeding to the next step, we must set a password for the OpenSearch admin user (replace ```<custom-admin-password>``` with something sane):

```bash
export OPENSEARCH_INITIAL_ADMIN_PASSWORD=<custom-admin-password>
```

Copy the following to "docker-compose.yml" in a suitable location:

```
version: '3'
services:
  opensearch-node1: # This is also the hostname of the container within the Docker network (i.e. https://opensearch-node1/)
    image: opensearchproject/opensearch:latest # Specifying the latest available image - modify if you want a specific version
    container_name: opensearch-node1
    environment:
      - cluster.name=opensearch-cluster # Name the cluster
      - node.name=opensearch-node1 # Name the node that will run in this container
      - discovery.seed_hosts=opensearch-node1,opensearch-node2 # Nodes to look for when discovering the cluster
      - cluster.initial_cluster_manager_nodes=opensearch-node1,opensearch-node2 # Nodes eligible to serve as cluster manager
      - bootstrap.memory_lock=true # Disable JVM heap memory swapping
      - "OPENSEARCH_JAVA_OPTS=-Xms512m -Xmx512m" # Set min and max JVM heap sizes to at least 50% of system RAM
      - OPENSEARCH_INITIAL_ADMIN_PASSWORD=${OPENSEARCH_INITIAL_ADMIN_PASSWORD}    # Sets the demo admin user password when using demo configuration, required for OpenSearch 2.12 and later
    ulimits:
      memlock:
        soft: -1 # Set memlock to unlimited (no soft or hard limit)
        hard: -1
      nofile:
        soft: 65536 # Maximum number of open files for the opensearch user - set to at least 65536
        hard: 65536
    volumes:
      - opensearch-data1:/usr/share/opensearch/data # Creates volume called opensearch-data1 and mounts it to the container
    ports:
      - 9200:9200 # REST API
      - 9600:9600 # Performance Analyzer
    networks:
      - opensearch-net # All of the containers will join the same Docker bridge network
  opensearch-node2:
    image: opensearchproject/opensearch:latest # This should be the same image used for opensearch-node1 to avoid issues
    container_name: opensearch-node2
    environment:
      - cluster.name=opensearch-cluster
      - node.name=opensearch-node2
      - discovery.seed_hosts=opensearch-node1,opensearch-node2
      - cluster.initial_cluster_manager_nodes=opensearch-node1,opensearch-node2
      - bootstrap.memory_lock=true
      - "OPENSEARCH_JAVA_OPTS=-Xms512m -Xmx512m"
      - OPENSEARCH_INITIAL_ADMIN_PASSWORD=${OPENSEARCH_INITIAL_ADMIN_PASSWORD}
    ulimits:
      memlock:
        soft: -1
        hard: -1
      nofile:
        soft: 65536
        hard: 65536
    volumes:
      - opensearch-data2:/usr/share/opensearch/data
    networks:
      - opensearch-net
  opensearch-dashboards:
    image: opensearchproject/opensearch-dashboards:latest # Make sure the version of opensearch-dashboards matches the version of opensearch installed on other nodes
    container_name: opensearch-dashboards
    ports:
      - 5601:5601 # Map host port 5601 to container port 5601
    expose:
      - "5601" # Expose port 5601 for web access to OpenSearch Dashboards
    environment:
      OPENSEARCH_HOSTS: '["https://opensearch-node1:9200","https://opensearch-node2:9200"]' # Define the OpenSearch nodes that OpenSearch Dashboards will query
    networks:
      - opensearch-net

volumes:
  opensearch-data1:
  opensearch-data2:

networks:
  opensearch-net:
```

Then run this command to start the service containers:

```bash
sudo docker compose up -d
```

Verify that the service containers started correctly:


```bash
sudo docker compose ps
```

### NAT Network forwarding rules

The last step is to set up port forwading between our main OS and Guest (Alma). We do so using Virtualbox "Network Manager" and these rules:

| Name | Protocol | Host IP | Host Port | Guest IP | Guest Port |
| ---- | -------- | ------- | --------- | -------- | ---------- |
| SSH | TCP | 127.0.0.1 | 22 | GUEST IP | 22 |
| OpenSearch Service | TCP | 127.0.0.1 | 443 | GUEST IP | 443 |
| OpenSearch Dashboards | TCP | 127.0.0.1 | 5601 | GUEST IP | 5601 |
| OpenSearch REST API | TCP | 127.0.0.1 | 9200 | GUEST IP | 9200 |
| OpenSearch Node communication and transport (internal), cross cluster search | TCP | 127.0.0.1 | 9300 | GUEST IP | 9300 |
| OpenSearch Performance Analyzer | TCP | 127.0.0.1 | 9600 | GUEST IP | 9600 |

More details on oorts for OpenSearch can be found [here][https://opensearch.org/docs/latest/install-and-configure/install-opensearch/index/]. Wait a couple of minutes to let services start, then you are ready to take this lab for a spin.  By using these NAT rules you can now visit OpenSearch in your browser:

```
http://127.0.0.1:5601/app/home#/
```

If things goes awry, please consult either the Docker, Alma or OpenSearch technial documentation and/or forums.
