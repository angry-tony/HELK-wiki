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
In order to make the installation of the HELK easy for everyone, the project comes with a menu where all you have to do is choose how you want to install the HELK. Just pick the option that you want to use, and it will do it all for you.
* **Option 1:** Pulls and runs the latest HELK Docker image from [Cyb3rWard0g Docker Hub](https://hub.docker.com/r/cyb3rward0g/helk/).
* **Option 2:** Builds and runs the HELK from its local Dockerfile.
* **Option 3:** Installs and runs the HELK from its local Bash Script.
```
**********************************************
**           HELK - M E N U                 **
**                                          **
** Author: Roberto Rodriguez (@Cyb3rWard0g) **
** HELK build version: 0.9 (Alpha)          **
** HELK ELK version: 6.1.3                  **
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
** HELK ELK version: 6.1.3                  **
** License: BSD 3-Clause                    **
**********************************************
 
1. Pull the latest HELK image from DockerHub
2. Build the HELK image from local Dockerfile
3. Install the HELK from local bash script
4. Exit
 
[HELK-INSTALLATION-INFO] Enter choice [ 1 - 4] 2
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

If you are using Docker to deploy your HELK, remember that the ELK logs are available inside of your docker image. You can access your docker image by running the following commands:
```
sudo docker exec -ti helk bash
root@7a9d6443a4bf:/opt/helk/scripts# 
```
I also recommend to monitor your Docker logs (available on the host and not inside of the Docker image). This will allow you to see the execution of the [helk_docker_entrypoint.sh](https://github.com/Cyb3rWard0g/HELK/blob/master/scripts/helk_docker_entrypoint.sh) bash script once the the docker image is run during the installation. Run the following commands:
```
sudo docker logs helk
```
# Final Details
Once your HELK installation ends, you will be presented with information that you will need to access the HELK and all its features. If you install the HELK via Docker, you will have the Jupyter Notebook server running and Spark ready to be used. You will have the following message showing in your main screen:
```
***********************************************************************************
** [HELK-INSTALLATION-INFO] YOUR HELK IS READY                                   **
** [HELK-INSTALLATION-INFO] USE THE FOLLOWING SETTINGS TO INTERACT WITH THE HELK **
***********************************************************************************
 
HELK KIBANA URL: http://192.168.64.131
HELK KIBANA USER: helk
HELK KIBANA PASSWORD: hunting
HELK JUPYTER CURRENT TOKEN: 8dc4ea6610a7b893055120f4812a0770a60805ba6fa8dd69
HELK SPARK UI: http://192.168.64.131:4040
HELK JUPYTER NOTEBOOK URI: http://192.168.64.131:8880
HELK DOCKER BASH ACCESS: sudo docker exec -ti helk bash
 
IT IS HUNTING SEASON!!!!!
```
If you are installing the HELK via its own bash script, you will get the same message, but with a different **HELK JUPYTER CURRENT TOKEN** message and without the **HELK DOCKER BASH ACCESS** line of course.
```
HELK JUPYTER CURRENT TOKEN: First, run the following: source ~/.bashrc && pyspark
```
It is important to remember that you can still run your HELK with all the ELK functionality even if you do not run those commands. Those commands are to start the Jupyter Server and Spark functionalities. After that, you will get your **JUPYTER TOKEN** and you will be able to access the Jupyter Notebook web interface.