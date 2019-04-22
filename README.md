# Hadoop-Cluster
MapReduce in Cluster.

# Prerequisites
You need at least two Linux based hosts to set up a hadoop cluster. One would act as a Master and other hosts act as slaves. You can add any number of slaves to your cluster. Here, I have used two hosts for the sake of simplicity and demonstration where one act as master and other as slave.

# Steps-by-Step Process
You might need to update your system

`apt-get update`

Hadoop wokrs on java. If you don't have java already installed, install java by using :

`apt-get install default-jdk -y`

See Java Version:

`java -version`

![image](https://user-images.githubusercontent.com/43897597/54974618-ffe77f80-4f6a-11e9-993d-c201529450c6.png)

There are many versions of Hadoop available online. Select one of the stable release and download using the `wget` command.  

http://www.apache.org/dyn/closer.cgi/hadoop/common/

`wget http://apache.mirrors.lucidnetworks.net/hadoop/common/stable/hadoop-3.2.0.tar.gz`

Once the download is complete, extract the package:

`tar -xvzf hadoop-3.2.0.tar.gz`

Move the extracted files into local directory /usr/local

`mv hadoop-3.2.0 /usr/local/hadoop`

Open the hadoop-env.sh file using `nano` command and look for the `export JAVA_HOME=` line. Uncomment it  and add the static value and dynamic value for `JAVA_HOME`

`nano /usr/local/hadoop/etc/hadoop/hadoop-env.sh`

```
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre/
export JAVA_HOME=$(readlink -f /usr/bin/java | sed " s:bin/java::" )
```

Check if the Hadoop is correctly installed by running the below command:

`/usr/local/hadoop/bin/hadoop`

![image](https://user-images.githubusercontent.com/43897597/54976334-6ae78500-4f70-11e9-9489-0cfea4feb82d.png)

Ceate a directory for your input classes.

`mkdir wordcount_classes`

Execute the java file with the following command (If java file is not in present working directory, use full directory address where java file is located):

`javac -classpath ${usr/local/hadoop/bin/hadoop classpath} -d wordcount_classes/ '/home/paras/Downloads/WordCount.java'`

you can find out the classpath by issuing:
`$ echo $(usr/local/hadoop/bin/hadoop classpath)`

![image](https://user-images.githubusercontent.com/43897597/54974334-18a36580-4f6a-11e9-9f4a-9b90f952a937.png)

Consolidate the files in the wordcount_classes/ directory into a single jar file:

`jar -cvf wc.jar -C wordcount_classes/ . `

![image](https://user-images.githubusercontent.com/43897597/54974497-a2ebc980-4f6a-11e9-8b97-0558a59de7c7.png)

Run the jar file in hadoop:

`/usr/local/hadoop/bin/hadoop jar wc.jar WordCount /usr/input /output`


# Hadoop in Pseudo-Distributed Mode
Edit core-site.xml:

`sudo gedit /usr/local/hadoop/etc/hadoop/core-site.xml`

Insert into configuration tags:

```
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```

![image](https://user-images.githubusercontent.com/43897597/54973713-5e126380-4f67-11e9-83f6-64ad6b79d071.png)

Edit hdfs-site.xml:

`sudo gedit /usr/local/hadoop/etc/hadoop/hdfs-site.xml`

Insert into configuration tags:

```
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>
```

![image](https://user-images.githubusercontent.com/43897597/54973872-31128080-4f68-11e9-9f6d-3e1a8782b5ec.png)

Create public private key pairs 

`$ ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa`

`$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys`

`$ chmod 0600 ~/.ssh/authorized_keys`

Now `ssh localhost`

**Format the filesystem**:

`/usr/local/hadoop/bin/hdfs namenode -format`

![image](https://user-images.githubusercontent.com/43897597/54974248-c6624480-4f69-11e9-8ca3-e0e9dadb7840.png)

Start the namenodes, secondary namenodes and datanodes

`/usr/local/hadoop/sbin/start-dfs.sh `

![image](https://user-images.githubusercontent.com/43897597/54974014-d594c280-4f68-11e9-8c83-fdadf51783a8.png)

Create the Directory in HDFS and insert the input file in it

```
$ /usr/local/hadoop/bin/hdfs dfs -mkdir /user 
$ /usr/local/hadoop/bin/hdfs dfs -mkdir /user/paras
$ /usr/local/hadoop/bin/hdfs dfs -put '/home/paras/Downloads/WordCountText.txt' /user/paras
```

![image](https://user-images.githubusercontent.com/43897597/54974975-1f32dc80-4f6c-11e9-91e1-21f7d9765a33.png)

![image](https://user-images.githubusercontent.com/43897597/54974072-0f65c900-4f69-11e9-9afb-07dd31834491.png)

Run Hadoop to execute the jar file:

`$ /usr/local/hadoop/bin/hadoop jar wc.jar WordCount /user/paras /output`

![image](https://user-images.githubusercontent.com/43897597/54975395-5229a000-4f6d-11e9-8c59-69b3daa4c7c9.png)

You can see the contents of output directory. Go to http://localhost:50070/ for Hadoop 2.x versions or older and http://localhost:9870/ for Hadoop 3.x versions:

![image](https://user-images.githubusercontent.com/43897597/54975481-974dd200-4f6d-11e9-8747-e9f52bb43b7f.png)

Opening the file part-r-00000:
 
![image](https://user-images.githubusercontent.com/43897597/54975584-e7c52f80-4f6d-11e9-8e84-04e6fa9fa075.png)

# Run WordCount  on the Hadoop cluster with 2 VMs
1) Configure two VMs 
2) Rename the master hostname as HadoopMaster
Rename the slave hostname as HadoopSlave

3)install Java on both :
4)install hadoop on both:
Make changes in configuration of below mentioned files:

**core-site.xml**:

`sudo gedit /usr/local/hadoop/etc/hadoop/core-site.xml `
```<property>
  <name>fs.default.name</name>
  <value>hdfs://HadoopMaster:9000</value>
</property>
```
![image](https://user-images.githubusercontent.com/43897597/54981561-f1a35e80-4f7e-11e9-9fa3-3dce0e7044d5.png)

**hdfc-site.xml**:

**_HadoopMaster_**:

`sudo gedit /usr/local/hadoop/etc/hadoop/hdfs-site.xml `
```
<configuration>
<property>
	<name>dfs.replication</name>
	<value>1</value>
</property>
<property>
<name>dfs.namenode.name.dir</name>
<value>file:/usr/local/hadoop_tmp/hdfs/namenode</value>
</property>
</configuration>
```
![image](https://user-images.githubusercontent.com/43897597/54981926-edc40c00-4f7f-11e9-9eb6-49df1df17aa6.png)

**_HadoopSlave_**:

`sudo gedit /usr/local/hadoop/etc/hadoop/hdfs-site.xml `
```
<configuration>
<property>
      <name>dfs.replication</name>
      <value>1</value>
 </property>

 <property>
      <name>dfs.datanode.data.dir</name>
      <value>file:/usr/local/hadoop_tmp/hdfs/datanode</value>
 </property>
</configuration>
```

![image](https://user-images.githubusercontent.com/43897597/54982150-76db4300-4f80-11e9-8adb-5530a6bc4983.png)

**yarn-site.xml**

**--HadoopMaster and HadoopSlave--**:

`sudo gedit /usr/local/hadoop/etc/hadoop/yarn-site.xml `
```
<property>
	<name>yarn.resourcemanager.resource-tracker.address</name>
	<value>HadoopMaster:8025</value>
</property>
<property>
	<name>yarn.resourcemanager.scheduler.address</name>
	<value>HadoopMaster:8035</value>
</property>
<property>
	<name>yarn.resourcemanager.address</name>
	<value>HadoopMaster:8050</value>
</property>
```
![image](https://user-images.githubusercontent.com/43897597/54982042-34b20180-4f80-11e9-9c53-e49e9d5b297d.png)

**mapred-site.xml**
`sudo gedit /usr/local/hadoop/etc/hadoop/mapred-site.xml`
```
<configuration>
<property>
	<name>mapreduce.job.tracker</name>
	<value>HadoopMaster:5431</value>
</property>
<property>
	<name>mapred.framework.name</name>
	<value>yarn</value>
</property>
</configuration>
```

**masters**
*_Only on master node i.e. HadoopMaster_**
`sudo gedit /usr/local/hadoop/etc/hadoop/masters`
```
HadoopMaster
```
**workers**
*_Only on master node i.e. HadoopMaster_**
`sudo gedit /usr/local/hadoop/etc/hadoop/workers`
```
HadoopSlave
```

**hosts**
`sudo gedit /etc/hosts`

**_HadoopMaster and HadoopSlave_**
```
127.0.0.1	localhost
<master node's IPv4 Address> HadoopMaster
<slave node's IPv4 Address> HadoopSlave
```

**_Note:_** If the Hadoop version is 2.X or older, then you might need to change **_slaves_** instead of **_workers_**

**~/.bashrc file**
Open the file using `sudo gedit ~/.bashrc` and add the below lines at the end of the file:

```
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre/
export HADOOP_HOME=/usr/local/hadoop
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin
```
![image](https://user-images.githubusercontent.com/43897597/55028397-f5b29900-4fdd-11e9-957d-99f25ee3cc0c.png)
 
If you have not configured your hadoop-env.sh file, edit the file as mentioned in above section (Single Node Standalone Mode)

![image](https://user-images.githubusercontent.com/43897597/55028598-82f5ed80-4fde-11e9-97ab-63c5dadcf29a.png)

**Setting Passwordless connection between master and Slave**
Refer to the below link to set up a passwordless ssh to localhost and the remote machine. This has to be done on master node as well as slave node.

https://www.tecmint.com/ssh-passwordless-login-using-ssh-keygen-in-5-easy-steps/

**Format the filesystem**:

`/usr/local/hadoop/bin/hdfs namenode -format`

![image](https://user-images.githubusercontent.com/43897597/54974248-c6624480-4f69-11e9-8ca3-e0e9dadb7840.png)

Start the namenodes, secondary namenodes and datanodes

`/usr/local/hadoop/sbin/start-dfs.sh `

![image](https://user-images.githubusercontent.com/43897597/54974014-d594c280-4f68-11e9-8c83-fdadf51783a8.png)

Create the Directory in HDFS and insert the input file in it

```
$ /usr/local/hadoop/bin/hdfs dfs -mkdir /user 
$ /usr/local/hadoop/bin/hdfs dfs -mkdir /user/paras
$ /usr/local/hadoop/bin/hdfs dfs -put '/home/paras/Downloads/WordCountText.txt' /user/paras
```

![image](https://user-images.githubusercontent.com/43897597/54974975-1f32dc80-4f6c-11e9-91e1-21f7d9765a33.png)

![image](https://user-images.githubusercontent.com/43897597/54974072-0f65c900-4f69-11e9-9afb-07dd31834491.png)

Run Hadoop to execute the jar file:

`$ /usr/local/hadoop/bin/hadoop jar wc.jar WordCount /user/paras /output`

![image](https://user-images.githubusercontent.com/43897597/54975395-5229a000-4f6d-11e9-8c59-69b3daa4c7c9.png)

# References
http://pingax.com/install-apache-hadoop-ubuntu-cluster-setup/

http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/SingleCluster.html

https://www.linuxhelp.com/how-to-install-hadoop-in-ubuntu

https://www.tecmint.com/ssh-passwordless-login-using-ssh-keygen-in-5-easy-steps/
