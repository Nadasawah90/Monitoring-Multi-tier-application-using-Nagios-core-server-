# Monitoring-Multi-tier-application-using-Nagios-core-server-

 ## Prerequistes :
 create new VM with the below spec : 
 
RAM : 4 GB 

CPU : 2 core 

Storage : 30 GB for testing 

OS : Centoes 9 

## Note 

on server nagios  ==> install nagios Core & check_nrpe plugin ==> is a client-side plugin used by Nagios server which connects to the NRPE daemon on the client.

on client         ==>  install NRPE daemon & nagios Plugins (check_tcp, check_http ,check_nrpe plugin ...etc) 

## Steps intstallation : 

### 1- on server nagios : 

1- install nagios Core 

   cd /tmp
  
   wget https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.5.10.tar.gz
  
  tar zxvf nagios-4.5.10.tar.gz
  
  cd nagios-4.5.10
  
   sudo yum install -y httpd php php-cli gcc glibc glibc-common gd gd-devel   net-snmp openssl-devel wget unzip make automake gcc-c++ perl
   
  sudo useradd nagios
  
  sudo groupadd nagcmd
  
  sudo usermod -a -G nagcmd nagios
  
   sudo usermod -a -G nagcmd apache  
  
  ./configure --with-command-group=nagcmd
  
  make all
  
  sudo make install
  
  sudo make install-init
  
  sudo make install-config
  
  sudo make install-commandmode
  
  sudo make install-webconf
  
  sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
  
 systemctl restart httpd
  
systemctl status nagios
  
systemctl enable  nagios
  
systemctl start   nagios
  
2- install check_nrpe plugin  on Nagios core to can connect to the NRPE daemon on the client.

 yum install -y epel-release &&  yum install -y nagios-plugins-nrpe
 
 <img width="1920" height="1030" alt="image" src="https://github.com/user-attachments/assets/9338d4a3-2fda-4b6a-a655-a48f6fece394" />
 
the Directory path  is  ==> /usr/lib64/nagios/plugins/check_nrpe 

3- we will create hosts.cfg & Services.cfg and add the commands check on commands.cfg files 

#### Hosts :  vi /usr/local/nagios/etc/objects/hosts.cfg

 <img width="1920" height="1030" alt="image" src="https://github.com/user-attachments/assets/80e9000e-2a82-47bc-961d-572d472320fb" />
 
 <img width="1920" height="1030" alt="image" src="https://github.com/user-attachments/assets/fc19af23-eb32-4f68-9149-a61ac45ed3e5" />

#### commands :  vi /usr/local/nagios/etc/objects/commands.cfg
 
define command{

    command_name    check_nrpe
    
    command_line    /usr/lib64/nagios/plugins/check_nrpe -H $HOSTADDRESS$ -c $ARG1$
}

#### services : vi /usr/local/nagios/etc/objects/services.cfg

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6ba9b96d-c499-4c4d-9f19-e6127e0aac2d" />

#### finally add Services and hosts.cfg in lines in the directory : 

/usr/local/nagios/etc/nagios.cfg

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0314511b-47c6-42e8-b0eb-7f73974443d0" />

confirm every thing working well without error :

<img width="1920" height="1030" alt="image" src="https://github.com/user-attachments/assets/cd920ebb-b88f-4f05-8f9b-acbf3b7b8348" />

<img width="1920" height="1030" alt="image" src="https://github.com/user-attachments/assets/a8df9346-be1b-49e6-87cd-b2b4b74adc11" />

## 2- on client servers : 

###  app01 

1- install NRPE & Plugin 
yum install -y epel-release
yum install -y nrpe nagios-plugins-all
vi /etc/nagios/nrpe.cfg
<img width="1920" height="1040" alt="image" src=e
"https://github.com/user-attachments/assets/9a705f3d-49fa-43c3-a125-6289616c05a4" />

make sure 

sudo dnf groupinstall "Development Tools" -y

sudo dnf install gcc glibc-devel make -y

cd /tmp
wget https://github.com/nagios-plugins/nagios-plugins/releases/download/release-2.4.9/nagios-plugins-2.4.9.tar.gz
tar zxvf nagios-plugins-2.4.9.tar.gz
cd nagios-plugins-2.4.9
cd /tmp/nagios-plugins-2.4.9

./configure --prefix=/usr/local/nagios --with-nagios-user=nrpe --with-nagios-group=nrpe

make

sudo make install

sudo systemctl restart nrpe

make sure from path directory of Check_nrpe path ==>  /usr/local/nagios/libexec/

after that add the commands on the file  :

vi /etc/nagios/nrpe.cfg

<img width="1920" height="1030" alt="image" src="https://github.com/user-attachments/assets/138db057-75b2-4837-beca-143772a0846d" />
make sure fro path directory of plugin 

<img width="1180" height="71" alt="image" src="https://github.com/user-attachments/assets/0c39b74a-3289-488c-827e-b6c8aeef9bc1" />
sudo -u nrpe /usr/local/nagios/libexec/check_http -p 80

sudo -u nrpe /usr/local/nagios/libexec/check_tcp -p 22

sudo -u nrpe /usr/local/nagios/libexec/check_ping -H 127.0.0.1

### DB01 
install plugin and NRPE 


### Web01 

###  mc01 

### rmq01 
install Plugins and NRPE with the below RabbitMQ service :

<img width="1180" height="71" alt="image" src="https://github.com/user-attachments/assets/0c39b74a-3289-488c-827e-b6c8aeef9bc1" 
  
 <img width="781" height="137" alt="image" src="https://github.com/user-attachments/assets/a224bf5b-4bb3-4eaf-898c-b66e5b337309" />
 
<img width="1442" height="97" alt="image" src="https://github.com/user-attachments/assets/a9a45845-4e32-4140-9595-5029d4cf555c" />


 
