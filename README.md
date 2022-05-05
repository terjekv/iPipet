# Installation

The software uses start-stop-daemon to manage its daemon, so it is easier to install this on a Debian based system. The following has been tested on Debian 11.

## System setup

Create system user 'ipipet':
```
sudo adduser --system --group --no-create-home ipipet
```

Create directories for ipipet:
```
sudo su - 
mkdir /var/{run,log}/ipipet
chown ipipet:ipipet /var/{run,log}/ipipet
chmod 0755 /var/{run,log}/ipipet
```


## Install git and clone repo

```
sudo su -
apt install git
cd ~
git clone https://github.com/terjekv/iPipet
```

## Python setup

To create virtual environment and install dependencies, we first need to install the apt package for virtualenv:

```
sudo apt install virtualenv
```

After this, create an environment, activate it, and populate it:

```
sudo su -
virtualenv ~/python-env/iPipet
source ~/python-env/iPipet/bin/activate
pip install -r ~/iPipet/requirements.txt
```

## Start the service

Note: In `pooling.sh` you will want to edit the `BIND_IP` option to `0.0.0.0` if you wish to make the service available outside of localhost, and you may wish to use a port other than `5105`. However, one option is to use a load balancer in front of the service, like ngnix, and route that to `127.0.0.1:5105`.

Run the flask app with gunicorn as user ipipet:
```
sudo su -
source ~/python-env/iPipet/bin/activate
cd ~/iPipet
./pooling.sh start
```
