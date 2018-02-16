# Requirements
* **OS Name:**
Depending on what option you choose to use when installing the HELK, the operating system requirement varies.
  * **Docker (Option 1-2)**: Specific Linux distros or OS X supported
    * HELK uses the official Docker Community Edition (CE) bash script (Edge Version) to install Docker if it is not installed yet. The Docker CE Edge script supports the following distros: **ubuntu**, **debian**, **raspbian**, **centos**, and **fedora**.
    * You can see the specific distro versions supported in the script [here](https://get.docker.com/).
    * If you have Docker already installed in your system, then you should be good to go (this would skip the Docker CE Edge script execution). 
  * **Bash Script (Option 3)**: Linux Debian-based systems ONLY 
* **Network Connection:** NAT or Bridge
* **RAM:** 12GB (minimum)
* **Applications:**
  * Docker (Needed for options 1-2 ONLY as explained above)
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
# HELK Menu
In order to make the installation of the HELK easy for everyone, the project comes with a menu where all you have to do is choose how you want to install the HELK. Just pick the option that you want to use, and it will do it all for you. You will just have to confirm that you want to use your HOST IP address for the HELK, unless you want to use a different one. Press \[Return\] or let the script continue on its own (30 Seconds sleep).
* **Option 1:** Pulls and runs the latest HELK Docker image from [Cyb3rWard0g Docker Hub](https://hub.docker.com/r/cyb3rward0g/helk/).
* **Option 2:** Builds and runs the HELK from its local Dockerfile.
* **Option 3:** Installs and runs the HELK from its local Bash Script.
```
**********************************************
**           HELK - M E N U                 **
**                                          **
** Author: Roberto Rodriguez (@Cyb3rWard0g) **
** HELK build version: 0.9 (Alpha)          **
** HELK ELK version: 6.2.0                  **
** License: BSD 3-Clause                    **
**********************************************
 
1. Pull the latest HELK image from DockerHub
2. Build the HELK image from local Dockerfile
3. Install the HELK from local bash script
4. Exit
 
[HELK-INSTALLATION-INFO] Enter choice [ 1 - 4]
```
# Monitor HELK installation Logs
Once you choose how you want to install the HELK, it will start showing you pre-defined messages about the installation, but no all the details of what is actually happening in the background. It is designed that way to keep your main screen clean and let you know where it is in the installation process.
```
**********************************************
**           HELK - M E N U                 **
**                                          **
** Author: Roberto Rodriguez (@Cyb3rWard0g) **
** HELK build version: 0.9 (Alpha)          **
** HELK ELK version: 6.2.0                  **
** License: BSD 3-Clause                    **
**********************************************
 
1. Pull the latest HELK image from DockerHub
2. Build the HELK image from local Dockerfile
3. Install the HELK from local bash script
4. Exit
 
[HELK-INSTALLATION-INFO] Enter choice [ 1 - 4] 2
[HELK-INSTALLATION-INFO] Obtaining current host IP..
[HELK-INSTALLATION-INFO] Set HELK IP. Default value is your current IP: 192.168.64.131
[HELK-INSTALLATION-INFO] HELK IP set to 192.168.64.131
[HELK-DOCKER-INSTALLATION-INFO] HELK identified Linux as the system kernel
[HELK-DOCKER-INSTALLATION-INFO] Checking distribution list and version
[HELK-DOCKER-INSTALLATION-INFO] You're using ubuntu version xenial
[HELK-DOCKER-INSTALLATION-INFO] Docker is not installed
[HELK-DOCKER-INSTALLATION-INFO] Checking if curl is installed first
[HELK-DOCKER-INSTALLATION-INFO] curl is already installed
[HELK-DOCKER-INSTALLATION-INFO] Ready to install  Docker..
[HELK-DOCKER-INSTALLATION-INFO] Installing docker via convenience script..
...
....
```
What I recommend to do all the time is to open another shell and monitor the HELK installation logs by using the **tail** command and pointing it to the **/var/log/helk-install.log** file. This log file is available always on the host even if you are deploying the HELK via Docker.
```
helk@HELK:~$ tail -f /var/log/helk-install.log
# Executing docker install script, commit: 1d31602
+ sh -c apt-get update -qq >/dev/null
+ sh -c apt-get install -y -qq apt-transport-https ca-certificates curl >/dev/null
+ sh -c curl -fsSL "https://download.docker.com/linux/ubuntu/gpg" | apt-key add -qq - >/dev/null
+ sh -c echo "deb [arch=amd64] https://download.docker.com/linux/ubuntu xenial edge" > /etc/apt/sources.list.d/docker.list
+ [ ubuntu = debian ]
+ sh -c apt-get update -qq >/dev/null
+ sh -c apt-get install -y -qq --no-install-recommends docker-ce >/dev/null
+ sh -c docker version
...
....
Step 2/70 : MAINTAINER Roberto Rodriguez @Cyb3rWard0g
 ---> Running in 7d3af2d63362
Removing intermediate container 7d3af2d63362
 ---> 359fe73cc17d
Step 3/70 : LABEL description="Dockerfile base for the HELK."
 ---> Running in 883643621f9e
Removing intermediate container 883643621f9e
 ---> 142e6c572022
Step 4/70 : ENV DEBIAN_FRONTEND noninteractive
 ---> Running in 802e90385030
Removing intermediate container 802e90385030
 ---> 4a78d69d6a44
Step 5/70 : RUN echo "[HELK-DOCKER-INSTALLATION-INFO] Updating Ubuntu base image.."   && apt-get update -qq   && echo "[HELK-DOCKER-INSTALLATION-INFO] Extracting templates from packages.."   && apt-get install -qqy   openjdk-8-jre-headless   wget   sudo   nano   apt-transport-https   python   python-pip   unzip
...
....
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

If you are using Docker to deploy your HELK, remember that the ELK logs are available inside of your docker image. You can access your docker image by running the following commands:
```
sudo docker exec -ti helk bash
root@7a9d6443a4bf:/opt/helk/scripts# 
```
I also recommend to monitor your **Docker logs**. The **"Docker logs"** command batch-retrieves logs present at the time of execution. This will allow you, for example, to see the execution of the [helk_docker_entrypoint.sh](https://github.com/Cyb3rWard0g/HELK/blob/master/scripts/helk_docker_entrypoint.sh) bash script once the the docker image is run during the installation. Run the following commands:
```
sudo docker logs helk
```
# Final Details
Once your HELK installation ends, you will be presented with information that you will need to access the HELK and all its features. Either you install the HELK via Docker or local bash sccript, you will have the Jupyter Notebook server running and ready to be used. **You will still need to access the Jupyter web interface and access/create a notebook in order to initialize Spark. This is because HELK uses PYSPARK and it depends on Jupyter Notebooks**.  You will have the following message showing in your main screen:
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
HELK JUPYTER CURRENT TOKEN: b5b4e1879177b1d6180cebcfb24cc5c777b19c92b7661061
HELK SPARK UI: http://192.168.64.131:4040
HELK JUPYTER NOTEBOOK URI: http://192.168.64.131:8880
HELK DOCKER BASH ACCESS: sudo docker exec -ti helk bash
 
IT IS HUNTING SEASON!!!!!
```
Use the **JUPYTER CURRENT TOKEN** to access the Jupyter Notebook web interface.

# Starting Jupyter Kernel & Spark UI
One of the questions that I had recently was **why the Spark UI is not available after installation**. This is because the HELK uses PYSPARK  (Python API) to interact with Spark and it depends on Jupyter's Kernel being initialized. When the installation of the HELK is done, if you check your **Docker Logs** (sudo docker logs helk) or the log **/var/log/spark/spark_pyspark.log**, you will see the following message:
```
[I 12:23:37.462 NotebookApp] Serving notebooks from local directory: /opt/helk/
[I 12:23:37.463 NotebookApp] 0 active kernels
[I 12:23:37.463 NotebookApp] The Jupyter Notebook is running at:
[I 12:23:37.463 NotebookApp] http://[all ip addresses on your system]:8880/?token=8be5c57adc5cc4a25a1d95b56d1e46ed0d559ec67706881d
[I 12:23:37.463 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 12:23:37.463 NotebookApp] 
    
    Copy/paste this URL into your browser when you connect for the first time,
    to login with a token:
        http://localhost:8880/?token=8be5c57adc5cc4a25a1d95b56d1e46ed0d559ec67706881d
[I 12:23:37.575 NotebookApp] 302 GET / (172.17.0.1) 5.41ms
```
That tells you that the Jupyter Server is running, but it does not have an active kernel yet. Therefore, a **Spark executor driver** has not being assigned yet. Also, you will see a **302** message in the spark logs, and that is fine because you still need to access the Jupyter web interface using your **Jupyter token**.

Open your preferred browser, go to your Jupyter Interface, and enter your Jupyter token. That Jupyter URL is defined in your **HELK JUPYTER NOTEBOOK URI:** message. It is basically your HELK's IP and port 8880

[[https://github.com/Cyb3rWard0g/HELK/raw/master/resources/images/JUPYTER-Token.png]]


Open the **Check_Spark_Graphframes_Integrations.ipynb** notebook

[[https://github.com/Cyb3rWard0g/HELK/raw/master/resources/images/JUPYTER-Tree.png]]


Once you access the notebook, you will see a message on the top right of your notebook saying that the **Kernel is starting,Please Wait..**

[[https://github.com/Cyb3rWard0g/HELK/raw/master/resources/images/JUPYTER-InitializeKernel.png]]

If you check your Docker logs you will see the following:
```
[I 15:25:32.762 NotebookApp] Kernel started: eeb1b4f7-47ba-4d7a-9faf-512c5bc3f161
Ivy Default Cache set to: /root/.ivy2/cache
The jars for the packages stored in: /root/.ivy2/jars
:: loading settings :: url = jar:file:/opt/helk/spark/spark-2.2.1-bin-hadoop2.7/jars/ivy-2.4.0.jar!/org/apache/ivy/core/settings/ivysettings.xml
graphframes#graphframes added as a dependency
org.apache.spark#spark-sql-kafka-0-10_2.11 added as a dependency
databricks#spark-sklearn added as a dependency
:: resolving dependencies :: org.apache.spark#spark-submit-parent;1.0
	confs: [default]
[W 15:25:42.980 NotebookApp] Timeout waiting for kernel_info reply from eeb1b4f7-47ba-4d7a-9faf-512c5bc3f161
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
	[SUCCESSFUL ] graphframes#graphframes;0.5.0-spark2.1-s_2.11!graphframes.jar (7939ms)
downloading https://repo1.maven.org/maven2/org/apache/spark/spark-sql-kafka-0-10_2.11/2.2.1/spark-sql-kafka-0-10_2.11-2.2.1.jar ...
	[SUCCESSFUL ] org.apache.spark#spark-sql-kafka-0-10_2.11;2.2.1!spark-sql-kafka-0-10_2.11.jar (318ms)
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