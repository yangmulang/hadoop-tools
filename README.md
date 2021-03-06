## Creating Azure Virtual Machines for a CDH Cluster
This project contains a bunch of shell scripts and tools for working on Azure with Cloudera CDH. The Azure directory contains the Azure VM creation and attach storage script

To begin with install Windows Azure cross platform cli tools http://www.windowsazure.com/en-us/documentation/articles/xplat-cli/

Windows Azure Command-Line Tools for Mac and Linux Both a windows and Mac installer is available. You can call you’re azure subscription from the shell. Once installed you need to associate the tool with the relevant subscription. 

Go to https://manage.windowsazure.com and login to your subscription

With the tools installed import the account settings:

```
azure account download
```

This will start a browser and download .publishingsettings file

```
azure account set <azuresubsctiptionname>
```

It should be possible to create the Virtual Network associated with the subscription via the command line, however this proved to challenging. We therefore choose to do this directly from the Management Console.

Create an Affinity Group from the management console, this acts as an alias to represent the datacentre where you create the cluster. In the managment port selct "Add and Affinity Group" from the settings on the left hand menu.

Then create the virtual network. You can do this from the portal and export to an xml file to modify it. The virtual network the name and affinity group you chose need to added as variables to the CreateAzureVMs.sh set the virtual network address space e.g. 10.0.0.0/20 Within this address space, we can create additional subnets, this were named Subnet-1. 

The next stage is to set the address of the DNS Server. The first node created in the subnet is usually 10.0.0.4, each node is subsequently allocated sequentially by Azure.

The full network configuration can be imported from xml once created using the Portal. Go to New Networks, Virtual Network, and select Import Configuration.

Create a storage account for the new Affinity Group, where virtual machine disks are stored. The storage is akin Amazon Elastic Block Storage. The small Azure images contain insufficient storage to maintain the hdfs storage and therefore you must create additional disks.

```
azure account storage create --affinity-group hadoop-AG hadoopstorage
```

The VM Creation scripts are in the azure directory. At this stage it's worth cloning into the repo

```
git clone https://github.com/ndunnage/hadoop-tools.git
cd hadoop-tools
```

You can test using the sample benchmark script. An example sqoop import is included setup to work with an Azure Database


