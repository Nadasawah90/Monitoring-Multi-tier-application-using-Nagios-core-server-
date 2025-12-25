# Monitoring-Multi-tier-application-using-Nagios-core-server-

1- create new VM with the below spec : 

RAM : 4 GB 

CPU : 2 core 
Storage : 30 GB for testing 

OS : Centoes 9 

Hint :

on server nagios : install nagios Core & check_nrpe plugin ==> is a client-side plugin used by Nagios server which connects to the NRPE daemon on the client.

on client         : install NRPE daemon & nagios Plugins (check_tcp, check_http) 

Steps intstallation : 

on server nagios : 

1- on the server to install nagios Core 

  161  cd /tmp
  
  162  wget https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.5.10.tar.gz
  
  163  tar zxvf nagios-4.5.10.tar.gz
  
  164  cd nagios-4.5.10
  
  165  sudo yum install -y httpd php php-cli gcc glibc glibc-common gd gd-devel   net-snmp openssl-devel wget unzip make automake gcc-c++ perl
  
  166  clear
  167  sudo useradd nagios
  
  168  sudo groupadd nagcmd
  
  169  sudo usermod -a -G nagcmd nagios
  
  170  sudo usermod -a -G nagcmd apache   # RHEL/CentOS
  
  172  ./configure --with-command-group=nagcmd
  
  173  make all
  
  174  sudo make install
  
  175  sudo make install-init
  
  176  sudo make install-config
  
  177  sudo make install-commandmode
  
  178  sudo make install-webconf
  
  179  sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
  
  182  systemctl restart httpd
  
  183  systemctl status nagios
  
  184  systemctl enable  nagios
  
  185  systemctl start   nagios
  
2- install check_nrpe plugin  on Nagios core to can connect to the NRPE daemon on the client.

 yum install -y epel-release &&  yum install -y nagios-plugins-nrpe
 
 <img width="1920" height="1030" alt="image" src="https://github.com/user-attachments/assets/9338d4a3-2fda-4b6a-a655-a48f6fece394" />
 
the Directory of check_nrpe is ==> /usr/lib64/nagios/plugins/check_nrpe 

3- we will create hosts.cfg & Services.cfg and add the commands check on commands.cfg files 

Hosts :  vi /usr/local/nagios/etc/objects/hosts.cfg

 <img width="1920" height="1030" alt="image" src="https://github.com/user-attachments/assets/80e9000e-2a82-47bc-961d-572d472320fb" />
 
 <img width="1920" height="1030" alt="image" src="https://github.com/user-attachments/assets/fc19af23-eb32-4f68-9149-a61ac45ed3e5" />

commands :  vi /usr/local/nagios/etc/objects/commands.cfg
 
define command{

    command_name    check_nrpe
    
    command_line    /usr/lib64/nagios/plugins/check_nrpe -H $HOSTADDRESS$ -c $ARG1$
}

services : vi /usr/local/nagios/etc/objects/services.cfg

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6ba9b96d-c499-4c4d-9f19-e6127e0aac2d" />

finally add Services and hosts.cfg in lines in the directory : 

/usr/local/nagios/etc/nagios.cfg

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0314511b-47c6-42e8-b0eb-7f73974443d0" />

confirm every thing working well without error :

<img width="1920" height="1030" alt="image" src="https://github.com/user-attachments/assets/cd920ebb-b88f-4f05-8f9b-acbf3b7b8348" />

<img width="1920" height="1030" alt="image" src="https://github.com/user-attachments/assets/a8df9346-be1b-49e6-87cd-b2b4b74adc11" />

on client servers : 

==> app01 

1- install NRPE 

2- install Plugins 

make sure 

sudo dnf groupinstall "Development Tools" -y

sudo dnf install gcc glibc-devel make -y

cd /tmp/nagios-plugins-2.4.9

./configure --prefix=/usr/local/nagios --with-nagios-user=nrpe --with-nagios-group=nrpe

make

sudo make install

sudo systemctl restart nrpe

make sure from path directory of Check_nrpe path ==>  /usr/local/nagios/libexec/

after that add the commands on the file  :

vi /etc/nagios/nrpe.cfg

<img width="1920" height="1030" alt="image" src="https://github.com/user-attachments/assets/138db057-75b2-4837-beca-143772a0846d" />

sudo -u nrpe /usr/local/nagios/libexec/check_http -p 80

sudo -u nrpe /usr/local/nagios/libexec/check_tcp -p 22

sudo -u nrpe /usr/local/nagios/libexec/check_ping -H 127.0.0.1

==> DB01 

==> Web01 

==> mc01 

==>rmq01 

 

 
