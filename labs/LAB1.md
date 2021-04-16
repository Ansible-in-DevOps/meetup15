# Lab 1 - Environment setup

## Ansible control node setup
1. Ubuntu/Debian
```
sudo apt-get update
sudo apt-get install -y python3-pip python3-virtualenv 
```

2. Centos/Fedora
```
sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
sudo dnf install -y python3-pip python3-virtualenv 
```

3. Python venv setup for Ansible
```
# Python venv for Asnible 2.9
virtualenv -p python3 py3-ans2.9
source py3-ans2.9/bin/activate
pip install ansible==2.9
ansible --version
pip list | grep -i ansible
deactivate

# Python venv for Asnible 2.10
virtualenv -p python3 py3-ans2.10
source py3-ans2.10/bin/activate
pip install ansible==2.10   # it will install ansible-base too
ansible --version
pip list | grep -i ansible
deactivate
```