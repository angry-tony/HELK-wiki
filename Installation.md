# Requirements
* **OS Name:**
Specific Linux distros or OS X supported if Docker & Docker-Compose are not installed yet. If so, then the HELK images should work fine with any OS.
    * HELK uses the official Docker Community Edition (CE) bash script (Edge Version) to install Docker if it is not installed yet. The Docker CE Edge script supports the following distros: **ubuntu**, **debian**, **raspbian**, **centos**, and **fedora**.
    * You can see the specific distro versions supported in the script [here](https://get.docker.com/).
    * If you have Docker & Docker-Compose already installed in your system, then you should be good to go (this would skip the Docker CE Edge script execution). 
* **Network Connection:** NAT or Bridge
* **RAM:** 16GB (minimum)
* **Cores:** 4 (minimum)
* **Disk:** 50gb for testing purposes and 100gb+ for production (minimum)
* **Applications:**
  * Docker & Docker-Compose
  * Winlogbeat running on your endpoints. You can install Winlogbeat by following one of [@Cyb3rWard0g](https://twitter.com/Cyb3rWard0g) posts [here](https://cyberwardog.blogspot.com/2017/02/setting-up-pentesting-i-mean-threat_87.html). Make sure you use the [winlogbeat config](https://github.com/Cyb3rWard0g/HELK/blob/master/winlogbeat/winlogbeat.yml) recommended by the HELK since it uses the [Kafka output plugin](https://www.elastic.co/guide/en/beats/winlogbeat/current/kafka-output.html) and it is already pointing to the right ports with recommended options. You will just have to add your HELK's IP address. 
# HELK Download
Run the following commands to clone the HELK repo via git.
```
git clone https://github.com/Cyb3rWard0g/HELK.git
```
Change your current directory location to the new HELK directory, and run the **helk_install.sh** bash script as root.
```
cd HELK/
sudo ./helk_install.sh
```
# HELK Install
In order to make the installation of the HELK easy for everyone, the project comes with the helk_install.sh script that builds and runs everything you need to start the HELK automatically. You will just have to confirm that you want to use your HOST IP address for the HELK, unless you want to use a different one. Press \[Return\] or let the script continue on its own (30 Seconds sleep).

```
**********************************************
**          HELK - THE HUNTING ELK          **
**                                          **
** Author: Roberto Rodriguez (@Cyb3rWard0g) **
** HELK build version: 0.9 (Alpha)          **
** HELK ELK version: 6.2.4                  **
** License: BSD 3-Clause                    **
**********************************************
 
[HELK-INSTALLATION-INFO] Available Memory: 16
[HELK-INSTALLATION-INFO] Available Disk: 60
[HELK-INSTALLATION-INFO] Obtaining current host IP..
[HELK-INSTALLATION-INFO] Set HELK IP. Default value is your current IP: 192.168.64.131
[HELK-INSTALLATION-INFO] HELK IP set to 192.168.64.131
[HELK-INSTALLATION-INFO] HELK identified Linux as the system kernel
....
......
```
# Always monitor HELK installation Logs
Once the installation kicks in, it will start showing you pre-defined messages about the installation, but no all the details of what is actually happening in the background. It is designed that way to keep your main screen clean and let you know where it is in the installation process.
```
**********************************************
**          HELK - THE HUNTING ELK          **
**                                          **
** Author: Roberto Rodriguez (@Cyb3rWard0g) **
** HELK build version: 0.9 (Alpha)          **
** HELK ELK version: 6.2.4                  **
** License: BSD 3-Clause                    **
**********************************************
 
[HELK-INSTALLATION-INFO] Available Memory: 16
[HELK-INSTALLATION-INFO] Available Disk: 60
[HELK-INSTALLATION-INFO] Obtaining current host IP..
[HELK-INSTALLATION-INFO] Set HELK IP. Default value is your current IP: 192.168.64.131
[HELK-INSTALLATION-INFO] HELK IP set to 192.168.64.131
[HELK-INSTALLATION-INFO] HELK identified Linux as the system kernel
[HELK-INSTALLATION-INFO] Checking distribution list and version
[HELK-INSTALLATION-INFO] You're using ubuntu version xenial
[HELK-INSTALLATION-INFO] Docker is not installed
[HELK-INSTALLATION-INFO] Checking if curl is installed first
[HELK-INSTALLATION-INFO] curl is already installed
[HELK-INSTALLATION-INFO] Ready to install  Docker..
[HELK-INSTALLATION-INFO] Installing docker via convenience script..
[HELK-INSTALLATION-INFO] Installing docker-compose ..
[HELK-INSTALLATION-INFO] Checking local vm.max_map_count variable and setting it to 262144
[HELK-INSTALLATION-INFO] Installing HELK via docker-compose
...
....
```
What I recommend to do all the time is to open another shell and monitor the HELK installation logs by using the **tail** command and pointing it to the **/var/log/helk-install.log** file. This log file is available always on the host even if you are deploying the HELK via Docker.
```
helk@HELK:~$ tail -f /var/log/helk-install.log 
# Executing docker install script, commit: 5f7af95
+ sh -c apt-get update -qq >/dev/null
+ sh -c apt-get install -y -qq apt-transport-https ca-certificates curl >/dev/null
+ sh -c curl -fsSL "https://download.docker.com/linux/ubuntu/gpg" | apt-key add -qq - >/dev/null
+ sh -c echo "deb [arch=amd64] https://download.docker.com/linux/ubuntu xenial edge" > /etc/apt/sources.list.d/docker.list
+ [ ubuntu = debian ]
+ sh -c apt-get update -qq >/dev/null
+ sh -c apt-get install -y -qq --no-install-recommends docker-ce >/dev/null
+ sh -c docker version
Client:
 Version:	18.02.0-ce
 API version:	1.36
 Go version:	go1.9.3
 Git commit:	fc4de44
 Built:	Wed Feb  7 21:16:33 2018
 OS/Arch:	linux/amd64
 Experimental:	false
 Orchestrator:	swarm
..
...
 ---> Running in baae2db592a7
Removing intermediate container baae2db592a7
Creating helk-kibana ... done
Creating helk-zookeeper ... done
Creating helk-spark-master ... done
Creating network "helk_helk" with driver "bridge"
Creating volume "helk_esdata" with local driver
Creating helk-spark-worker2 ... done
Creating helk-kibana ... 
Creating helk-spark-master ... 
Creating helk-zookeeper ... 
Creating helk-logstash ... 
Creating helk-nginx ... 
Creating helk-kafka-broker ... 
Creating helk-spark-worker2 ... 
Creating helk-spark-worker ... 
Creating helk-jupyter ... 
```
Once you see that the containers have been created you can check all the containers running by running:

```
helk@HELK:~$ sudo docker ps
CONTAINER ID        IMAGE                     COMMAND                  CREATED             STATUS              PORTS                                            NAMES
c0263aeaa3c3        helk_helk-spark-worker2   "./spark-worker-entr…"   23 seconds ago      Up 21 seconds       0.0.0.0:8082->8081/tcp                           helk-spark-worker2
729edcf1ff97        helk_helk-jupyter         "./jupyter-entrypoin…"   23 seconds ago      Up 21 seconds       0.0.0.0:4040->4040/tcp, 0.0.0.0:8880->8880/tcp   helk-jupyter
70cac0b20d9f        helk_helk-spark-worker    "./spark-worker-entr…"   23 seconds ago      Up 21 seconds       0.0.0.0:8081->8081/tcp                           helk-spark-worker
2c618930d555        helk_helk-kafka-broker    "./kafka-entrypoint.…"   24 seconds ago      Up 21 seconds       0.0.0.0:9092->9092/tcp                           helk-kafka-broker
2e5f790064e1        helk_helk-nginx           "./nginx-entrypoint.…"   24 seconds ago      Up 22 seconds       0.0.0.0:80->80/tcp                               helk-nginx
7f6014946966        helk_helk-spark-master    "./spark-master-entr…"   26 seconds ago      Up 23 seconds       0.0.0.0:7077->7077/tcp, 0.0.0.0:8080->8080/tcp   helk-spark-master
7cf46440ec58        helk_helk-zookeeper       "./zookeeper-entrypo…"   26 seconds ago      Up 23 seconds       2888/tcp, 0.0.0.0:2181->2181/tcp, 3888/tcp       helk-zookeeper
e14f9dc2899f        helk_helk-logstash        "/usr/local/bin/dock…"   26 seconds ago      Up 23 seconds       0.0.0.0:5044->5044/tcp, 9600/tcp                 helk-logstash
4cb80263117c        helk_helk-kibana          "./kibana-entrypoint…"   26 seconds ago      Up 24 seconds       5601/tcp                                         helk-kibana
2e517a9fd8b8        helk_helk-elasticsearch   "/usr/local/bin/dock…"   27 seconds ago      Up 26 seconds       9200/tcp, 9300/tcp                               helk-elasticsearch
```

If you want to monitor the resources being utilized (Memory, CPU, etc), you can run the following:
```
helk@HELK:~$ sudo docker stats --all

CONTAINER ID        NAME                 CPU %               MEM USAGE / LIMIT     MEM %               NET I/O             BLOCK I/O           PIDS
c0263aeaa3c3        helk-spark-worker2   0.40%               106MiB / 7.594GiB     1.36%               3.18kB / 4.24kB     0B / 65.5kB         27
729edcf1ff97        helk-jupyter         0.15%               37.79MiB / 7.594GiB   0.49%               1.97kB / 844B       0B / 65.5kB         1
70cac0b20d9f        helk-spark-worker    0.39%               103.7MiB / 7.594GiB   1.33%               3.18kB / 4.22kB     0B / 65.5kB         27
2c618930d555        helk-kafka-broker    0.72%               306.8MiB / 7.594GiB   3.95%               10.5kB / 11.3kB     393kB / 8.64MB      80
2e5f790064e1        helk-nginx           0.04%               172KiB / 7.594GiB     0.00%               2.76kB / 2.38kB     131kB / 0B          2
7f6014946966        helk-spark-master    0.55%               131.8MiB / 7.594GiB   1.69%               9.42kB / 4.45kB     0B / 65.5kB         33
7cf46440ec58        helk-zookeeper       0.13%               50.39MiB / 7.594GiB   0.65%               11.7kB / 8.98kB     0B / 344kB          21
e14f9dc2899f        helk-logstash        0.75%               267.7MiB / 7.594GiB   3.44%               1.04kB / 0B         0B / 221kB          13
4cb80263117c        helk-kibana          0.03%               292KiB / 7.594GiB     0.00%               3.05kB / 2.6kB      2.02MB / 0B         2
2e517a9fd8b8        helk-elasticsearch   0.89%               2.227GiB / 7.594GiB   29.33%              6.48kB / 3.68kB     0B / 233MB          3
```

You should also monitor the logs of each container while they are being initialized:

Just run the following:

```
helk@HELK:~$ sudo docker logs --follow helk-elasticsearch
OpenJDK 64-Bit Server VM warning: If the number of processors is expected to increase from one, then you should configure the number of parallel GC threads appropriately using -XX:ParallelGCThreads=N
[2018-05-03T06:47:12,880][INFO ][o.e.n.Node               ] [helk-1] initializing ...
[2018-05-03T06:47:13,121][INFO ][o.e.e.NodeEnvironment    ] [helk-1] using [1] data paths, mounts [[/usr/share/elasticsearch/data (/dev/mapper/HELK--vg-root)]], net usable_space [7.5gb], net total_space [14.2gb], types [ext4]
[2018-05-03T06:47:13,123][INFO ][o.e.e.NodeEnvironment    ] [helk-1] heap size [3.9gb], compressed ordinary object pointers [true]
[2018-05-03T06:47:13,128][INFO ][o.e.n.Node               ] [helk-1] node name [helk-1], node ID [1bPlK3y1R2ug6EQK2pdsMQ]
[2018-05-03T06:47:13,129][INFO ][o.e.n.Node               ] [helk-1] version[6.2.4], pid[1], build[ccec39f/2018-04-12T20:37:28.497551Z], OS[Linux/4.4.0-87-generic/amd64], JVM[Oracle Corporation/OpenJDK 64-Bit Server VM/1.8.0_161/25.161-b14]
[2018-05-03T06:47:13,131][INFO ][o.e.n.Node               ] [helk-1] JVM arguments [-Xms1g, -Xmx1g, -XX:+UseConcMarkSweepGC, -XX:CMSInitiatingOccupancyFraction=75, -XX:+UseCMSInitiatingOccupancyOnly, -XX:+AlwaysPreTouch, -Xss1m, -Djava.awt.headless=true, -Dfile.encoding=UTF-8, -Djna.nosys=true, -XX:-OmitStackTraceInFastThrow, -Dio.netty.noUnsafe=true, -Dio.netty.noKeySetOptimization=true, -Dio.netty.recycler.maxCapacityPerThread=0, -Dlog4j.shutdownHookEnabled=false, -Dlog4j2.disable.jmx=true, -Djava.io.tmpdir=/tmp/elasticsearch.K1hNvi78, -XX:+HeapDumpOnOutOfMemoryError, -XX:+PrintGCDetails, -XX:+PrintGCDateStamps, -XX:+PrintTenuringDistribution, -XX:+PrintGCApplicationStoppedTime, -Xloggc:logs/gc.log, -XX:+UseGCLogFileRotation, -XX:NumberOfGCLogFiles=32, -XX:GCLogFileSize=64m, -Des.cgroups.hierarchy.override=/, -Xms4g, -Xmx4g, -Des.path.home=/usr/share/elasticsearch, -Des.path.conf=/usr/share/elasticsearch/config]
[2018-05-03T06:47:24,642][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [aggs-matrix-stats]
[2018-05-03T06:47:24,643][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [analysis-common]
[2018-05-03T06:47:24,643][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [ingest-common]
[2018-05-03T06:47:24,643][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [lang-expression]
[2018-05-03T06:47:24,643][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [lang-mustache]
[2018-05-03T06:47:24,643][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [lang-painless]
[2018-05-03T06:47:24,644][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [mapper-extras]
[2018-05-03T06:47:24,644][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [parent-join]
[2018-05-03T06:47:24,644][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [percolator]
[2018-05-03T06:47:24,644][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [rank-eval]
[2018-05-03T06:47:24,644][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [reindex]
[2018-05-03T06:47:24,644][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [repository-url]
[2018-05-03T06:47:24,644][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [transport-netty4]
[2018-05-03T06:47:24,644][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [tribe]
[2018-05-03T06:47:24,645][INFO ][o.e.p.PluginsService     ] [helk-1] loaded plugin [ingest-geoip]
[2018-05-03T06:47:24,648][INFO ][o.e.p.PluginsService     ] [helk-1] loaded plugin [ingest-user-agent]
[2018-05-03T06:47:24,648][INFO ][o.e.p.PluginsService     ] [helk-1] loaded plugin [x-pack-core]
[2018-05-03T06:47:24,648][INFO ][o.e.p.PluginsService     ] [helk-1] loaded plugin [x-pack-deprecation]
[2018-05-03T06:47:24,649][INFO ][o.e.p.PluginsService     ] [helk-1] loaded plugin [x-pack-graph]
[2018-05-03T06:47:24,649][INFO ][o.e.p.PluginsService     ] [helk-1] loaded plugin [x-pack-logstash]
[2018-05-03T06:47:24,649][INFO ][o.e.p.PluginsService     ] [helk-1] loaded plugin [x-pack-ml]
[2018-05-03T06:47:24,649][INFO ][o.e.p.PluginsService     ] [helk-1] loaded plugin [x-pack-monitoring]
[2018-05-03T06:47:24,650][INFO ][o.e.p.PluginsService     ] [helk-1] loaded plugin [x-pack-security]
[2018-05-03T06:47:24,650][INFO ][o.e.p.PluginsService     ] [helk-1] loaded plugin [x-pack-upgrade]
[2018-05-03T06:47:24,650][INFO ][o.e.p.PluginsService     ] [helk-1] loaded plugin [x-pack-watcher]
[2018-05-03T06:47:48,853][INFO ][o.e.x.m.j.p.l.CppLogMessageHandler] [controller/93] [Main.cc@128] controller (64 bit): Version 6.2.4 (Build 524e7fe231abc1) Copyright (c) 2018 Elasticsearch BV
[2018-05-03T06:47:51,173][INFO ][o.e.d.DiscoveryModule    ] [helk-1] using discovery type [single-node]
[2018-05-03T06:47:55,967][INFO ][o.e.n.Node               ] [helk-1] initialized
[2018-05-03T06:47:55,976][INFO ][o.e.n.Node               ] [helk-1] starting ...
[2018-05-03T06:47:56,867][INFO ][o.e.t.TransportService   ] [helk-1] publish_address {172.18.0.2:9300}, bound_addresses {0.0.0.0:9300}
[2018-05-03T06:47:57,092][INFO ][o.e.x.s.t.n.SecurityNetty4HttpServerTransport] [helk-1] publish_address {172.18.0.2:9200}, bound_addresses {0.0.0.0:9200}
[2018-05-03T06:47:57,096][INFO ][o.e.n.Node               ] [helk-1] started
```

All you need to do now for the other ones is define the containers name:
```
sudo docker logs --follow <container name>
```

Remember that you can also access your docker images by running the following commands:
```
sudo docker exec -ti helk-elasticsearch bash
root@7a9d6443a4bf:/opt/helk/scripts# 
```

# Final Details
Once your HELK installation ends, you will be presented with information that you will need to access the HELK and all its features. You will also have the Jupyter Notebook server running and ready to be used. The HELK uses Jupyter Lab. **You will still need to access the Jupyter Lab web interface and access/create a notebook in order to initialize the Spark Context (This initializes the default Python Kernel)**. You will have the following message showing in your main screen:
```
***********************************************************************************
** [HELK-INSTALLATION-INFO] YOUR HELK IS READY                                   **
** [HELK-INSTALLATION-INFO] USE THE FOLLOWING SETTINGS TO INTERACT WITH THE HELK **
***********************************************************************************
 
HELK KIBANA URL: http://192.168.64.131
HELK ELASTICSEARCH EXTERNAL URL: http://192.168.64.131:8082
HELK KIBANA & ELASTICSEARCH USER: helk
HELK KIBANA & ELASTICSEARCH PASSWORD: hunting
HELK JUPYTER CURRENT TOKEN: 3f46301da4cd20011391327647000e8006ee3574cab0b163
HELK JUPYTER LAB URL: http://192.168.64.131:8880/lab
HELK SPARK Pyspark UI: http://192.168.64.131:4040
HELK SPARK Cluster Master UI: http://192.168.64.131:8080
HELK SPARK Cluster Worker1 UI: http://192.168.64.131:8081
HELK SPARK Cluster Worker2 UI: http://192.168.64.131:8082

IT IS HUNTING SEASON!!!!!
```
Use the **JUPYTER CURRENT TOKEN** to access the Jupyter Notebook web interface.

# Starting Jupyter Kernel & Spark UI
One of the questions that I had recently was **why the Spark UI is not available after installation**. This is because the HELK uses PYSPARK  (Python API) to interact with Spark and it depends on its own Jupyter's Kernel being initialized. When the installation of the HELK is done, if you check your **helk-analytics docker Logs** (sudo docker logs helk-analytics) or the log **/var/log/analytics/analytics.log**, you will see the following message:
```
helk@HELK:~$ sudo docker logs helk-analytics
trap: SIGTERM: bad trap
[HELK-DOCKER-INSTALLATION-INFO] Starting analytic services..
Starting analytics
analytics started.
[HELK-DOCKER-INSTALLATION-INFO] Pushing analytic Logs to console..
[I 04:41:10.752 LabApp] 0 active kernels
[I 04:41:10.752 LabApp] The Jupyter Notebook is running at:
[I 04:41:10.752 LabApp] http://[all ip addresses on your system]:8880/?token=3f46301da4cd20011391327647000e8006ee3574cab0b163
[I 04:41:10.752 LabApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 04:41:10.753 LabApp] 
    
    Copy/paste this URL into your browser when you connect for the first time,
    to login with a token:
        http://localhost:8880/?token=3f46301da4cd20011391327647000e8006ee3574cab0b163
[I 04:41:10.893 LabApp] 302 GET / (172.18.0.1) 5.77ms
```
That tells you that the Jupyter Server is running, but it does not have an active kernel yet. Therefore, a **Spark executor driver** needs to be assigned in order for Spark to start. Also, you will see a **302** message in the spark logs, and that is normal because you still need to access the Jupyter Lab web interface using your **Jupyter token**.

Open your preferred browser, go to your Jupyter Interface, and enter your Jupyter token. That Jupyter URL is defined in your **HELK JUPYTER NOTEBOOK URI:** message.

[[https://github.com/Cyb3rWard0g/HELK/raw/master/resources/images/JUPYTER-Token.png]]


Open the **Check_Spark_Graphframes_Integrations.ipynb** notebook

[[https://github.com/Cyb3rWard0g/HELK/raw/master/resources/images/JUPYTER-Lab.png]]

If you check your Docker logs you will see the following:
```
[I 05:17:50.225 LabApp] Kernel started: dbd0f485-9659-4761-8ce9-6bc5361c9669
Ivy Default Cache set to: /root/.ivy2/cache
The jars for the packages stored in: /root/.ivy2/jars
:: loading settings :: url = jar:file:/opt/helk/spark/spark-2.2.1-bin-hadoop2.7/jars/ivy-2.4.0.jar!/org/apache/ivy/core/settings/ivysettings.xml
graphframes#graphframes added as a dependency
org.apache.spark#spark-sql-kafka-0-10_2.11 added as a dependency
databricks#spark-sklearn added as a dependency
:: resolving dependencies :: org.apache.spark#spark-submit-parent;1.0
	confs: [default]
	found graphframes#graphframes;0.5.0-spark2.1-s_2.11 in spark-packages
	found com.typesafe.scala-logging#scala-logging-api_2.11;2.1.2 in central
	found com.typesafe.scala-logging#scala-logging-slf4j_2.11;2.1.2 in central
	found org.scala-lang#scala-reflect;2.11.0 in central
	found org.slf4j#slf4j-api;1.7.7 in central
	found org.apache.spark#spark-sql-kafka-0-10_2.11;2.2.1 in central
	found org.apache.kafka#kafka-clients;0.10.0.1 in central
	found net.jpountz.lz4#lz4;1.3.0 in central
	found org.xerial.snappy#snappy-java;1.1.2.6 in central
	found org.slf4j#slf4j-api;1.7.16 in central
	found org.apache.spark#spark-tags_2.11;2.2.1 in central
	found org.spark-project.spark#unused;1.0.0 in central
	found databricks#spark-sklearn;0.2.3 in spark-packages
downloading http://dl.bintray.com/spark-packages/maven/graphframes/graphframes/0.5.0-spark2.1-s_2.11/graphframes-0.5.0-spark2.1-s_2.11.jar ...
	[SUCCESSFUL ] graphframes#graphframes;0.5.0-spark2.1-s_2.11!graphframes.jar (494ms)
downloading https://repo1.maven.org/maven2/org/apache/spark/spark-sql-kafka-0-10_2.11/2.2.1/spark-sql-kafka-0-10_2.11-2.2.1.jar ...
	[SUCCESSFUL ] org.apache.spark#spark-sql-kafka-0-10_2.11;2.2.1!spark-sql-kafka-0-10_2.11.jar (89ms)
downloading http://dl.bintray.com/spark-packages/maven/databricks/spark-sklearn/0.2.3/spark-sklearn-0.2.3.jar ...
	[SUCCESSFUL ] databricks#spark-sklearn;0.2.3!spark-sklearn.jar (176ms)
...
.....
	:: modules in use:
	com.typesafe.scala-logging#scala-logging-api_2.11;2.1.2 from central in [default]
	com.typesafe.scala-logging#scala-logging-slf4j_2.11;2.1.2 from central in [default]
	databricks#spark-sklearn;0.2.3 from spark-packages in [default]
	graphframes#graphframes;0.5.0-spark2.1-s_2.11 from spark-packages in [default]
	net.jpountz.lz4#lz4;1.3.0 from central in [default]
	org.apache.kafka#kafka-clients;0.10.0.1 from central in [default]
	org.apache.spark#spark-sql-kafka-0-10_2.11;2.2.1 from central in [default]
	org.apache.spark#spark-tags_2.11;2.2.1 from central in [default]
	org.scala-lang#scala-reflect;2.11.0 from central in [default]
	org.slf4j#slf4j-api;1.7.16 from central in [default]
	org.spark-project.spark#unused;1.0.0 from central in [default]
	org.xerial.snappy#snappy-java;1.1.2.6 from central in [default]
	:: evicted modules:
	org.slf4j#slf4j-api;1.7.7 by [org.slf4j#slf4j-api;1.7.16] in [default]
	---------------------------------------------------------------------
	|                  |            modules            ||   artifacts   |
	|       conf       | number| search|dwnlded|evicted|| number|dwnlded|
	---------------------------------------------------------------------
	|      default     |   13  |   13  |   13  |   1   ||   12  |   12  |
	---------------------------------------------------------------------
:: retrieving :: org.apache.spark#spark-submit-parent
	confs: [default]
	12 artifacts copied, 0 already retrieved (7101kB/45ms)
```
You can see **graphframes** being downloaded and spark being initialized. Now, if you go to your Spark UI (HELK's IP and port 4040), you will see that the Spark UI is running. You will see that one of the first messages is **"Executor driver added"**

[[https://github.com/Cyb3rWard0g/HELK/raw/master/resources/images/SPARK-UI.png]]

# Access HELK Web Interface
Open your preferred browser, go to your HELK's IP address, and enter the HELK credentials **(helk:hunting)**. By default, you will be presented by the Kibana's Home page. Once there, you could explore the different features that Kibana provides. I personally like to check the **Index Patterns** first and then **Discovery**

[[https://github.com/Cyb3rWard0g/HELK/raw/master/resources/images/KIBANA-Home.png]]

[[https://github.com/Cyb3rWard0g/HELK/raw/master/resources/images/KIBANA-IndexPatterns.png]]

[[https://github.com/Cyb3rWard0g/HELK/raw/master/resources/images/KIBANA-Discovery.png]]


I hope this document was helpful to deploy your own HELK. Let us know if you have any questions or if you think that this document can be improved. Feel free to create an **issue** for updates to this procedure. A more detailed **HOW-TO** will be developed soon to go into more details of how to use all the HELK components. 

IT IS HUNTING SEASON!!