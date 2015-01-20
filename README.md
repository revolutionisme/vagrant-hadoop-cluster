# A working virtual Hadoop cluster

With these files you can setup and provision a locally running, virtual Hadoop cluster in real distributed fashion for trying out Hadoop and related technologies. It runs the latest Cloudera Hadoop distribution: **CDH5**. It also allows you to practise the use of [Cloudera Manager](http://www.cloudera.com/content/cloudera/en/products-and-services/cloudera-enterprise/cloudera-manager.html) for installing the Hadoop stack. If you're looking for a fully automated install, without user intervention, look elsewhere. I specifically made this with the goal of creating an environment ideally suited for Cloudera Manager to do its job. This gives you the freedom to actually install the services you want, and change the configuration how you see fit.

This README describes how to get the cluster with Cloudera Manager up and running. For more detailed instructions on how to install the whole Hadoop stack on that, you can use [this guide](http://dandydev.net/blog/installing-virtual-hadoop-cluster).

## Specs

The cluster conists of 4 nodes:

* Master node with 4GB of RAM (Running the NameNode, Hue, ResourceManager etc. after installing the Hadoop services)
* 3 slaves with 2GB of RAM each (Running DataNodes)

As you can see, **you'll need at least 10GB of free RAM to run this**. If you have less, you can try to remove one machine from the Vagrantfile. This will lead to worse performance though!

## Usage

Depending on the hardware of your computer, installation will probably take between 15 and 25 minutes.

First install [VirtualBox](https://www.virtualbox.org/) and [Vagrant](http://www.vagrantup.com/).

Install the Vagrant [Hostmanager plugin](https://github.com/smdahlen/vagrant-hostmanager)

```bash
$ vagrant plugin install vagrant-hostmanager
```

Clone this repository.

```bash
$ git clone https://github.com/andre3k1/vagrant-hadoop-cluster.git
```

Provision the bare cluster. It will ask you to enter your password, so it can modify your `/etc/hosts` file for easy access in your browser. It uses the Vagrant Hostmanager plugin to do this.

```bash
$ cd virtual-hadoop-cluster
$ vagrant up
```

## Installing Hadoop and Related Components

Go to the [Cloudera Manager web console](http://vm-cluster-node1:7180) and follow the following instructions to install Hadoop on your virtual machine cluster.

1. Surf to: [http://vm-cluster-node1:7180](http://vm-cluster-node1:7180)
2. Login with `admin`/`admin`.
3. Select Cloudera Express, and click Continue twice.
4. On the page where you have to specify hosts, enter the following: `vm-cluster-node[1-4]` the click Search. The 4 nodes should pop up. Select them all and click Continue.
5. On the next page (Cluster Installation > Select Repository), leave everything as is and click Continue.
6. On the next page (Cluster Installation > Configure Java Encryption) I’d advise to tick the box, but only if your country allows it. Click Continue.
7. On the next page, [1] Login To All Hosts As => Another user => enter: `vagrant`, [2] In the two password fields enter: `vagrant`, and [3] Click Continue.
8. Wait for Cloudera Manager to install the prerequisite software, then click Continue.
9. Wait for Cloudera Manager to download and distribute the CDH packages, then click Continue.
10. Wait for the installer to finish inspecting the hosts. Run Again, if you encounter any errors. After this, click Finish.
11. For now, we’ll install everything but HBase. You can add HBase later, but it’s quite taxing for the virtual cluster. So on the Cluster Setup page, choose Custom Services and select the following: **HDFS, Hive, Hue, Impala, Oozie, Solr, Spark, Sqoop2, YARN and ZooKeeper**. Click Continue.
12. On the next page, you can select what services end up on what nodes. Cloudera Manager automatically chooses the best configuration, but you can change it if you want. Click Continue.
13. On the Database Setup page, make sure Use Embedded Database is activated. Click Test Connection (it says it will skip this step) then click Continue.
14. After clicking Continue on the Review Changes page, Cloudera Manager will attempt to configure and start all services.

**Done!** Have fun with your Hadoop cluster.
