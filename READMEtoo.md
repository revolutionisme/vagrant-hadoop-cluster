-- vagrant manual CDH cluster via Cloudera Manager

Clone https://github.com/DandyDev/virtual-hadoop-cluster.git

Customize the vagrant file for memory or number of nodes required.

vagrant up

go to the following url -  http://vm-cluster-node1:7180/

use username and password admin - admin

select cloudera express 


follows steps

Optional(to save time if repeated setup is done)
  Download CDH parcel, parcel.sha1 and manifest.json for ubuntu precise from here -> https://archive.cloudera.com/cdh5/parcels/5/
  copy the files to the master node in /opt/cloudera/parcel-repo/
  start a webservice using
   - python -m SimpleHTTPServer 8900
  access the files using http://master:8900/ add it to the parcel repo
  [These steps would reduce the processing time of CDH parcel significantly]

--Important - Don't use single-user mode if not required(creates problem)

Optional(if psycopg2 is not found or cant be installed)
  ssh to master node
  download mysql-server and libmysql-java(jdbc connector)
,  sudo apt-get install mysql-server libmysql-java
  Once downloaded, login to mysql and required databases shown on the cloudera manager
  set hostname and port to localhost:3306 and username and password as to what you set after installation
  Test connection... It should work
 

After everything, the user permissions could create problems in accessing or running any applications. To solve that, follow the next step

In cloudera manager, go to hdfs configuration under advanced and put the following code in HDFS Service Configuration Safety Valve:
<property>
<name>dfs.permissions</name>
<value>false</value>
</property>
