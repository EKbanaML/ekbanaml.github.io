---
title: "Setting Up Cassandra"
excerpt_separator: "A introduction to Data Modeling in Cassandra."
last_modified_at: 2021-09-15T16:20:02-05:00
categories:
  - Cassandra
tags:
  - nosql
  - cassandra
  - Installation
header:
  image: "https://images.pexels.com/photos/772803/pexels-photo-772803.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260"
  caption: "Photo credit: [**Pexel**](https://images.pexels.com/)"
author: sudip
---

Before we begin about installing the cassandra,let’s know what cassandra is and Also know the idea of the cassandra architecture.

#### What is Cassandra?
Cassandra is a NoSql distributed database(Decentralised) based on peer-peer architecture as it is masterless with high availability,
scalability and data distribution across multiple nodes.With High Availability service there is no single point of failure

#### Architecture of Cassandra
![architectureofcassandra]({{ site.url }}{{ site.baseurl }}/assets/images/cassandra/architecture.png)

The design of cassandra is based to handle enormous amount of data across multiple nodes without any single point of failure.Cassandra
has peer-peer distributed system across it’s nodes,and data is distributed among all the nodes in a cluster.

1) All the nodes in a cluster play the same role. Each node is independent and at the same time interconnected to other nodes.

2) Each node in a cluster can accept read and write requests, regardless of where the data is actually located in the cluster.

3) When a node goes down, read/write requests can be served from other nodes in the network.

In Cassandra, one or more of the nodes in a cluster act as replicas for a given piece of data. If it is detected that some of the nodes
responded with an out-of-date value, Cassandra will return the most recent value to the client. After returning the most recent value,
Cassandra performs a read repair in the background to update the stale values. 

#### Installation Methods

Let’s start,there are three common methods of installing the cassandra

1) **Docker Image**

2) **Package installation (RPM)**

3) **Tarball binary file**

For **Docker image installation**,you need Docker install in your system.If you got Docker in your system,pull the cassandra image from
dockerhub and start the cassandra with run command.

For **Package installation** in  Debian-based distributions(UBUNTU) you can install cassandra using APT.Note that to install cassandra using
APT requires root permission and a new Cassandra OS user is created along with its binaries and configuration files.

For **Tarball binary file installation**,download the tar file and extract it.The tarball file extracts all of its contents and its binaries,
configuration in the directory respectively.Here,your are going to install the cassandra using tarball binary file..

#### Installing the cassandra using binary tarball

1) Verify if JAVA is installed in your system.Otherwise install JAVA in your system.

2) Download the tarball file of cassandra from this site and extract it.

The directories after extracting the tarball file is shown:
```
bin/
conf/	
data/		
doc/
interface/
javadoc/
lib/
logs/		
pylib/
tools/
```
1) The **bin** directory consists of commands to run cassandra,nodetool,cqlsh and SSTable tools.

2) The directory **conf** consists of a cassandra.yaml file in which the configuration is available to start a single node cassandra cluster.
   For multi-node cluster,the configuration must be modified to required criteria. 

3) The **data** directory consists of information on commit logs,hints and SSTables.

4) The **logs** directory consist of system and debug logs.

#### Setting Up Cassandra in Cluster 

In our cluster,we are using 4 nodes and setting up cassandra in it.But you can setup up in any number of cluster in your machines.
The process of setting up cassandra is given below:

1) Firstly,Install the cassandra using tarball file in a node as mentioned in  above.

2) Secondly,in the conf directory there is cassandra.yaml file which holds the configuration file and loads during startup of cassandra.
   Add ip address of the nodes which you wanna connect in the seeds field found in cassandra.yaml file.

![seedsInfo]({{ site.url }}{{ site.baseurl }}/assets/images/cassandra/seeds.png)

3) Repeat above steps in all the nodes

4) At last start the cassandra in every node using the command:
   **bin/cassandra -f**   which is found in the bin directory of the extracted tarball file of cassandra.
