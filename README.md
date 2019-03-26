# Hadoop-Cluster
MapReduce in Cluster.

# Prerequisites
Update your system
apt-get update

Install Java:
apt-get install default-jdk -y

See Java Version:
java -version
![image](https://user-images.githubusercontent.com/43897597/54974618-ffe77f80-4f6a-11e9-993d-c201529450c6.png)

download the hadoop package:
wget http://apache.mirrors.lucidnetworks.net/hadoop/common/stable/hadoop-3.2.0.tar.gz
Extract the hadoop package:
tar -xvzf hadoop-3.2.0.tar.gz
move the extracted files into /usr/local
mv hadoop-3.2.0 /usr/local/hadoop
open the hadoop-env.sh file and add the static value and dynamic value
nano /usr/local/hadoop/etc/hadoop/hadoop-env.sh
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre/
export JAVA_HOME=$(readlink -f /usr/bin/java | sed " s:bin/java::" )
run the hadoop:
/usr/local/hadoop/bin/hadoop
Copy the Hadoop' s configuration files into the newly created directory to use those files as our data.
$ mkdir wordcount_classes
javac -classpath ${usr/local/hadoop/bin/hadoop classpath} -d wordcount_classes/ '/home/paras/Downloads/WordCount.java'
you can find out the classpath by issuing:
echo $(usr/local/hadoop/bin/hadoop classpath)

![image](https://user-images.githubusercontent.com/43897597/54974334-18a36580-4f6a-11e9-9f4a-9b90f952a937.png)

jar -cvf wc.jar -C wordcount_classes/ .
/usr/local/hadoop/bin/hadoop jar wc.jar WordCount /usr/input /output
![image](https://user-images.githubusercontent.com/43897597/54974497-a2ebc980-4f6a-11e9-8b97-0558a59de7c7.png)


# Hadoop in Pseudo-Distributed Mode
Edit core-site.xml:
sudo gedit /usr/local/hadoop/etc/hadoop/core-site.xml

Insert into configuration tags:

<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>

![image](https://user-images.githubusercontent.com/43897597/54973713-5e126380-4f67-11e9-83f6-64ad6b79d071.png)

Edit hdfs-site.xml:
sudo gedit /usr/local/hadoop/etc/hadoop/hdfs-site.xml

Insert into configuration tags:

<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>

![image](https://user-images.githubusercontent.com/43897597/54973872-31128080-4f68-11e9-9f6d-3e1a8782b5ec.png)
Create public private key pairs 

  $ ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
  $ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
  $ chmod 0600 ~/.ssh/authorized_keys

Now ssh localhost

--Format the filesystem:
$ bin/hdfs namenode -format
![image](https://user-images.githubusercontent.com/43897597/54974248-c6624480-4f69-11e9-8ca3-e0e9dadb7840.png)

Start the namenodes. secondary namenodes and datanodes
![image](https://user-images.githubusercontent.com/43897597/54974014-d594c280-4f68-11e9-8c83-fdadf51783a8.png)

Create the Directory in HDFS and insert the input file in it
$ /usr/local/hadoop/bin/hdfs dfs -mkdir /user 
$ /usr/local/hadoop/bin/hdfs dfs -mkdir /user/paras
$ /usr/local/hadoop/bin/hdfs dfs -put '/home/paras/Downloads/WordCountText.txt' /user/paras
![image](https://user-images.githubusercontent.com/43897597/54974975-1f32dc80-4f6c-11e9-91e1-21f7d9765a33.png)


![image](https://user-images.githubusercontent.com/43897597/54974072-0f65c900-4f69-11e9-9afb-07dd31834491.png)

Run Hadoop to execute the jar file:
$ /usr/local/hadoop/bin/hadoop jar wc.jar WordCount /user/paras /output
![image](https://user-images.githubusercontent.com/43897597/54975395-5229a000-4f6d-11e9-8c59-69b3daa4c7c9.png)

You can see the contents of output directory:

![image](https://user-images.githubusercontent.com/43897597/54975481-974dd200-4f6d-11e9-8747-e9f52bb43b7f.png)
