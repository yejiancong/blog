
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

Install Docker Compose
```
pip install docker-compose
```

Aliyun accelerator
```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://fon8u9ig.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```
