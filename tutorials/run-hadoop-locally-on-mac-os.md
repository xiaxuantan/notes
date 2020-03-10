# Run Hadoop locally on Mac OS

I've installed Hadoop for countless times. Still, I forgot some very basic configurations. Here I try to write down some key steps so that this process can be reproduced. This includes very few actions to take and hopefully, everyone can get started by following these steps. And of course, it is just for running toy models locally. It can be much more complicated if you are setting up a cluster in production environment.

## Manage Java/Hadoop with Homebrew

As a Mac user, I try to manager all libraries with Homebrew. Similarly, managing Java and Hadoop with Homebrew can save you a lot of time.

**Install Hadoop**

```
$ brew install hadoop
```

As of March 2020, the latest Hadoop version is 3.2.1. Therefore, after downloading, I have Hadoop installed in directory `/usr/local/Cellar/hadoop/3.2.1_1`. A symlink is also created at `/usr/local/opt/hadoop/`.

**Install Java 8**

It might surprise you that Java 13 is already released but we still need Java 8. Hadoop is now trying to migrate to a higher version but Java 8 is best supported. With homebrew, it is convenient to keep multiple Java versions on your machine simultaneously. Refer to [this guidance](https://stackoverflow.com/questions/24342886/how-to-install-java-8-on-mac) for Java 8 installation.

For me, I have Java 8 and Java 12 at the same time, there is a command to find the Java home quickly.

```
$ /usr/libexec/java_home -V
Matching Java Virtual Machines (2):
    12.0.1, x86_64:	"OpenJDK 12.0.1"	/Library/Java/JavaVirtualMachines/openjdk-12.0.1.jdk/Contents/Home
    1.8.0_192, x86_64:	"Java SE 8"	/Library/Java/JavaVirtualMachines/jdk1.8.0_192.jdk/Contents/Home
```

## Configuration

Hadoop has a lot of configuration files. Inside each configuration file, there are a lot of properties. It is impossible to list everything here. I would just cite some paragraphs from [official document](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/SingleCluster.html), which allow hadoop to run in pseudo-distributed mode (a couple of services in a single machine).

The working directory for all following procedures is:

```
$ pwd
/usr/local/Cellar/hadoop/3.2.1_1/libexec
```

**Edit "etc/hadoop/core-site.xml"**

```xml
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```

**Edit "etc/hadoop/hdfs-site.xml"**

```xml
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>
```

**Edit "etc/hadoop/mapred-site.xml"**

```xml
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
    <property>
        <name>mapreduce.application.classpath</name>
        <value>$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/*:$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/lib/*</value>
    </property>
</configuration>
```

**Edit "etc/hadoop/yarn-site.xml" **

```xml
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
        <name>yarn.nodemanager.env-whitelist</name>
        <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
    </property>
</configuration>
```

All the content above is just merely copy/paste from documentation. It contains no customization on my machine. So I assume it would work exactly the same everywhere else.

**Change JAVA_HOME environment variable**

As I mention above, Hadoop requires Java 8. Thus, if your Java version if not Java 8 by default, please export it as an environment variable in file `etc/hadoop/hadoop-env.sh`

```shell
# The java implementation to use. By default, this environment
# variable is REQUIRED on ALL platforms except OS X!
# export JAVA_HOME=
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_192.jdk/Contents/Home
```

Your Java home might differ from me so do check your location with the command mentioned above.

## Start the services

**Setup passphraseless ssh**

It is not included in this article. If command `ssh localhost` fails to connect to your machine, please refer to other guidance that solves this problem.

**Start HDFS**

Format the filesystem is a crucial step that you must not skip:

```
$ bin/hdfs namenode -format
```

Start NameNode daemon and DataNode daemon:

```
$ sbin/start-dfs.sh
```

You should be able to access via web interface http://localhost:9870/

**Start yarn**

Start ResourceManager daemon and NodeManager daemon:

```
sbin/start-yarn.sh
```

You should be able to access via web interface http://localhost:8088/

**Troubleshooting**

You can use `jps` to see if all services are launched successfully. For example:

```
$ jps
94775 SecondaryNameNode
95065 ResourceManager
95160 NodeManager
95832 Jps
94539 NameNode
94639 DataNode
```

There should be five services besides jps itself: `NameNode`, `DataNode`, `SecondaryNameNode`, `ResourceManager` and `NodeManager`. Missing any of them might indicate you are doing something wrong.

If errors do occur, you can refer to logs. By default, it is generated at `logs` folder.

```
$ pwd
/usr/local/Cellar/hadoop/3.2.1_1/libexec

$ ls logs
hadoop-alice-datanode-alice.local.log
hadoop-alice-namenode-alice.local.log
hadoop-alice-secondarynamenode-alice.local.log
hadoop-alice-resourcemanager-alice.local.log
hadoop-alice-nodemanager-alice.local.log
...
```