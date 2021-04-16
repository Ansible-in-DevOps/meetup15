# Lab 3 - Using collections

1. Using collections in playbook.
- Reference a collection content by its fully qualified collection name (FQCN).
- Using FQCN is recommended option.
```
- hosts: all
  tasks:
   - name: Execute module from collection
     my_namespace.my_collection.module:
      option1: value
      option2: value
       ...
```

2. Importing role from collection.
- Import role in playbook.
```
- hosts: all
  tasks:
   - name: Import role from collection
     import_role:
      name: my_namespace.my_collection.role_name
```
or simply
```
- hosts: all
  collections:
    - my_namespace.my_collection

  tasks:
    - name: Import role from collection
      import_role:
       name: role_name
```

3. First play example.
- Put FQCN as module name.
- Simple builtin modules usage example.
- List of builtin content: https://docs.ansible.com/ansible/latest/collections/ansible/builtin/index.html .
```
# Inventory file:
[lab]
managed_server  ansible_connection=local

# first_play.yml
- hosts: all
  gather_facts: no

  tasks:
   - name: Simple ping
     ansible.builtin.ping:

   - name: Check ssh service
     ansible.builtin.service:
      name: ssh
      state: started

# run
ansible-playbook -i inventory first_play.yml
```

4. More examples in [Lab 4](LAB4.md).

