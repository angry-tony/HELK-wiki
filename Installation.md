# Requirements
* **OS Name:**
  * Ubuntu 16.04 (preferred) 
  * HELK uses the official Docker Community Edition (CE) bash script (Edge Version) to install Docker for you. The Docker CE Edge script supports the following distros: **ubuntu**, **debian**, **raspbian**, **centos**, and **fedora**.
  * You can see the specific distro versions supported in the script [here](https://get.docker.com/).
  * If you have Docker & Docker-Compose already installed in your system, make sure you uninstall them to avoid old incompatible version. Let HELK use the official Docker CE Edge script execution to install Docker. 
* **Network Connection:** NAT or Bridge
* **RAM:** 16GB (minimum)
* **Cores:** 4 (minimum)
* **Disk:** 50gb for testing purposes and 100gb+ for production (minimum)
* **Applications:**
  * Docker: 18.05.0-ce+ & Docker-Compose (HELK INSTALLS THIS FOR YOU)
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
In order to make the installation of the HELK easy for everyone, the project comes with the helk_install.sh script that builds and runs everything you need to start the HELK automatically. You will just have to do the following:
* Set the HELK's IP. By default you can confirm that you want to use your HOST IP address for the HELK, unless you want to use a different one. Press \[Return\] or let the script continue on its own (30 Seconds sleep).
* Set the HELK's License Subscription. By default the HELK has the **basic** subscription selected. You can set it to **trial** if you want. If you want to learn more about subscriptions go [here](https://www.elastic.co/subscriptions) 

```
**********************************************
**          HELK - THE HUNTING ELK          **
**                                          **
** Author: Roberto Rodriguez (@Cyb3rWard0g) **
** HELK build version: v0.1.1-alpha07062018 **
** HELK ELK version: 6.3.1                  **
** License: GPL-3.0                         **
**********************************************
 
[HELK-INSTALLATION-INFO] HELK being hosted on a Linux box
[HELK-INSTALLATION-INFO] Available Memory: 11
[HELK-INSTALLATION-INFO] Available Disk: 40		
[HELK-INSTALLATION-INFO] Obtaining current host IP..
[HELK-INSTALLATION-INFO] Set HELK IP. Default value is your current IP: 192.168.64.131
[HELK-INSTALLATION-INFO] Set HELK License. Default value is basic: basic
[HELK-INSTALLATION-INFO] HELK IP set to 192.168.64.131
[HELK-INSTALLATION-INFO] HELK License set to basic
[HELK-INSTALLATION-INFO] Checking distribution list and version
....
......
```
# Always monitor HELK installation Logs
Once the installation kicks in, it will start showing you pre-defined messages about the installation, but no all the details of what is actually happening in the background. It is designed that way to keep your main screen clean and let you know where it is in the installation process.

What I recommend to do all the time is to open another shell and monitor the HELK installation logs by using the **tail** command and pointing it to the **/var/log/helk-install.log** file that gets created by the **helk_install** script as soon as it is run. This log file is available always on the host even if you are deploying the HELK via Docker.
```
helk@HELK:~$ tail -f /var/log/helk-install.log 
# Executing docker install script, commit: 36b78b2
+ sh -c apt-get update -qq >/dev/null
+ sh -c apt-get install -y -qq apt-transport-https ca-certificates curl >/dev/null
+ sh -c curl -fsSL "https://download.docker.com/linux/ubuntu/gpg" | apt-key add -qq - >/dev/null
+ sh -c echo "deb [arch=amd64] https://download.docker.com/linux/ubuntu xenial edge" > /etc/apt/sources.list.d/docker.list
+ [ ubuntu = debian ]
+ sh -c apt-get update -qq >/dev/null
+ sh -c apt-get install -y -qq --no-install-recommends docker-ce >/dev/null
+ sh -c docker version
Client:
 Version:      18.05.0-ce
 API version:  1.37
 Go version:   go1.9.5
 Git commit:   f150324
 Built:        Wed May  9 22:16:25 2018
 OS/Arch:      linux/amd64
 Experimental: false
 Orchestrator: swarm

Server:
 Engine:
  Version:      18.05.0-ce
  API version:  1.37 (minimum version 1.12)
  Go version:   go1.9.5
  Git commit:   f150324
  Built:        Wed May  9 22:14:32 2018
  OS/Arch:      linux/amd64
  Experimental: false
If you would like to use Docker as a non-root user, you should now consider
adding your user to the "docker" group with something like:

  sudo usermod -aG docker your-user

Remember that you will have to log out and back in for this to take effect!

WARNING: Adding a user to the "docker" group will grant the ability to run
         containers which can be used to obtain root privileges on the
         docker host.
         Refer to https://docs.docker.com/engine/security/security/#docker-daemon-attack-surface
         for more information.
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   617    0   617    0     0   1979      0 --:--:-- --:--:-- --:--:--  1983
100 8288k  100 8288k    0     0  2282k      0  0:00:03  0:00:03 --:--:-- 2507k
Creating network "helk_helk" with driver "bridge"
Creating volume "helk_esdata" with local driver
Pulling helk-elasticsearch (docker.elastic.co/elasticsearch/elasticsearch:6.3.1)...
6.3.1: Pulling from elasticsearch/elasticsearch
..
...
Creating helk-jupyter ... done
Creating helk-elasticsearch ... done
Digest: sha256:019081b63dd8a10826629de3fded6664f2a52a47f99323c190dfa9da3cdaf2d5
Status: Downloaded newer image for cyb3rward0g/helk-kafka-broker:1.1.0
Pulling helk-sigma (thomaspatzke/helk-sigma:latest)...
Creating helk-spark-master ... done
Digest: sha256:5a4568af3fc3a1e0c0ef37326553a9dd9b719ed3c93c93aed18d9131b755c1e0
Creating helk-zookeeper ... done
Pulling helk-logstash (docker.elastic.co/logstash/logstash:6.3.1)...
Creating helk-kibana ... done
Digest: sha256:e2e932fc7ce50e7e6c1152b4bd723db8c6e699524ec0fc27b5ea7580444c4d58
Status: Downloaded newer image for docker.elastic.co/logstash/logstash:6.3.1
Creating helk-kafka-broker ... done
Creating helk-spark-master ... 
Creating helk-kibana ... 
Creating helk-zookeeper ... 
Creating helk-logstash ... 
Creating helk-spark-worker ... 
Creating helk-spark-worker2 ... 
Creating helk-kafka-broker ... 
Creating helk-kafka-broker2 ... 
Creating helk-jupyter ... 
Creating helk-sigma ... 
Creating helk-nginx ... 
```
Once you see that the containers have been created you can check all the containers running by executing the following:

```
helk@HELK:~$ sudo docker ps
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                                                      NAMES
e625e89862dc        helk_helk-jupyter                           "./jupyter-entrypoin…"   About an hour ago   Up About an hour    0.0.0.0:4040-4050->4040-4050/tcp, 0.0.0.0:8880->8880/tcp   helk-jupyter
30e0bbeb472c        cyb3rward0g/helk-nginx:0.0.3                "./nginx-entrypoint.…"   About an hour ago   Up About an hour    0.0.0.0:80->80/tcp                                         helk-nginx
669a2cce38e2        cyb3rward0g/helk-kafka-broker:1.1.0         "./kafka-entrypoint.…"   About an hour ago   Up About an hour    0.0.0.0:9093->9093/tcp                                     helk-kafka-broker2
b9e72b736f11        cyb3rward0g/helk-kafka-broker:1.1.0         "./kafka-entrypoint.…"   About an hour ago   Up About an hour    0.0.0.0:9092->9092/tcp                                     helk-kafka-broker
cfcfd0607e41        cyb3rward0g/helk-spark-worker:2.3.1         "./spark-worker-entr…"   About an hour ago   Up About an hour    0.0.0.0:8082->8082/tcp                                     helk-spark-worker2
7ddf75da3e5b        cyb3rward0g/helk-spark-worker:2.3.1         "./spark-worker-entr…"   About an hour ago   Up About an hour    0.0.0.0:8081->8081/tcp                                     helk-spark-worker
71a0a38968d2        docker.elastic.co/kibana/kibana:6.3.1       "/usr/share/kibana/s…"   About an hour ago   Up About an hour    5601/tcp                                                   helk-kibana
e4c89c73b6aa        docker.elastic.co/logstash/logstash:6.3.1   "/usr/local/bin/dock…"   About an hour ago   Up About an hour    5044/tcp, 9600/tcp                                         helk-logstash
2a21f25f1879        cyb3rward0g/helk-zookeeper:3.4.10           "./zookeeper-entrypo…"   About an hour ago   Up About an hour    2888/tcp, 0.0.0.0:2181->2181/tcp, 3888/tcp                 helk-zookeeper
a494b87627f8        cyb3rward0g/helk-spark-master:2.3.1         "./spark-master-entr…"   About an hour ago   Up About an hour    0.0.0.0:7077->7077/tcp, 0.0.0.0:8080->8080/tcp             helk-spark-master
d19d0d9d0533        helk_helk-elasticsearch                     "/usr/local/bin/dock…"   About an hour ago   Up About an hour    9200/tcp, 9300/tcp                                         helk-elasticsearch
```

If you want to monitor the resources being utilized (Memory, CPU, etc), you can run the following:
```
helk@HELK:~$ sudo docker stats --all
CONTAINER ID        NAME                 CPU %               MEM USAGE / LIMIT     MEM %               NET I/O             BLOCK I/O           PIDS
d7543613aa67        helk-sigma           0.00%               0B / 0B               0.00%               0B / 0B             0B / 0B             0
e625e89862dc        helk-jupyter         1.87%               40.53MiB / 11.44GiB   0.35%               1.1kB / 138B        76.1MB / 65.5kB     1
30e0bbeb472c        helk-nginx           4.19%               172KiB / 11.44GiB     0.00%               11.2kB / 13.7kB     19.4MB / 8.59MB     2
669a2cce38e2        helk-kafka-broker2   15.57%              265.2MiB / 11.44GiB   2.26%               136kB / 116kB       45.3MB / 26MB       66
b9e72b736f11        helk-kafka-broker    14.19%              317.4MiB / 11.44GiB   2.71%               188kB / 195kB       30.7MB / 9.08MB     68
cfcfd0607e41        helk-spark-worker2   5.19%               157.5MiB / 11.44GiB   1.34%               4.8kB / 18.7kB      28.8MB / 4.26MB     26
7ddf75da3e5b        helk-spark-worker    5.18%               147.8MiB / 11.44GiB   1.26%               5.78kB / 20.4kB     77.6MB / 221kB      26
71a0a38968d2        helk-kibana          4.29%               292KiB / 11.44GiB     0.00%               11kB / 13.6kB       17.7MB / 17MB       2
e4c89c73b6aa        helk-logstash        68.06%              741.1MiB / 11.44GiB   6.33%               1.92kB / 1.19kB     281MB / 39.3MB      28
2a21f25f1879        helk-zookeeper       2.38%               54.35MiB / 11.44GiB   0.46%               84.1kB / 93.7kB     50MB / 647kB        23
a494b87627f8        helk-spark-master    6.82%               162.1MiB / 11.44GiB   1.38%               40.2kB / 8.56kB     61MB / 106kB        35
d19d0d9d0533        helk-elasticsearch   46.30%              3.973GiB / 11.44GiB   34.73%              29.4kB / 21.1kB     1.59GB / 4.29GB     19
```

You should also monitor the logs of each container while they are being initialized:

Just run the following:

```
helk@HELK:~$ sudo docker logs --follow helk-elasticsearch
OpenJDK 64-Bit Server VM warning: Option UseConcMarkSweepGC was deprecated in version 9.0 and will likely be removed in a future release.
[2018-07-11T17:06:48,679][INFO ][o.e.n.Node               ] [helk-1] initializing ...
[2018-07-11T17:06:48,836][INFO ][o.e.e.NodeEnvironment    ] [helk-1] using [1] data paths, mounts [[/usr/share/elasticsearch/data (/dev/mapper/HELK--vg-root)]], net usable_space [35.4gb], net total_space [43.8gb], types [ext4]
[2018-07-11T17:06:48,837][INFO ][o.e.e.NodeEnvironment    ] [helk-1] heap size [5.9gb], compressed ordinary object pointers [true]
[2018-07-11T17:06:48,840][INFO ][o.e.n.Node               ] [helk-1] node name [helk-1], node ID [AXTFJ_-HTy2_coCqh6HBCg]
[2018-07-11T17:06:48,844][INFO ][o.e.n.Node               ] [helk-1] version[6.3.1], pid[1], build[default/tar/eb782d0/2018-06-29T21:59:26.107521Z], OS[Linux/4.4.0-87-generic/amd64], JVM[Oracle Corporation/OpenJDK 64-Bit Server VM/10.0.1/10.0.1+10]
[2018-07-11T17:06:48,844][INFO ][o.e.n.Node               ] [helk-1] JVM arguments [-Xms1g, -Xmx1g, -XX:+UseConcMarkSweepGC, -XX:CMSInitiatingOccupancyFraction=75, -XX:+UseCMSInitiatingOccupancyOnly, -XX:+AlwaysPreTouch, -Xss1m, -Djava.awt.headless=true, -Dfile.encoding=UTF-8, -Djna.nosys=true, -XX:-OmitStackTraceInFastThrow, -Dio.netty.noUnsafe=true, -Dio.netty.noKeySetOptimization=true, -Dio.netty.recycler.maxCapacityPerThread=0, -Dlog4j.shutdownHookEnabled=false, -Dlog4j2.disable.jmx=true, -Djava.io.tmpdir=/tmp/elasticsearch.NDCUgQDZ, -XX:+HeapDumpOnOutOfMemoryError, -XX:HeapDumpPath=data, -XX:ErrorFile=logs/hs_err_pid%p.log, -Xlog:gc*,gc+age=trace,safepoint:file=logs/gc.log:utctime,pid,tags:filecount=32,filesize=64m, -Djava.locale.providers=COMPAT, -Des.cgroups.hierarchy.override=/, -Xms6g, -Xmx6g, -Des.path.home=/usr/share/elasticsearch, -Des.path.conf=/usr/share/elasticsearch/config, -Des.distribution.flavor=default, -Des.distribution.type=tar]
[2018-07-11T17:06:53,727][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [aggs-matrix-stats]
[2018-07-11T17:06:53,732][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [analysis-common]
[2018-07-11T17:06:53,732][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [ingest-common]
[2018-07-11T17:06:53,734][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [lang-expression]
[2018-07-11T17:06:53,734][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [lang-mustache]
[2018-07-11T17:06:53,735][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [lang-painless]
[2018-07-11T17:06:53,735][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [mapper-extras]
[2018-07-11T17:06:53,735][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [parent-join]
[2018-07-11T17:06:53,735][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [percolator]
[2018-07-11T17:06:53,735][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [rank-eval]
[2018-07-11T17:06:53,735][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [reindex]
[2018-07-11T17:06:53,735][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [repository-url]
[2018-07-11T17:06:53,735][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [transport-netty4]
[2018-07-11T17:06:53,735][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [tribe]
[2018-07-11T17:06:53,735][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [x-pack-core]
[2018-07-11T17:06:53,735][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [x-pack-deprecation]
[2018-07-11T17:06:53,735][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [x-pack-graph]
[2018-07-11T17:06:53,735][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [x-pack-logstash]
[2018-07-11T17:06:53,741][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [x-pack-ml]
[2018-07-11T17:06:53,741][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [x-pack-monitoring]
[2018-07-11T17:06:53,741][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [x-pack-rollup]
[2018-07-11T17:06:53,741][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [x-pack-security]
[2018-07-11T17:06:53,741][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [x-pack-sql]
[2018-07-11T17:06:53,741][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [x-pack-upgrade]
[2018-07-11T17:06:53,741][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [x-pack-watcher]
[2018-07-11T17:06:53,742][INFO ][o.e.p.PluginsService     ] [helk-1] loaded plugin [ingest-geoip]
[2018-07-11T17:06:53,742][INFO ][o.e.p.PluginsService     ] [helk-1] loaded plugin [ingest-user-agent]
[2018-07-11T17:07:01,205][INFO ][o.e.x.m.j.p.l.CppLogMessageHandler] [controller/76] [Main.cc@109] controller (64 bit): Version 6.3.1 (Build 4d0b8f0a0ef401) Copyright (c) 2018 Elasticsearch BV
[2018-07-11T17:07:02,748][INFO ][o.e.d.DiscoveryModule    ] [helk-1] using discovery type [single-node]
[2018-07-11T17:07:04,389][INFO ][o.e.n.Node               ] [helk-1] initialized
[2018-07-11T17:07:04,390][INFO ][o.e.n.Node               ] [helk-1] starting ...
[2018-07-11T17:07:04,848][INFO ][o.e.t.TransportService   ] [helk-1] publish_address {172.18.0.2:9300}, bound_addresses {0.0.0.0:9300}
[2018-07-11T17:07:05,036][INFO ][o.e.h.n.Netty4HttpServerTransport] [helk-1] publish_address {172.18.0.2:9200}, bound_addresses {0.0.0.0:9200}
[2018-07-11T17:07:05,036][INFO ][o.e.n.Node               ] [helk-1] started
[2018-07-11T17:07:05,317][INFO ][o.e.g.GatewayService     ] [helk-1] recovered [0] indices into cluster_state
[2018-07-11T17:07:05,742][INFO ][o.e.c.m.MetaDataIndexTemplateService] [helk-1] adding template [.watches] for index patterns [.watches*]
[2018-07-11T17:07:05,785][INFO ][o.e.c.m.MetaDataIndexTemplateService] [helk-1] adding template [.triggered_watches] for index patterns [.triggered_watches*]
[2018-07-11T17:07:05,910][INFO ][o.e.c.m.MetaDataIndexTemplateService] [helk-1] adding template [.watch-history-7] for index patterns [.watcher-history-7*]
[2018-07-11T17:07:06,024][INFO ][o.e.c.m.MetaDataIndexTemplateService] [helk-1] adding template [.monitoring-logstash] for index patterns [.monitoring-logstash-6-*]
[2018-07-11T17:07:06,135][INFO ][o.e.c.m.MetaDataIndexTemplateService] [helk-1] adding template [.monitoring-es] for index patterns [.monitoring-es-6-*]
[2018-07-11T17:07:06,214][INFO ][o.e.c.m.MetaDataIndexTemplateService] [helk-1] adding template [.monitoring-alerts] for index patterns [.monitoring-alerts-6]
[2018-07-11T17:07:06,329][INFO ][o.e.c.m.MetaDataIndexTemplateService] [helk-1] adding template [.monitoring-beats] for index patterns [.monitoring-beats-6-*]
[2018-07-11T17:07:06,371][INFO ][o.e.c.m.MetaDataIndexTemplateService] [helk-1] adding template [.monitoring-kibana] for index patterns [.monitoring-kibana-6-*]
[2018-07-11T17:07:06,616][INFO ][o.e.l.LicenseService     ] [helk-1] license [de9d2c22-1c5a-4888-9e04-4433b4f3b669] mode [basic] - valid
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
HELK KIBANA & ELASTICSEARCH USER: helk
HELK KIBANA & ELASTICSEARCH PASSWORD: hunting
HELK JUPYTER CURRENT TOKEN: 3ff8ccfb7f9d4fb6faea7a381ed15d69a6250d359fd0d45f
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