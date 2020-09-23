# Installing Hadoop on Ubuntu  20.04 LTS

Hadoop is written on Java, so for a compatible service a Java Environemt must be installed.

First, we must do the usual setting and state all Ubuntu modules updated.

$ sudo apt-get update

## Installing Java 8 in Ubuntu

Then, we must install Java 8 Environment.

$ sudo apt install openjdk-8-jdk -y

To identify which Java and Javac 

$ java -version; javac -version

## Setting up a user for the Hadoop framework

Install OpenSSH on Ubuntu

$ sudo apt install openssh-server openssh-client -y

Creating a Hadoop User, so ussing the command **adduser** to create a new user in Ubuntu, then set all features and options that Ubuntu request.

$ sudo adduser hadoop

Allows root permissions

$ sudo usermod -a -G sudo hadoop

Start on it.

$ su - hadoop

## Generating SSH key pair and define its location.

$ ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa

To store the key we must using the command **cat** command:

$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

Setting the permission by using the **chmod** command:

$ chmod 0600 ~/.ssh/authorized_keys

To verify all options were well establish correctly, start the SSH service.

$ ssh localhost

## Download and install Hadoop framework

Go to Hadoop Website and download the lastest version of [Hadoop][1]

You can use the **wget** command to provide a webpage call:

$ wget https://downloads.apache.org/hadoop/common/hadoop-3.2.1/hadoop-3.2.1.tar.gz

Extract the file.

$ tar xzf hadoop-3.2.1.tar.gz

[1]:https://downloads.apache.org/hadoop/common/hadoop-3.2.1/hadoop-3.2.1.tar.gz

## Hadoop framework deployment

The files to edit are:
* bashrc
* hadoop-env.sh
* core-site.xml
* hdfs-site.xml
* mapred-site-xml
* yarn-site.xml

### Set up the variables

$ sudo nano .bashrc

The Hadoop environment variables to add are:

* export HADOOP_HOME=/home/hadoop/hadoop-3.2.1
* export HADOOP_INSTALL=$HADOOP_HOME
* export HADOOP_MAPRED_HOME=$HADOOP_HOME
* export HADOOP_COMMON_HOME=$HADOOP_HOME
* export HADOOP_HDFS_HOME=$HADOOP_HOME
* export YARN_HOME=$HADOOP_HOME
* export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
* export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
* export HADOOP_OPTS=-Djava.library.path=$HADOOP_HOME/lib/native

Apply the changes

$ source ~/.bashrc

#### Configuration of the files

$ sudo nano $HADOOP_HOME/etc/hadoop/hadoop-env.sh

Add Java Environment to Hadoop Environment

* export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64

save the changes and exit.

$ sudo nano $HADOOP_HOME/etc/hadoop/core-site.xml

Add the following configuration for the Hadoop core properties

<property>
  <name>hadoop.tmp.dir</name>
  <value>/home/hdoop/tmpdata</value>
</property>
<property>
  <name>fs.default.name</name>
  <value>hdfs://127.0.0.1:9000</value>
</property>

Provide the configuration for the metadata storage

$ sudo nano $HADOOP_HOME/etc/hadoop/hdfs-site.xml

Adding the namenode and datanode:

<property>
  <name>dfs.data.dir</name>
  <value>/home/hdoop/dfsdata/namenode</value>
</property>
<property>
  <name>dfs.data.dir</name>
  <value>/home/hdoop/dfsdata/datanode</value>
</property>
<property>
  <name>dfs.replication</name>
  <value>1</value>
</property>

Defining Mapreduce values

$ sudo nano $HADOOP_HOME/etc/hadoop/mapred-site.xml

The configuration is:

<property> 
  <name>mapreduce.framework.name</name> 
  <value>yarn</value> 
</property>

Configure the node manager, resource manager, containers and application master

$ sudo nano $HADOOP_HOME/etc/hadoop/yarn-site.xml

<property>
  <name>yarn.nodemanager.aux-services</name>
  <value>mapreduce_shuffle</value>
</property>
<property>
  <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
  <value>org.apache.hadoop.mapred.ShuffleHandler</value>
</property>
<property>
  <name>yarn.resourcemanager.hostname</name>
  <value>127.0.0.1</value>
</property>
<property>
  <name>yarn.acl.enable</name>
  <value>0</value>
</property>
<property>
  <name>yarn.nodemanager.env-whitelist</name>   
  <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PERPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
</property>

## Establish the HDFS namenode

hdfs namenode -format

## Start the Hadoop Cluster

Go to the hadoop-3.2.1/sbin directory and start the namenode and datanode.

Namenode:

$ ./start-dfs.sh

Datanode:

./start-yarn.sh

Cheking all Java processes:

$ jps
