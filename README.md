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
"Plugin "
yum install -y epel-release
dnf config-manager --set-enabled crb
dnf update -y
yum install -y nrpe nagios-plugins-all

<img width="663" height="957" alt="image" src="https://github.com/user-attachments/assets/058ffbf1-2642-46fe-a8f0-ebf2d5b70a1e" />

"NRPE"
sudo dnf groupinstall "Development Tools" -y

sudo dnf install gcc glibc-devel make -y
cd /tmp
wget https://github.com/nagios-plugins/nagios-plugins/releases/download/release-2.4.9/nagios-plugins-2.4.9.tar.gz
tar zxvf nagios-plugins-2.4.9.tar.gz
cd nagios-plugins-2.4.9

./configure --prefix=/usr/local/nagios --with-nagios-user=nrpe --with-nagios-group=nrpe

make

vi /etc/nagios/nrpe.cfg

<img width="1920" height="1040" alt="image" src=e

"https://github.com/user-attachments/assets/9a705f3d-49fa-43c3-a125-6289616c05a4" />

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
install plugin and NRPE and also install systemd_check to check Mariadb service :

nagios doesn’t include check_systemd by default. Install manually:

 dnf install -y python3 python3-pip
 
 pip3 install nagiosplugin
 
 cd /tmp
 wget https://raw.githubusercontent.com/Josef-Friedrich/check_systemd/master/check_systemd.py -O check_systemd
 
 mv check_systemd.py check_systemd
 
 chmod +x check_systemd  ==> to executable it 
 
 ./check_systemd -u mariadb
 
<img width="1263" height="112" alt="image" src="https://github.com/user-attachments/assets/434f2842-a1ca-4098-8c2b-24674afbb530" />

## note : 

Move the plugin file "/usr/local/nagios/libexec/"to the NRPE plugin directory "/usr/lib64/nagios/plugins/"and make it executable

sudo mv check_systemd /usr/lib64/nagios/plugins/check_systemd

<img width="568" height="130" alt="image" src="https://github.com/user-attachments/assets/9c7691d2-2377-4875-bbed-676b839dce5f" />

<img width="1241" height="63" alt="image" src="https://github.com/user-attachments/assets/4af1a1c1-835e-4a5d-8ce2-1e6e2a617e46" />

###  mc01 
install plugin and NRPE and also install memcache service using check_systemd 


### rmq01 
install Plugins and NRPE with the below RabbitMQ service :

<img width="1180" height="71" alt="image" src="https://github.com/user-attachments/assets/0c39b74a-3289-488c-827e-b6c8aeef9bc1" 
  
 <img width="781" height="137" alt="image" src="https://github.com/user-attachments/assets/a224bf5b-4bb3-4eaf-898c-b66e5b337309" />
 
<img width="1442" height="97" alt="image" src="https://github.com/user-attachments/assets/a9a45845-4e32-4140-9595-5029d4cf555c" />

### Web01 
 
