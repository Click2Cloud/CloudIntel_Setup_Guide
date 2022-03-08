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
   ![image](https://user-images.githubusercontent.com/65389762/157035625-59c0b119-987a-493c-99ea-5f00d1391b01.png)
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
