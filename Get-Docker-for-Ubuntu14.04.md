
The current system environment
```
root@iZwz9g4x9o6sjet97h7prfZ:~# lsb_release -cs
trusty
```

```
sudo apt-get -y install \
  apt-transport-https \
  ca-certificates \
  curl \
  software-properties-common 

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository \
       "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
       $(lsb_release -cs) \
       stable"

sudo apt-get update
```

Get Docker CE

Install the latest version of Docker CE on Ubuntu:
```
sudo apt-get -y install docker-ce
```

Configure Docker to start on boot
Most current Linux distributions (RHEL, CentOS, Fedora, Ubuntu 16.04 and higher) use systemd to manage which services start when the system boots. Ubuntu 14.10 and below use upstart.
```
echo manual | sudo tee /etc/init/docker.override
```

Install Docker Compose
```
pip install urllib3==1.14
export PYTHONPATH=/usr/local/lib/python2.7/dist-packages:/usr/lib/python2.7/dist-packages

pip install docker-compose
```

Aliyun accelerator
```
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://fon8u9ig.mirror.aliyuncs.com"]
}
EOF
sudo service docker restart
```
