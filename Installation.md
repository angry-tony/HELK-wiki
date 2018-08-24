# Requirements
* **OS Name:**
  * Ubuntu 16.04 (preferred) 
  * HELK uses the official Docker Community Edition (CE) bash script (Edge Version) to install Docker for you. The Docker CE Edge script supports the following distros: **ubuntu**, **debian**, **raspbian**, **centos**, and **fedora**.
  * You can see the specific distro versions supported in the script [here](https://get.docker.com/).
  * If you have Docker & Docker-Compose already installed in your system, make sure you uninstall them to avoid old incompatible version. Let HELK use the official Docker CE Edge script execution to install Docker. 
* **Network Connection:** NAT or Bridge
* **RAM:** 12GB (minimum)
* **Cores:** 4 (minimum)
* **Disk:** 30gb for testing purposes and 100gb+ for production (minimum)
* **Applications:**
  * Docker: 18.06.1-ce+ & Docker-Compose (HELK INSTALLS THIS FOR YOU)
  * Winlogbeat running on your endpoints. You can install Winlogbeat by following one of [@Cyb3rWard0g](https://twitter.com/Cyb3rWard0g) posts [here](https://cyberwardog.blogspot.com/2017/02/setting-up-pentesting-i-mean-threat_87.html). Make sure you use the [winlogbeat config](https://github.com/Cyb3rWard0g/HELK/blob/master/winlogbeat/winlogbeat.yml) recommended by the HELK since it uses the [Kafka output plugin](https://www.elastic.co/guide/en/beats/winlogbeat/current/kafka-output.html) and it is already pointing to the right ports with recommended options. You will just have to add your HELK's IP address. 
# HELK Download
Run the following commands to clone the HELK repo via git.
```
git clone https://github.com/Cyb3rWard0g/HELK.git
```
Change your current directory location to the new HELK directory, and run the **helk_install.sh** bash script as root.
```
cd HELK/docker
sudo ./helk_install.sh
```
# HELK Install
In order to make the installation of the HELK easy for everyone, the project comes with an install script named **helk_install.sh**. This script builds and runs everything you need to build and run the HELK automatically. During the installation process, the script will allow you to set up the following:
* Set the HELK's IP. By default you can confirm that you want to use your HOST IP address for the HELK, unless you want to use a different one. Press \[Return\] or let the script continue on its own (30 Seconds sleep).
* Set the HELK's License Subscription. By default the HELK has the **basic** subscription selected. You can set it to **trial** if you want. If you want to learn more about subscriptions go [here](https://www.elastic.co/subscriptions) 

```
**********************************************
**          HELK - THE HUNTING ELK          **
**                                          **
** Author: Roberto Rodriguez (@Cyb3rWard0g) **
** HELK build version: v0.1.2-alpha08022018 **
** HELK ELK version: 6.3.2                  **
** License: GPL-3.0                         **
**********************************************
 
[HELK-INSTALLATION-INFO] HELK being hosted on a Linux box
[HELK-INSTALLATION-INFO] Available Memory: 15
[HELK-INSTALLATION-INFO] Available Disk: 30		
[HELK-INSTALLATION-INFO] Obtaining current host IP..
[HELK-INSTALLATION-INFO] Set HELK IP. Default value is your current IP: 192.168.1.208
[HELK-INSTALLATION-INFO] Set HELK License. Default value is basic: basic
[HELK-INSTALLATION-INFO] HELK IP set to 192.168.1.208
[HELK-INSTALLATION-INFO] HELK License set to basic
....
......
```
# Monitor HELK installation Logs (Always)
Once the installation kicks in, it will start showing you pre-defined messages about the installation, but no all the details of what is actually happening in the background. It is designed that way to keep your main screen clean and let you know where it is in the installation process.

What I recommend to do all the time is to open another shell and monitor the HELK installation logs by using the **tail** command and pointing it to the **/var/log/helk-install.log** file that gets created by the **helk_install** script as soon as it is run. This log file is available on your local host even if you are deploying the HELK via Docker (I want to make sure it is clear that it is a local file).
```
helk@HELK:~$ tail -f /var/log/helk-install.log 
# Executing docker install script, commit: 36b78b2
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
 Version:           18.06.0-ce
 API version:       1.38
 Go version:        go1.10.3
 Git commit:        0ffa825
 Built:             Wed Jul 18 19:11:02 2018
 OS/Arch:           linux/amd64
 Experimental:      false

Server:
 Engine:
  Version:          18.06.0-ce
  API version:      1.38 (minimum version 1.12)
  Go version:       go1.10.3
  Git commit:       0ffa825
  Built:            Wed Jul 18 19:09:05 2018
  OS/Arch:          linux/amd64
  Experimental:     false
..
...
Creating network "docker_helk" with driver "bridge"
Creating volume "docker_esdata" with local driver
Pulling helk-elasticsearch (docker.elastic.co/elasticsearch/elasticsearch:6.3.2)...
6.3.2: Pulling from elasticsearch/elasticsearch
Digest: sha256:8f06aecf7227dbc67ee62d8d05db680f8a29d0296ecd74c60d21f1fe665e04b0
Status: Downloaded newer image for docker.elastic.co/elasticsearch/elasticsearch:6.3.2
Pulling helk-spark-master (cyb3rward0g/helk-spark-master:2.3.1-a)...
2.3.1-a: Pulling from cyb3rward0g/helk-spark-master
Digest: sha256:65b35c0b58ae1c66e95bd53e59cc5d3b98963334127ce8290bf959eb1d70c7fd
Status: Downloaded newer image for cyb3rward0g/helk-spark-master:2.3.1-a
Pulling helk-spark-worker (cyb3rward0g/helk-spark-worker:2.3.1-a)...
2.3.1-a: Pulling from cyb3rward0g/helk-spark-worker
Digest: sha256:eeca5cfa8fb11d0d741a11338e3578d29326bf213952eae8aa9c511d75290a8e
Status: Downloaded newer image for cyb3rward0g/helk-spark-worker:2.3.1-a
Pulling helk-kibana (docker.elastic.co/kibana/kibana:6.3.2)...
6.3.2: Pulling from kibana/kibana
Digest: sha256:7ae0616a5f5ddfb0f93ae4cc94038afd2d4e45fa7858fd39c506c8c682ee71f0
Status: Downloaded newer image for docker.elastic.co/kibana/kibana:6.3.2
Pulling helk-nginx (cyb3rward0g/helk-nginx:0.0.5)...
0.0.5: Pulling from cyb3rward0g/helk-nginx
Digest: sha256:2b3b0c4e7c82f2b3632511a0b0a0fcb0911d0f4edfd3e7be19982c9b4b33840f
Status: Downloaded newer image for cyb3rward0g/helk-nginx:0.0.5
Pulling helk-jupyter (cyb3rward0g/helk-jupyter:0.0.3)...
0.0.3: Pulling from cyb3rward0g/helk-jupyter
Digest: sha256:5b30ecf274e76659ab7180d883e56f5924eee216e32e1d73bc7fe8b969f0662e
Status: Downloaded newer image for cyb3rward0g/helk-jupyter:0.0.3
Pulling helk-zookeeper (cyb3rward0g/helk-zookeeper:1.1.1)...
1.1.1: Pulling from cyb3rward0g/helk-zookeeper
Digest: sha256:4e2ccac9fb982afe808587d8905a9d0245c09748da863b370643d53bed9b5aec
Status: Downloaded newer image for cyb3rward0g/helk-zookeeper:1.1.1
Pulling helk-kafka-broker (cyb3rward0g/helk-kafka-broker:1.1.1)...
Creating helk-elasticsearch ... done
Digest: sha256:8d348199eddc60820e107c4a3ac78a3fdb55ba794cd450d85537023ac407cd69
Status: Downloaded newer image for cyb3rward0g/helk-kafka-broker:1.1.1
Creating helk-spark-master ... done
latest: Pulling from thomaspatzke/helk-sigma
Creating helk-kibana ... done
Status: Downloaded newer image for thomaspatzke/helk-sigma:latest
Pulling helk-logstash (docker.elastic.co/logstash/logstash:6.3.2)...
Creating helk-nginx ... done
Creating helk-zookeeper ... done
Status: Downloaded newer image for docker.elastic.co/logstash/logstash:6.3.2
Creating helk-kafka-broker2 ... done
Creating helk-logstash ... 
Creating helk-spark-master ... 
Creating helk-kibana ... 
Creating helk-spark-worker2 ... 
Creating helk-spark-worker ... 
Creating helk-nginx ... 
Creating helk-zookeeper ... 
Creating helk-sigma ... 
Creating helk-jupyter ... 
Creating helk-kafka-broker2 ... 
Creating helk-kafka-broker ...  
```
Once you see that the containers have been created you can check all the containers running by executing the following:

```
helk@HELK:~$ sudo docker ps
CONTAINER ID        IMAGE                                                 COMMAND                  CREATED             STATUS              PORTS                                        NAMES
40520f1dac56        cyb3rward0g/helk-kafka-broker:1.1.1                   "./kafka-entrypoint.…"   2 hours ago         Up 2 hours          0.0.0.0:9092->9092/tcp                       helk-kafka-broker
cfb55e9403f5        cyb3rward0g/helk-kafka-broker:1.1.1                   "./kafka-entrypoint.…"   2 hours ago         Up 2 hours          0.0.0.0:9093->9093/tcp                       helk-kafka-broker2
4d727052c9d0        cyb3rward0g/helk-jupyter:0.0.3                        "./jupyter-entrypoin…"   2 hours ago         Up 2 hours          8000/tcp                                     helk-jupyter
ba858cbef600        cyb3rward0g/helk-zookeeper:1.1.1                      "./zookeeper-entrypo…"   2 hours ago         Up 2 hours          2888/tcp, 0.0.0.0:2181->2181/tcp, 3888/tcp   helk-zookeeper
cdb16480a8fb        cyb3rward0g/helk-nginx:0.0.5                          "./nginx-entrypoint.…"   2 hours ago         Up 2 hours          0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp     helk-nginx
e882198552e6        cyb3rward0g/helk-spark-worker:2.3.1-a                 "./spark-worker-entr…"   2 hours ago         Up 2 hours          0.0.0.0:8081->8081/tcp                       helk-spark-worker
2097a326e131        cyb3rward0g/helk-spark-worker:2.3.1-a                 "./spark-worker-entr…"   2 hours ago         Up 2 hours          0.0.0.0:8082->8082/tcp                       helk-spark-worker2
a014305770cd        docker.elastic.co/kibana/kibana:6.3.2                 "/usr/share/kibana/s…"   2 hours ago         Up 2 hours          5601/tcp                                     helk-kibana
17ce203be82c        cyb3rward0g/helk-spark-master:2.3.1-a                 "./spark-master-entr…"   2 hours ago         Up 2 hours          7077/tcp, 0.0.0.0:8080->8080/tcp             helk-spark-master
f07879e3ddbc        docker.elastic.co/logstash/logstash:6.3.2             "/usr/local/bin/dock…"   2 hours ago         Up 2 hours          5044/tcp, 9600/tcp                           helk-logstash
cc49b5bf4d17        docker.elastic.co/elasticsearch/elasticsearch:6.3.2   "/usr/local/bin/dock…"   2 hours ago         Up 2 hours          9200/tcp, 9300/tcp                           helk-elasticsearch
```

If you want to monitor the resources being utilized (Memory, CPU, etc), you can run the following:
```
helk@HELK:~$ sudo docker stats --all
CONTAINER ID        NAME                 CPU %               MEM USAGE / LIMIT     MEM %               NET I/O             BLOCK I/O           PIDS
40520f1dac56        helk-kafka-broker    5.86%               312.2MiB / 15.67GiB   1.95%               619kB / 558kB       868kB / 24MB        71
cfb55e9403f5        helk-kafka-broker2   0.68%               318.1MiB / 15.67GiB   1.98%               1.12MB / 1.21MB     983kB / 29.3MB      74
4d727052c9d0        helk-jupyter         0.02%               52.3MiB / 15.67GiB    0.33%               1.46kB / 0B         131kB / 1.22MB      7
6676ff41687c        helk-sigma           0.00%               0B / 0B               0.00%               0B / 0B             0B / 0B             0
ba858cbef600        helk-zookeeper       0.06%               60.42MiB / 15.67GiB   0.38%               183kB / 136kB       344kB / 1.03MB      28
cdb16480a8fb        helk-nginx           0.02%               3.922MiB / 15.67GiB   0.02%               14.3MB / 14.2MB     5.59MB / 8.19kB     7
e882198552e6        helk-spark-worker    0.06%               145.1MiB / 15.67GiB   0.90%               11.6kB / 76.8kB     0B / 5.05MB         32
2097a326e131        helk-spark-worker2   0.06%               139.3MiB / 15.67GiB   0.87%               11.6kB / 76.8kB     0B / 25.3MB         32
a014305770cd        helk-kibana          0.00%               194.4MiB / 15.67GiB   1.21%               16.6MB / 15.9MB     131MB / 1.3MB       12
17ce203be82c        helk-spark-master    0.05%               189.8MiB / 15.67GiB   1.18%               155kB / 20.4kB      0B / 885kB          42
f07879e3ddbc        helk-logstash        4.35%               1.259GiB / 15.67GiB   8.03%               473kB / 57.3MB      46.2MB / 21.2MB     66
cc49b5bf4d17        helk-elasticsearch   3.22%               8.669GiB / 15.67GiB   55.32%              59.4MB / 16.6MB     413MB / 127MB       77
```

You should also monitor the logs of each container while they are being initialized:

Just run the following:

```
helk@HELK:~$ sudo docker logs --follow helk-elasticsearch
OpenJDK 64-Bit Server VM warning: Option UseConcMarkSweepGC was deprecated in version 9.0 and will likely be removed in a future release.
OpenJDK 64-Bit Server VM warning: UseAVX=2 is not supported on this CPU, setting it to UseAVX=1
[2018-07-31T21:13:41,595][INFO ][o.e.n.Node               ] [helk-1] initializing ...
[2018-07-31T21:13:41,859][INFO ][o.e.e.NodeEnvironment    ] [helk-1] using [1] data paths, mounts [[/usr/share/elasticsearch/data (/dev/mapper/helk--vg-root)]], net usable_space [24.3gb], net total_space [32.8gb], types [ext4]
[2018-07-31T21:13:41,860][INFO ][o.e.e.NodeEnvironment    ] [helk-1] heap size [7.9gb], compressed ordinary object pointers [true]
[2018-07-31T21:13:41,862][INFO ][o.e.n.Node               ] [helk-1] node name [helk-1], node ID [rOX7uGt2SRKrk2W3956LPg]
[2018-07-31T21:13:41,862][INFO ][o.e.n.Node               ] [helk-1] version[6.3.2], pid[1], build[default/tar/053779d/2018-07-20T05:20:23.451332Z], OS[Linux/4.4.0-31-generic/amd64], JVM["Oracle Corporation"/OpenJDK 64-Bit Server VM/10.0.2/10.0.2+13]
[2018-07-31T21:13:41,862][INFO ][o.e.n.Node               ] [helk-1] JVM arguments [-Xms1g, -Xmx1g, -XX:+UseConcMarkSweepGC, -XX:CMSInitiatingOccupancyFraction=75, -XX:+UseCMSInitiatingOccupancyOnly, -XX:+AlwaysPreTouch, -Xss1m, -Djava.awt.headless=true, -Dfile.encoding=UTF-8, -Djna.nosys=true, -XX:-OmitStackTraceInFastThrow, -Dio.netty.noUnsafe=true, -Dio.netty.noKeySetOptimization=true, -Dio.netty.recycler.maxCapacityPerThread=0, -Dlog4j.shutdownHookEnabled=false, -Dlog4j2.disable.jmx=true, -Djava.io.tmpdir=/tmp/elasticsearch.VN8ZJpIQ, -XX:+HeapDumpOnOutOfMemoryError, -XX:HeapDumpPath=data, -XX:ErrorFile=logs/hs_err_pid%p.log, -Xlog:gc*,gc+age=trace,safepoint:file=logs/gc.log:utctime,pid,tags:filecount=32,filesize=64m, -Djava.locale.providers=COMPAT, -XX:UseAVX=2, -Des.cgroups.hierarchy.override=/, -Xms8g, -Xmx8g, -Des.path.home=/usr/share/elasticsearch, -Des.path.conf=/usr/share/elasticsearch/config, -Des.distribution.flavor=default, -Des.distribution.type=tar]
[2018-07-31T21:13:50,236][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [aggs-matrix-stats]
[2018-07-31T21:13:50,237][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [analysis-common]
[2018-07-31T21:13:50,237][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [ingest-common]
[2018-07-31T21:13:50,237][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [lang-expression]
[2018-07-31T21:13:50,238][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [lang-mustache]
[2018-07-31T21:13:50,238][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [lang-painless]
[2018-07-31T21:13:50,238][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [mapper-extras]
[2018-07-31T21:13:50,238][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [parent-join]
[2018-07-31T21:13:50,238][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [percolator]
[2018-07-31T21:13:50,239][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [rank-eval]
[2018-07-31T21:13:50,239][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [reindex]
[2018-07-31T21:13:50,239][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [repository-url]
[2018-07-31T21:13:50,239][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [transport-netty4]
[2018-07-31T21:13:50,239][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [tribe]
[2018-07-31T21:13:50,240][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [x-pack-core]
[2018-07-31T21:13:50,240][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [x-pack-deprecation]
[2018-07-31T21:13:50,240][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [x-pack-graph]
[2018-07-31T21:13:50,240][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [x-pack-logstash]
[2018-07-31T21:13:50,241][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [x-pack-ml]
[2018-07-31T21:13:50,241][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [x-pack-monitoring]
[2018-07-31T21:13:50,241][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [x-pack-rollup]
[2018-07-31T21:13:50,241][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [x-pack-security]
[2018-07-31T21:13:50,242][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [x-pack-sql]
[2018-07-31T21:13:50,242][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [x-pack-upgrade]
[2018-07-31T21:13:50,242][INFO ][o.e.p.PluginsService     ] [helk-1] loaded module [x-pack-watcher]
[2018-07-31T21:13:50,243][INFO ][o.e.p.PluginsService     ] [helk-1] loaded plugin [ingest-geoip]
[2018-07-31T21:13:50,243][INFO ][o.e.p.PluginsService     ] [helk-1] loaded plugin [ingest-user-agent]
[2018-07-31T21:13:57,755][INFO ][o.e.x.m.j.p.l.CppLogMessageHandler] [controller/92] [Main.cc@109] controller (64 bit): Version 6.3.2 (Build 903094f295d249) Copyright (c) 2018 Elasticsearch BV
[2018-07-31T21:13:59,114][INFO ][o.e.d.DiscoveryModule    ] [helk-1] using discovery type [single-node]
[2018-07-31T21:14:00,510][INFO ][o.e.n.Node               ] [helk-1] initialized
[2018-07-31T21:14:00,519][INFO ][o.e.n.Node               ] [helk-1] starting ...
[2018-07-31T21:14:00,987][INFO ][o.e.t.TransportService   ] [helk-1] publish_address {172.18.0.2:9300}, bound_addresses {0.0.0.0:9300}
[2018-07-31T21:14:01,195][INFO ][o.e.h.n.Netty4HttpServerTransport] [helk-1] publish_address {172.18.0.2:9200}, bound_addresses {0.0.0.0:9200}
[2018-07-31T21:14:01,195][INFO ][o.e.n.Node               ] [helk-1] started
[2018-07-31T21:14:01,887][INFO ][o.e.g.GatewayService     ] [helk-1] recovered [0] indices into cluster_state
[2018-07-31T21:14:02,492][INFO ][o.e.c.m.MetaDataIndexTemplateService] [helk-1] adding template [.triggered_watches] for index patterns [.triggered_watches*]
[2018-07-31T21:14:02,631][INFO ][o.e.c.m.MetaDataIndexTemplateService] [helk-1] adding template [.watches] for index patterns [.watches*]
[2018-07-31T21:14:02,847][INFO ][o.e.c.m.MetaDataIndexTemplateService] [helk-1] adding template [.watch-history-7] for index patterns [.watcher-history-7*]
[2018-07-31T21:14:03,086][INFO ][o.e.c.m.MetaDataIndexTemplateService] [helk-1] adding template [.monitoring-logstash] for index patterns [.monitoring-logstash-6-*]
[2018-07-31T21:14:03,230][INFO ][o.e.c.m.MetaDataIndexTemplateService] [helk-1] adding template [.monitoring-es] for index patterns [.monitoring-es-6-*]
[2018-07-31T21:14:03,480][INFO ][o.e.c.m.MetaDataIndexTemplateService] [helk-1] adding template [.monitoring-alerts] for index patterns [.monitoring-alerts-6]
[2018-07-31T21:14:03,564][INFO ][o.e.c.m.MetaDataIndexTemplateService] [helk-1] adding template [.monitoring-beats] for index patterns [.monitoring-beats-6-*]
[2018-07-31T21:14:03,609][INFO ][o.e.c.m.MetaDataIndexTemplateService] [helk-1] adding template [.monitoring-kibana] for index patterns [.monitoring-kibana-6-*]
[2018-07-31T21:14:03,728][INFO ][o.e.l.LicenseService     ] [helk-1] license [47473359-1042-42ef-addb-b3054998f7a2] mode [basic] - valid
..
....
```

All you need to do now for the other ones is just replace helk-elasticsearch with the specific containers name:
```
sudo docker logs --follow <container name>
```

Remember that you can also access your docker images by running the following commands:
```
sudo docker exec -ti helk-elasticsearch bash
root@7a9d6443a4bf:/opt/helk/scripts# 
```

# Final Details
Once your HELK installation ends, you will be presented with information that you will need to access the HELK and all its other components. 

You will get the following information:

```
***********************************************************************************
** [HELK-INSTALLATION-INFO] YOUR HELK IS READY                                   **
** [HELK-INSTALLATION-INFO] USE THE FOLLOWING SETTINGS TO INTERACT WITH THE HELK **
***********************************************************************************
 
HELK KIBANA URL: https://192.168.1.208
HELK KIBANA USER: helk
HELK KIBANA PASSWORD: hunting
HELK JUPYTERHUB URL: https://192.168.1.208/jupyter
HELK JUPYTERHUB USER:PWD : hunter1:hunter1P@ssw0rd!
HELK JUPYTERHUB USER:PWD : hunter2:hunter2P@ssw0rd!
HELK SPARK MASTER UI: http://192.168.1.208:8080
 
IT IS HUNTING SEASON!!!!!
```
| Type | Description |
|--------|---------|
| HELK KIBANA URL | URL to access the Kibana server. You will need to copy that and paste it in your browser to access Kibana. Make sure you use **https** since Kibana is running behind NGINX via port 443 with a self-signed certificate|
| HELK KIBANA USER & PASSWORD | Credentials used to access Kibana |
| HELK JUPYTERHUB URL | URL to access the JupyterHub server. You will need to copy that and paste it in your browser to access JupyterHub. Make sure you use **https** since JupyterHub is running behind NGINX via port 443 with a self-signed certificate |
| HELK JUPYTERHUB USER:PWD | Credentials of the two users that were created during the HELK install to access JupyterHub. This allows you to have two users running queries and their own notebooks on separate Jupyter Lab environments |
| HELK SPARK MASTER UI | URL to access the Spark Master server (Spark Standalone). You will need to copy that and paste it in your browser to access Spark Master. That server manages the Spark Workers used during execution of code by Jupyter Notebooks. Spark Master acts as a proxy to Spark Workers and applications running |


# Access HELK Web Interface
Open your preferred browser, go to your HELK's IP address, and enter the HELK credentials **(helk:hunting)**. By default, you will be presented by the Kibana's Home page. Once there, you could explore the different features that Kibana provides. I personally like to check the **Index Patterns** first and then **Discovery**

[[https://github.com/Cyb3rWard0g/HELK/raw/master/resources/images/KIBANA-Home.png]]

[[https://github.com/Cyb3rWard0g/HELK/raw/master/resources/images/KIBANA-IndexPatterns.png]]

[[https://github.com/Cyb3rWard0g/HELK/raw/master/resources/images/KIBANA-Discovery.png]]

# Access JupyterHub Interface
HELK now comes with a JupyterHub server which is a multi-user server for Jupyter notebooks. In the back-end, it spawns Jupyter Lab sessions for the two users that were created in the JupyterHub server.

Use the HELK JUPYTERHUB URL and you will get the following prompt

[[https://github.com/Cyb3rWard0g/HELK/raw/master/resources/images/JUPYTERHUB-Login.png]]
 
JupyterHub will create a Jupyter Lab environment for the specific user and position the session on the notebooks folder where the user will see the only folder that can be accessed. In the example below, hunter2 can only see hunter2 folder.

[[https://github.com/Cyb3rWard0g/HELK/raw/master/resources/images/JUPYTERHUB-hunter2.png]]

Once you click on hunterX>notebooks, you will see two folder **Basic** and **Trial. Depending on what Elastic subscription you use, you will use the notebooks of a specific folder. Notebooks in the trial folder have Elastic's password hardcoded to be able to interact with the Elasticsearch DB.

[[https://github.com/Cyb3rWard0g/HELK/raw/master/resources/images/JUPYTERHUB-basic-trial.png]]

Pick one of the notebooks and **make sure you select the Pyspark_Python3 Kernel** to start running Apache Spark applications (Remember that this is when Apache Spark applications will be ready to be used. I had questions about when Spark applications would start in the past). The Spark Master and Workers do not depend on Spark applications. Spark applications depend on the Spark Cluster. The Spark cluster gets initialized when the HELK is installed. Spark applications need to be initialized via a Jupyter notebook.

[[https://github.com/Cyb3rWard0g/HELK/raw/master/resources/images/JUPYTERHUB-Pyspark-Kernel.png]]

When you select the Pyspark-Python3 kernel and click accept, if you check your helk-jupyter container logs, you will see how Spark initializes

```
helk@helk:~$ sudo docker logs --follow helk-jupyter
Ivy Default Cache set to: /opt/helk/jupyterhub/hunter2/.ivy2/cache
The jars for the packages stored in: /opt/helk/jupyterhub/hunter2/.ivy2/jars
:: loading settings :: url = jar:file:/opt/helk/spark/jars/ivy-2.4.0.jar!/org/apache/ivy/core/settings/ivysettings.xml
graphframes#graphframes added as a dependency
org.apache.spark#spark-sql-kafka-0-10_2.11 added as a dependency
databricks#spark-sklearn added as a dependency
:: resolving dependencies :: org.apache.spark#spark-submit-parent-0485d9af-dd78-410e-96ea-f6693800257a;1.0
	confs: [default]
	found graphframes#graphframes;0.5.0-spark2.1-s_2.11 in spark-packages
	found com.typesafe.scala-logging#scala-logging-api_2.11;2.1.2 in central
	found com.typesafe.scala-logging#scala-logging-slf4j_2.11;2.1.2 in central
	found org.scala-lang#scala-reflect;2.11.0 in central
	found org.slf4j#slf4j-api;1.7.7 in central
	found org.apache.spark#spark-sql-kafka-0-10_2.11;2.3.0 in central
	found org.apache.kafka#kafka-clients;0.10.0.1 in central
	found net.jpountz.lz4#lz4;1.3.0 in central
	found org.xerial.snappy#snappy-java;1.1.2.6 in central
	found org.slf4j#slf4j-api;1.7.16 in central
	found org.spark-project.spark#unused;1.0.0 in central
	found databricks#spark-sklearn;0.2.3 in spark-packages
downloading http://dl.bintray.com/spark-packages/maven/graphframes/graphframes/0.5.0-spark2.1-s_2.11/graphframes-0.5.0-spark2.1-s_2.11.jar ...
	[SUCCESSFUL ] graphframes#graphframes;0.5.0-spark2.1-s_2.11!graphframes.jar (458ms)
downloading https://repo1.maven.org/maven2/org/apache/spark/spark-sql-kafka-0-10_2.11/2.3.0/spark-sql-kafka-0-10_2.11-2.3.0.jar ...
	[SUCCESSFUL ] org.apache.spark#spark-sql-kafka-0-10_2.11;2.3.0!spark-sql-kafka-0-10_2.11.jar (51ms)
downloading http://dl.bintray.com/spark-packages/maven/databricks/spark-sklearn/0.2.3/spark-sklearn-0.2.3.jar ...
	[SUCCESSFUL ] databricks#spark-sklearn;0.2.3!spark-sklearn.jar (421ms)
downloading https://repo1.maven.org/maven2/com/typesafe/scala-logging/scala-logging-api_2.11/2.1.2/scala-logging-api_2.11-2.1.2.jar ...
	[SUCCESSFUL ] com.typesafe.scala-logging#scala-logging-api_2.11;2.1.2!scala-logging-api_2.11.jar (12ms)
downloading https://repo1.maven.org/maven2/com/typesafe/scala-logging/scala-logging-slf4j_2.11/2.1.2/scala-logging-slf4j_2.11-2.1.2.jar ...
	[SUCCESSFUL ] com.typesafe.scala-logging#scala-logging-slf4j_2.11;2.1.2!scala-logging-slf4j_2.11.jar (20ms)
downloading https://repo1.maven.org/maven2/org/scala-lang/scala-reflect/2.11.0/scala-reflect-2.11.0.jar ...
	[SUCCESSFUL ] org.scala-lang#scala-reflect;2.11.0!scala-reflect.jar (499ms)
downloading https://repo1.maven.org/maven2/org/apache/kafka/kafka-clients/0.10.0.1/kafka-clients-0.10.0.1.jar ...
	[SUCCESSFUL ] org.apache.kafka#kafka-clients;0.10.0.1!kafka-clients.jar (101ms)
downloading https://repo1.maven.org/maven2/org/spark-project/spark/unused/1.0.0/unused-1.0.0.jar ...
	[SUCCESSFUL ] org.spark-project.spark#unused;1.0.0!unused.jar (15ms)
downloading https://repo1.maven.org/maven2/net/jpountz/lz4/lz4/1.3.0/lz4-1.3.0.jar ...
	[SUCCESSFUL ] net.jpountz.lz4#lz4;1.3.0!lz4.jar (38ms)
downloading https://repo1.maven.org/maven2/org/xerial/snappy/snappy-java/1.1.2.6/snappy-java-1.1.2.6.jar ...
	[SUCCESSFUL ] org.xerial.snappy#snappy-java;1.1.2.6!snappy-java.jar(bundle) (127ms)
downloading https://repo1.maven.org/maven2/org/slf4j/slf4j-api/1.7.16/slf4j-api-1.7.16.jar ...
	[SUCCESSFUL ] org.slf4j#slf4j-api;1.7.16!slf4j-api.jar (20ms)
:: resolution report :: resolve 4250ms :: artifacts dl 1775ms
	:: modules in use:
	com.typesafe.scala-logging#scala-logging-api_2.11;2.1.2 from central in [default]
	com.typesafe.scala-logging#scala-logging-slf4j_2.11;2.1.2 from central in [default]
	databricks#spark-sklearn;0.2.3 from spark-packages in [default]
	graphframes#graphframes;0.5.0-spark2.1-s_2.11 from spark-packages in [default]
	net.jpountz.lz4#lz4;1.3.0 from central in [default]
	org.apache.kafka#kafka-clients;0.10.0.1 from central in [default]
	org.apache.spark#spark-sql-kafka-0-10_2.11;2.3.0 from central in [default]
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
	|      default     |   12  |   12  |   12  |   1   ||   11  |   11  |
	---------------------------------------------------------------------
:: retrieving :: org.apache.spark#spark-submit-parent-0485d9af-dd78-410e-96ea-f6693800257a
	confs: [default]
	11 artifacts copied, 0 already retrieved (7136kB/28ms)
18/07/31 23:59:32 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
[I 2018-07-31 23:59:36.608 SingleUserLabApp handlers:190] Adapting to protocol v5.1 for kernel 02d756c7-9a9d-443a-bd8a-0981e8fa989e
```
You are now ready to run Spark applications via Jupyter Lab. Run the first line of the basic notebooks and you will see that the Spark application is already communicating with the Spark Master:

[[https://github.com/Cyb3rWard0g/HELK/raw/master/resources/images/JUPYTERHUB-SparkContext.png]]


I hope this document was helpful to deploy your own HELK. Let us know if you have any questions or if you think that this document can be improved. Feel free to create an **issue** for updates to this procedure. A more detailed **HOW-TO** will be developed soon to go into more details of how to use all the HELK components. 

IT IS HUNTING SEASON!!