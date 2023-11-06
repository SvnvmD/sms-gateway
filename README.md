# sms-gateway
If you are trying to implement an SMS gateway for your applications in Bhutan, the defacto SMS gateway application you may use is playSMS. 

Here I am going to show how to install playSMS and connect your playSMS to BMobile or TashiCell SMSC gateways. 

## Installation of playSMS
playSMS runs on Ubuntu or Debian-based Operating Systems. So, you will need your application server running on these OSs. You may create a separate droplet/ec2 instance/virtual machine for installing your SMS gateway.

There are several ways in which you can install playSMS. You can find [Installation Guide here](https://playsms.org/).

I am going to share how to install playSMS in a docker image (which is easier to install and manage).

### Installation of playSMS as a docker image
Click here for [Docker installation guide](https://docs.docker.com/engine/install/ubuntu/)

Install docker locally, and run this to download playSMS 1.4.3 docker container: 

``docker pull playsms/playsms:1.4.3``


And then run this to get playSMS up and running:

``docker run -d -p 80:80 playsms/playsms``


Access playSMS web admin by browsing localhost or your server’s IP address, or domain if you have mapped it correctly.


Verify if playSMS is installed correctly by running playsmsd check on the inside of the container, run this:\

``docker exec CONTAINER_ID playsmsd check``

## Testing playSMS with Kannel
How to install Kannel for playSMS in Ubuntu: 

``sudo apt install kannel kannel-extras``
 
``sudo mkdir -p /var/log/kannel /var/run/kannel /var/spool/kannel/store``
 
``sudo chown -R kannel /var/log/kannel /var/run/kannel /var/spool/kannel/store``
 
``sudo usermod -a -G dialout kannel``
 

Edit /etc/default/kannel to activate smsbox, smsbox is part of Kannel, the daemon that handles SMS in Kannel: 

``sudo sed -i 's/#START_SMSBOX/START_SMSBOX/' /etc/default/kannel``

I don’t need wapbox so I disable it: 

``sudo sed -i 's/START_WAPBOX/#START_WAPBOX/' /etc/default/kannel``

Backup original kannel.conf: 

``sudo cp /etc/kannel/kannel.conf /etc/kannel/kannel.conf.dist``

Then edit ``kannel.conf`` using the SMSC details provided by [BMobile](https://www.bt.bt/) and/or [TashiCell](https://www.tashicell.com/other-services/smsc-gateway) upon subscription. 


## Restart Kannel Daemon:
Check status:

``/etc/init.d/kannel status``				OR 

``systemctl kannel status``


 Stop:
 
``/etc/init.d/kannel stop`` 					OR

``systemctl kannel stop``
 
Start:

``/etc/init.d/kannel start``					OR
``systemctl kannel start``

## Manage gateway and SMSC
Go to Settings -> Manage gateway and SMSC and click Manage (the folder icon) on Gateway Kannel. Fill in with data form ``kannel.conf``

## For Further Assistance
For Further Assistance contact me via my [blog](https://bhutanio.com/)


