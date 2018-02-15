# Design
[[https://github.com/Cyb3rWard0g/HELK/raw/master/resources/images/ELASTICSEARCH-Design.png]]

# Settings
## HELK's Heap Size
By default, HELK calculates how much memory the host has and assigns 50% of it to it (You can change that by manually modifying the /etc/elasticsearch/jvm.options file after the installation and restarting your elasticsearch service)
```
sudo nano /etc/elasticsearch/jvm.options
sudo service elasticsearch restart
```