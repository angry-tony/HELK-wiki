# Requirements
* **OS Name:**
Specific Linux distros or OS X supported if Docker & Docker-Compose are not installed yet. If so, then the HELK images should work fine with any OS.
    * HELK uses the official Docker Community Edition (CE) bash script (Edge Version) to install Docker if it is not installed yet. The Docker CE Edge script supports the following distros: **ubuntu**, **debian**, **raspbian**, **centos**, and **fedora**.
    * You can see the specific distro versions supported in the script [here](https://get.docker.com/).
    * If you have Docker & Docker-Compose already installed in your system, then you should be good to go (this would skip the Docker CE Edge script execution). 
* **Network Connection:** NAT or Bridge
* **RAM:** 16GB (minimum)
* **Cores:** 4 (minimum)
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
** HELK ELK version: 6.2.2                  **
** License: BSD 3-Clause                    **
**********************************************
 
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
** HELK ELK version: 6.2.2                  **
** License: BSD 3-Clause                    **
**********************************************
 
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
Creating network "helk_helk" with driver "bridge"
Creating volume "helk_esdata" with local driver
Pulling helk-elk (cyb3rward0g/helk-elk:6.2.2)...
6.2.2: Pulling from cyb3rward0g/helk-elk
Digest: sha256:05025efb6c2fc0fef2790859aa98bb28c7c20e541bdbabda6751960a5adb65c2
Status: Downloaded newer image for cyb3rward0g/helk-elk:6.2.2
Pulling helk-kafka (cyb3rward0g/helk-kafka:1.0.0)...
1.0.0: Pulling from cyb3rward0g/helk-kafka
Digest: sha256:58b9252bf43e79734c935d4fac6ac910ad25ff6aeb1db8b6e6f699ccbb08a0b7
Status: Downloaded newer image for cyb3rward0g/helk-kafka:1.0.0
Pulling helk-analytics (cyb3rward0g/helk-analytics:0.0.1)...
0.0.1: Pulling from cyb3rward0g/helk-analytics
Digest: sha256:a3995ccb75da7a5d27736e5111f6db3d6d7bfff18d1618d0d66ee62086e995e0
Status: Downloaded newer image for cyb3rward0g/helk-analytics:0.0.1
Creating helk-kafka ... done
Creating helk-kafka ... 
Creating helk-analytics ...
```
Other logs that you should monitor also to see if everything is running properly after the installation are the following ones:
* **Elasticsearch:**
   * /var/log/elasticsearch/elasticsearch.log
* **Logstash:**
  * /var/log/logstash/logstash-plain.log
* **Kibana:**
  * /var/log/kibana/kibana.log
* **Spark:**
  * /var/log/spark/spark_pyspark.log
* **Cerebro:**
  * /var/log/cerebro/cerebro.log
* **Kafka:**
  * /var/log/kafka/helk-kafka_zookeeper.log
  * /var/log/kafka/helk-kafka.log
  * /var/log/kafka/helk-kafka_1.log
  * /var/log/kafka/helk-kafka_2.log
* **Analytics**
  * /var/log/analytics/analytics.log

Remember that the elasticsearch, logstash, kibana, nginx, Kafka and Analytic logs are available inside of your docker images. You can access your docker images by running the following commands:
```
sudo docker exec -ti helk-elasticsearch bash
root@7a9d6443a4bf:/opt/helk/scripts# 
```
```
sudo docker exec -ti helk-kibana bash
root@7a9d6883a4bf:/opt/helk/scripts# 
```
```
sudo docker exec -ti helk-logstash bash
root@7a9d6773a4bf:/opt/helk/scripts# 
```
```
sudo docker exec -ti helk-nginx bash
root@7a9d6333a4bf:/opt/helk/scripts# 
```
```
sudo docker exec -ti helk-kafka bash
root@7a9d6443a4bf:/opt/helk/scripts# 
```
```
sudo docker exec -ti helk-analytics bash
root@7a9d6443a4bf:/opt/helk/scripts# 
```
You can also monitor your **Docker logs**. The **"Docker logs"** command batch-retrieves logs present at the time of execution of the **entrypoint** scripts. This will allow you, for example, to see the execution of the [elk-entrypoint.sh](https://github.com/Cyb3rWard0g/HELK/blob/master/helk-elasticsearch/scripts/entrypoint.sh) bash script once the the docker image is run. Run the following commands:
```
sudo docker logs helk-ellasticsearch
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
HELK CEREBRO URL: http://192.168.64.131:9000
HELK KIBANA & ELASTICSEARCH USER: helk
HELK KIBANA & ELASTICSEARCH PASSWORD: hunting
HELK JUPYTER CURRENT TOKEN: 3f46301da4cd20011391327647000e8006ee3574cab0b163
HELK SPARK UI: http://192.168.64.131:4040
HELK JUPYTER LAB URL: http://192.168.64.131:8880/lab
 
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