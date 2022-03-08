# Cloud Intel Deployment On Single VM 

### Abstract
**The purpose of this document is to setup and configure the Cloud Intel on single virtual machine. Cloud Intel is an assessment tool that identifies your IT inventory and evaluates it to make you decide the movement of your workloads to Cloud.**

### Virtual Machine Configuration

- Centos 7 `minimal`
- 16 vCPUs `with enabled virtualization`  
- Reserved 32GB RAM and 200 GB Storage `SSD wth thick provision recommended`
- 150 GB Root partition
- 16 GB Swap partition -- Optional

### Perequisites for Deployment:

- Disable Firewall
- Disable Selinux 
- Install NTP packages
- Install Docker
- Install Docker-compose 

#### Disabling Firewall:
```bash
systemctl stop firewalld
systemctl disable firewalld 
```	  
#### Disabling SeLinux:
```bash
setenforce 0
vi /etc/selinux/config
```
- **Modify the file: SELINUX=disabled**

![image](https://user-images.githubusercontent.com/95343388/153411761-6977772e-5581-42fb-ab2c-75cb63bad5d3.png)


#### Installing NTP:
```bash
yum install ntp -y
systemctl start ntpd  &&  systemctl enable  ntpd
systemctl status ntpd
ntpdate -q  0.ro.pool.ntp.org  1.ro.pool.ntp.org
```	
#### Install Docker:
```bash    
sudo yum install -y yum-utils
sudo yum-config-manager \
         --add-repo \
         https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install -y docker-ce docker-ce-cli containerd.io
systemctl start docker && systemctl enable docker
```	
#### Install Docker Compose:
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose version
```
#### Running Docker Compose YAML:
```bash
cd /root
yum install -y git
git clone https://github.com/Click2Cloud/CloudIntel_Setup_Guide.git
```
```bash
cd /root/CloudIntel_Setup_Guide/Client/
mkdir -p /etc/postgresql 
cp postgresql.conf  /etc/postgresql/

```	
- **Make the changes to replace Host IP in yaml file by using VI editor**
```bash
vi docker-compose.yaml
```
- **Enter ':' and paste the below command in front of it**
```bash
%s/192.168.2.164/XXX.XXX.X.X/g
```
- **Replace 'XXX.XXX.X.X' with your VM's IP**
- **eg: ":%s/192.168.2.164/192.168.1.137/g"** 
   ![image](https://user-images.githubusercontent.com/65389762/157243257-b8161884-c1ec-48df-bbd2-6342b95e17cc.png)
- **eg: Exit by pressing 'Esc' and :wq!** 

- **Pull the image from Click2cloud's private registry** 
```bash
docker login harbornew.thecloudsbrain.com
```
`Username:`
```bash
robotci_client+click2cloud
```
`Password:`
```bash
dI9LM3XMBumf6w2dyUOJ8TeIIYVQwEfv
```
### Run Docker Compose YAML:

```bash
docker-compose up -d
```
- **View the running container by:(Optional)** 
```bash
docker ps
```
- **Access Cloud Intel by: http://XXX.XXX.X.X:30282**
- **eg: http://192.168.1.137:30282**

- **NOTE: The container might take around 10 mins to initialize-- Please wait**

- **Once Cloud Intel is accessed you will see the below screen**

![image](https://user-images.githubusercontent.com/65389762/157237191-a56020a5-4665-429e-a162-5ccb58ffc570.png)

- **Click on Register and create your user**

![image](https://user-images.githubusercontent.com/65389762/157238420-766fd916-0ec3-405a-ab64-9f2d27fe4cd8.png)

- **Once the user is created, login with the admin credentials for activating the user**
- **Username - 'admin@click2cloud-team.com'**
- **Password - 'ROOT#123'**
- **Activate the user as below image:**
![image](https://user-images.githubusercontent.com/65389762/157239187-5b14e8b8-e58d-4794-8938-0266fb9a9813.png)

- **Logout the admin account and login with your account's credentials**
- **The below screen is the dashboard view of a CloudIntel user:**
- ![image](https://user-images.githubusercontent.com/65389762/157241612-231aafb3-8b29-4a37-ab98-cb1993388fdb.png)
