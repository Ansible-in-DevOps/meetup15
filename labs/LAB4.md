# Lab 4 - Custom collection demo

1. In this lab we download from Galaxy custom collection written by David Newswanger, and next will analyze content and finally execute it.
- Visit Galaxy for details:  https://galaxy.ansible.com/newswangerd/collection_demo
- This collection contains module called real_facts which returns random facts.
- This collection contains role example, which uses mentioned above module.
- Lab scenario:
```
# Install collection from Galaxy
ansible-galaxy collection install newswangerd.collection_demo

# Quick check:
ansible localhost -m newswangerd.collection_demo.real_facts

# Analyze content
cd .ansible/collections/ansible_collections/newswangerd/collection_demo
tree
- Analyze module: plugins/modules/real_facts.py
- Analyze role: roles/factoid/tasks/main.yml

# Clone git repo with test playbook, check content
cd ~/labs
git clone https://github.com/newswangerd/collection_demo.git
view collection_demo/collections_in_playbooks.yaml

ansible-playbook  -i inventory collection_demo/collections_in_playbooks.yaml

# Playbook contains use cases already discussed in LAB3, point 1 and 2:
- Run a module from inside a collection.
- Run a module from inside a collection using the collections keyword.
- Run a role from inside of a collection.

```