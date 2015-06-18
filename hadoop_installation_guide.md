# Hadoop Installation Guide
This installation guide focus on installation on MacOS.

## Step 1: Download
* Download Hadoop From Apache website
* Download a stable release copy ending with tar.gz
* Extract file **hadoop.x.y.z.tar.gz** to the folder of your choice, for example, in ```$sudo tar -vxzf hadoop.2.6.0.tar.gz /Users/{yourUserName}/hadoop```

## Step 2: Install Java
* Check if Java 1.7 or above is already installed by typing command in the console ``` $java -version ```

* If not, download and install java 1.7 or above, like this ```$sudo tar -vxzf jdk1.8.0_20.tar.gz /Library/Java/JavaVirtualMachines/```



## Step 3: Edit Enviorment

* Modify .bash_profile in ```/Users/yourUserName``` by adding the following lines

```
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_20.jdk/Contents/Home
PATH=$PATH:$JAVA_HOME/bin

export HADOOP_INSTALL=/Users/{yourUserName}/hadoop/hadoop-2.6.0
PATH=$PATH:$HADOOP_INSTALL/bin
PATH=$PATH:$HADOOP_INSTALL/sbin

export JAVA_HOME
export PATH
```

* To test if java and hadoop are installed properly, run these commands and check if you get similar results.

```
$ java -version
java version "1.8.0_20"
Java(TM) SE Runtime Environment (build 1.8.0_20-b26)
Java HotSpot(TM) 64-Bit Server VM (build 25.20-b23, mixed mode)
``` 

```
$ hadoop version
Hadoop 2.6.0
Subversion https://git-wip-us.apache.org/repos/asf/hadoop.git -r e3496499ecb8d220fba99dc5ed4c99c8f9e33bb1
Compiled by jenkins on 2014-11-13T21:10Z
```

## Step 4: Stand alone mode is now installed
* There is no further actions to take in standalone mode.

## Step 5: Pseudo Distribution mode

### Intall ssh and rsync

* They are already installed in MacOS.

### Edit ```core-site.xml```
* Add the following lines in the ```/Users/{yourUserName}/hadoop/hadoop-2.6.0/etc/hadoop/core-stie.xml```

```
<configuration>
   <property>
      <name>fs.default.name</name>
      <value>hdfs://localhost:9000</value>
   </property>
</configuration>
```

### Edit hdfs-site.xml
* Add the following lines in the ```/Users/{yourUserName}/hadoop/hadoop-2.6.0/etc/hadoop/hdfs-stie.xml```

```
<configuration>
  <property>
    <name>dfs.replication</name>
    <value>1</value>
  </property>
</configuration>
```

### Edit mapred-site.xml
* Add the following lines in the ```/Users/{yourUserName}/hadoop/hadoop-2.6.0/etc/hadoop/mapred-stie.xml```

```
<configuration>
  <property>
     <name>mapreduce.framework.name</name>
     <value>yarn</value>
  </property>
</configuration>
```


* The above code is specifically for hadoop version 2.6.0 or above as it usses MapReduce 2, aka Yarn.

### Setup passwordless

1. Remote Host 
	>```$ ssh-keygen -t rsa -f ~/.ssh/id_rsa```

2. Creat host-specific key and sstore in standard location  
	> ```$ ssh-keygen -t rsa -f ~/.ssh/user_hostname ```

3. Copy key to remote host 
	> ```$ cat ~/.ssh/user_hostname.pub | ssh {yourUserName}@localhost 'cat >> ~/.ssh/authorized_keys'```

4. To confirm passwordless ssh has been setup, try the following and you should not be prompted for a password  
	> ```$ ssh localhost```

5. Format the name node 
	> ```$ ~/hadoop/hadoop-2.6.0/bin/hadoop namenode -format```

6. Start all the daemon 
	> ```$ /bin/start-all.sh```

7. To make sure hadoop is started properly, navigate to 
	>```http://localhost:50070/```  
	>```http://locahost:50030/ ``` 
	
	and make sure they both will forward to the relevant pages, namely **Hadooop Map/Reduce Adminstration Page** and **NameNode Page** respectively.

8. Alternatively, we  could run ```jps``` and we are expected to see these servers are started

	>```
	2310 SecondaryNameNode	1833 NameNode	2068 DataNode	2397 JobTracker	2635 TaskTracker	2723 Jps
	```

### Name node and Data node configuration

If NameNode or DataNode is not listed, then we need to explicitly set them up

1. Stop hadoop by running 
	>```stop-all.sh```
	
2. Create folder **dfs** in the hadoop directory by running 
	> ```$ mkdir ~/hadoop/hadoop-2.6.0/dfs``` 

3. Edit **hdfs-site.xml*** as follows
	> ```
	<configuration>
	  <property>
	    <name>dfs.data.dir</name>
	    <value>/Users/{yourUserName}/hadoop/dfs/data/</value>
	  </property>
	  <property>
	    <name>dfs.name.dir</name>
	    <value>/Users/{yourUserName}/hadoop/dfs/name/</value>
	  </property>
	  <property>
	    <name>dfs.replication</name>
	    <value>1</value>
	  </property>
	</configuration>


4. Run command 
>```$ hadoop namenode -format```


### Restart Hadoop

* Run ```$ start-all.sh```
* Run ```jps```

	> ```
	1590 SecondaryNameNode
	1496 DataNode
	1802 Jps
	1772 NodeManager
	1693 ResourceManager
	1423 NameNode
	```


