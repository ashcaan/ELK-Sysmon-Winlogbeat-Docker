# ELK-Sysmon-Winlogbeat-Docker
Setting up ELK on Docker and transfer sysmon logs with winlogbeat to ELK stack.

Our Enviroment: 
VM with 6 Core, 12GB Ram and 200GB SSD

**Step-1: Setting up Docker**<br>

We need to install docker and docker-compose on our server. Run following commands to update and install docker

```shell
sudo apt-get update && sudo apt-get upgrade
sudo apt-get install docker docker-compose
```

**Step-2: Downloading project**<br>
We will be using amazing work of deviantony/docker-elk

```shell
cd /root
sudo git clone https://github.com/deviantony/docker-elk
cd docker-elk
```


**Step-3: Creating containers**<br>
Befor running docker compose we need to set passwords for our environment. Edit .env file with any editor and change password for following items:

```shell
ELASTIC_PASSWORD='changeme'
LOGSTASH_INTERNAL_PASSWORD='changeme'
KIBANA_SYSTEM_PASSWORD='changeme'
```
After setting secure password on our ELK stack users run below commands to creat containers.
```shell
docker-compose up setup
docker-compose up
```
Now you should be able to login on Kibana with user elastic and your password on .env file.
```shell
http://IP:5061
User: elastic
Pass: $ELASTIC_PASSWORD
```

**Step-4: Install Sysmon and Winlogbeat**

Download below link and extract it on your Windows VM.
```shell
https://ashkan.solutions/dl/ELK.zip
```
Move "Winlogbeat" folder to "C:\Program Files"

In power shell run following commands:
```shell
cd 'C:\Program Files\Winlogbeat
PowerShell.exe -ExecutionPolicy UnRestricted -File .\install-service-winlogbeat.ps1
```
Edit "C:\Program Files\Winlogbeat\winlogbeat.yml" file and change IP and password for Elastic, Kibana and Logstash section. Now start Winlogbeat service.

Now for installing Sysmon go to it's directory and run below command:

```shell
Sysmon64.exe -accepteula -i sysmonconfig-export.xml
```


**Step-5: Chech logs on Kibana**

Now the instalation is finished and you can check logs in Kibana Web interface.