
# How to develop Ansible filter plugins

Ansible filters works pretty much like jinja2 filters, basically the plugin receives the 
input(data and parameters) and return the output:

```
{{ data | filter_plugin_name(paras) }}
```

Ansible ships a bunch of builtin filter plugins. But sometimes we desire to have custom filter plugins to 
satisfy our user cases. Ansible as an excellent configuration managment framework gives us the ability to 
design the custom plugins in your playbooks, as well as the modules.

[Official Ansible filter plugins documents](https://docs.ansible.com/ansible/devel/playbooks_filters.html)
[Examples of Ansible filter plugins](https://github.com/ansible/ansible/tree/devel/lib/ansible/plugins/filter)

The following example guides you to implement a very simple ansible filter plugin which provides the ability to get
the attribute of a dictionary, even nested.

## Create a test_inventory file including a nested dictionary

```
[all:vars]
test_par={'a': {'b': {'c': '5'}}}
```

## Create a test_playbook to get a netsted attribute by filter plugin `get_attr`

```
- hosts: localhost
  gather_facts: no
  tasks:
    - debug: 
        msg: "{{ test_par | get_attr('a.b.c') }}"
```

## Create a python script `filter_plugins/oaa.py`

Note: 
- `oaa.py` needs to placed under the directory `filter_plugins` that could be retrieved by Ansible
- The script name is arbitrary as long as it doesn't conflicts with the Ansible builtin name

```
#!/usr/bin/python
# -*- coding: utf-8 -*-
# pylint: disable=too-many-lines

from ansible import errors
import ast

def get_attr(data, attribute=None):
    """ This looks up dictionary attributes of the form a.b.c and returns
        the value.

        If the key isn't present, None is returned.
        Ex: data = {'a': {'b': {'c': 5}}}
            attribute = "a.b.c"
            returns 5
    """
    if not attribute:
        raise errors.AnsibleFilterError("|failed expects attribute to be set")

    if not isinstance(data, dict):
        raise errors.AnsibleFilterError("|failed expects data to be a dict")

    ptr = data
    for attr in attribute.split('.'):
        if attr in ptr:
            ptr = ptr[attr]
        else:
            ptr = None
            break

    return ptr

# Create FilterModule class and filters function like this
class FilterModule(object):
    """ Custom ansible filter mapping """

    filter_map = {
        'get_attr': get_attr,
    }

    def filters(self):
        return self.filter_map

```

## Run the playbook to validate the plugin

```
$ ansible-playbook -i test_inventory test_playbook.yml -v
Using /etc/ansible/ansible.cfg as config file
 [WARNING]: provided hosts list is empty, only localhost is available


PLAY [localhost] ************************************************************************************************************************************************************

TASK [debug] ****************************************************************************************************************************************************************
ok: [localhost] => {
    "changed": false
}

MSG:

5


PLAY RECAP ******************************************************************************************************************************************************************
localhost                  : ok=1    changed=0    unreachable=0    failed=0   

```
