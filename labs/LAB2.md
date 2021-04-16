# Lab 2 - Managing collections

1. Create new own collection.
```
source py3-ans2.10/bin/activate
ansible-galaxy collection init mynamespace.mycollection
```

2. List collection folder structure.
```
tree mynamespace

$ tree mynamespace/
mynamespace/
└── mycollection
    ├── README.md
    ├── docs
    ├── galaxy.yml
    ├── plugins
    │   └── README.md
    └── roles
```
- This skeleton allows to create own collection, in similar way like roles are created.
- This step shows only capabilities of ansible-galaxy tool.

3. Check available modules and collections .
- If you need to analyze content of collections and modules, to go official Ansible website.

- Version 2.10
    - [Modules](https://docs.ansible.com/ansible/2.10/collections/index_module.html)
    - [Collections](https://docs.ansible.com/ansible/2.10/collections/index.html)

- The latest version
    - [Modules](https://docs.ansible.com/ansible/latest/collections/index_module.html)
    - [Collections](https://docs.ansible.com/ansible/latest/collections/index.html)


4. Install collection from Galaxy.
- ansible-galaxy collection install my_namespace.my_collection:version
```
# Install the latest version
ansible-galaxy collection install community.docker

# or into my dedicated folder, next check content
cd labs
ansible-galaxy collection install community.docker -p ./collections
tree -L 3 ./collections/

NOTE: Default paths are:
- ~/.ansible/collections
- global: /usr/share/ansible/collections 
    - or for customer python venv: venv_base_path/lib/python3.8/site-packages/ansible_collections
- local for playbook repo/location: current_playbook_path/collections
    - Note: For git repo user together with Ansible Tower, put in root folder: collections/requirements.yml
```

5. List installed collection.
- ansible-galaxy collection list
```
ansible-galaxy collection list

Output example:

# /home/vagrant/.ansible/collections/ansible_collections
Collection       Version
---------------- -------
community.docker 1.5.0

# /home/vagrant/py3-ans2.10/lib/python3.8/site-packages/ansible_collections
Collection                Version
------------------------- -------
amazon.aws                1.2.0  
and more ...
```

- Remove ansible package and list available collections again
```
pip uninstall ansible
ansible-galaxy collection list

Note: Collections under site-packages should be removed. Only manually installed collections should be listed.
```

6. Download and install collection from tarball file.
- ansible-galaxy collection install my_namespace-my_collection-<version>.tar.gz 
```
# Download and check content
ansible-galaxy collection download community.google
ls -l collections/

# Install collection from tarball
ansible-galaxy collection install ./collections/community-google-1.0.0.tar.gz
```

7. Install collection from Red Hat Automation Hub.
- You must have valid Red Hat subscription.
- Get your Automation Hub API token. Go to https://cloud.redhat.com/ansible/automation-hub/token/ and click Get API token from the version dropdown to copy your API token.
- Configure Red Hat Automation Hub server in the server_list option under the [galaxy] section in your ansible.cfg file.
```
# ansible.cfg
# Paths to search for collections, colon separated
# collection_paths = ~/.ansible/collections:/usr/share/ansible/collections

[galaxy]
server_list = automation_hub, my_private_hub, release_galaxy

[galaxy_server.automation_hub]
url=https://cloud.redhat.com/api/automation-hub/
auth_url=https://sso.redhat.com/auth/realms/redhat-external/protocol/openid-connect/token
token=my_ah_token

[galaxy_server.my_private_hub]
url=https://automation.my_org/
username=my_user
password=my_pass

[galaxy_server.release_galaxy]
url=https://galaxy.ansible.com/

# go to webpage and find proper certified collection 
https://cloud.redhat.com/api/automation-hub/

# download certified collection from Red Hat
ansible-galaxy collection install google.cloud
ansible-galaxy collection install ansible.tower
```

8. Verifying collections.
- ansible-galaxy collection verify my_namespace.my_collection
```
NOTE: The command is silent if no local modification is done
ansible-galaxy collection verify community.mysql

# Let's modify collection
rm .ansible/collections/ansible_collections/community/mysql/plugins/modules/mysql_db.py

# Next validate again
ansible-galaxy collection verify community.mysql

Downloading https://galaxy.ansible.com/download/community-mysql-1.3.0.tar.gz to /home/vagrant/.ansible/tmp/ansible-local-3155n7t2qwxu/tmpm239sa0e
Collection community.mysql contains modified content in the following files:
community.mysql
    plugins/modules/mysql_db.py

# Remove collection and reinstall broken collection
rm -rf .ansible/collections/ansible_collections/community/mysql
ansible-galaxy collection install community.mysql
ansible-galaxy collection verify community.mysql
```

9. Installing collections from requirements file.
- You can put requirement file in git repository and download automatically required roles and collections.
- Configuration example.
```
# requirements.yml
---
roles:
  # from galaxt
  - src: role
    name: local_role_name

  # from git repo
  - src: https://<url>/repo.git
    scm: git
    version: master
    name: local_role_name

collections:
 - src: https://galaxy.ansible.com
   name: collection_name
```
- Real example
```
cat << EOF >> requirements.yml
---
collections:
 - src: https://galaxy.ansible.com
   name: community.azure
EOF
ansible-galaxy collection install -r requirements.yml
```