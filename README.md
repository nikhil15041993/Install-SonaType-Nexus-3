# Install-SonaType-Nexus-3

## What is Nexus Repository Manager ?

Artifact Repository: Artifact repository is a location where you can store your all artifacts which are needed for the projects.

Nexus Repository Manager: It allows developer to collect, retrieve, manage our artifacts.

Basically Nexus Repository Manager helps us to host our repositories.
For eg- “Maven Central Repository” so we can use it to retrieve all dependencies needed for a Maven build.

## Prerequisites

*Open JDK 8
*Minimum CPU’s: 4
*Ubuntu Server with User sudo privileges.
*Set User limits
*Web Browser
*Firewall/Inbound port: 22, 8081

### update the system packages

```
sudo apt-get update
```

### Install OpenJDK 8 on Ubuntu

```
sudo apt-get install openjdk-8-jdk
```

### Download Nexus Repository Manager

Execute the below commands — navigate to /opt directory by changing directory:
```
cd /opt
```
Download the SonaType Nexus on Ubuntu using wget
```
sudo wget https://download.sonatype.com/nexus/3/nexus-3.37.3-02-unix.tar.gz
```
### Install Nexus Repository 

Extract the Nexus repository setup in /opt directory
```
sudo tar -xvf nexus-3.37.3-02-unix.tar.gz 
```
Rename the extracted Nexus setup folder to nexus
```
sudo mv nexus-3.37.3-02 ./nexus
```
Give permission to nexus files and nexus directory to ```ubuntu``` user
```
sudo chown -R ubuntu:ubuntu nexus
sudo chown -R ubuntu:ubuntu sonatype-work
```
To run nexus as service at boot time, open /opt/nexus/bin/nexus.rc file, uncomment it and add ubuntu user as shown below
```
sudo nano /opt/nexus/bin/nexus.rcCopy
run_as_user="ubuntu"
```
To Increase the nexus JVM heap size, open the /opt/nexus/bin/nexus.vmoptions file, you can modify the size as shown below

```
-Xms512m
-Xmx512m
-XX:MaxDirectMemorySize=512m

-XX:LogFile=./sonatype-work/nexus3/log/jvm.log
-XX:-OmitStackTraceInFastThrow
-Djava.net.preferIPv4Stack=true
-Dkaraf.home=.
-Dkaraf.base=.
-Dkaraf.etc=etc/karaf
-Djava.util.logging.config.file=/etc/karaf/java.util.logging.properties
-Dkaraf.data=./sonatype-work/nexus3
-Dkaraf.log=./sonatype-work/nexus3/log
-Djava.io.tmpdir=./sonatype-work/nexus3/tmp

```

### Run Nexus as a service using Systemd

To run nexus as service using Systemd
```
sudo nano /etc/systemd/system/nexus.service
```
paste the below lines into it.

```
[Unit]
Description=nexus service
After=network.target
[Service]
Type=forking
LimitNOFILE=65536
User=ubuntu
Group=ubuntu
ExecStart=/opt/nexus/bin/nexus start
ExecStop=/opt/nexus/bin/nexus stop
User=ubuntu
Restart=on-abort
[Install]
WantedBy=multi-user.target
```

## Now Start Nexus

```
sudo systemctl enable nexus
sudo systemctl start nexus
sudo systemctl status nexus
```


Once Nexus is successfully installed, you can access it in the browser by
URL — http://public_dns_name:8081
