# Hadoop-Cluster
MapReduce in Cluster.

# Prerequisites
Update your system
apt-get update
Install Java:
apt-get install default-jdk -y
See Java Version:
java -version
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
mkdir wordcount_classes
javac -classpath ${usr/local/hadoop/bin/hadoop classpath} -d wordcount_classes/ '/home/paras/Downloads/WordCount.java'
(you can find out the classpath by issuing hadoop-2.7.2/bin/hadoop classpath )
jar -cvf wc.jar -C wordcount_classes/ .
/usr/local/hadoop/bin/hadoop jar wc.jar WordCount /usr/input /output


# Hadoop in Pseudo-Distributed Mode
sudo gedit /usr/local/hadoop/etc/hadoop/core-site.xml

