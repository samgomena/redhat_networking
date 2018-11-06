### Ansible Networking Event 11/5 Portland, Oregon

The event website can be found [here](http://pdx-network.rhdemo.io/).

Some basic ansible commands:

```sh
ansible --version

ansible 2.7.0
  config file = /home/student36/.ansible.cfg
  configured module search path = [u'/home/student36/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python2.7/site-packages/ansible
  executable location = /usr/bin/ansible
  python version = 2.7.5 (default, Sep 12 2018, 05:31:16) [GCC 4.8.5 20150623 (Red Hat 4.8.5-36)]
```

#### Define a playbook
```sh
- name: GATHER INFORMATION FROM ROUTER                              # Top level name for the playbook
  hosts: cisco                                                      # Define the group to run playbook on
  connection: network_cli                                           # `network_cli` module will keep persistent ssh connection
  gather_facts: no                                                  # 

  tasks:                                                            # Tasks that the playbook runs on each host in the group
    - name: GATHER ROUTER FACTS
      ios_facts:                                                    # Uses the `ios_facts` module 
      register: all_output                                          # Saves all of the output from `ios_facts` to a variable called `all_output`

    - name: DISPLAY VERSION                                         
      debug:                                                        # Uses the `debug` module which is equivalent to a `print` statement
        msg: "The IOS version is: {{ ansible_net_version }}"        # `debug` takes a parameter `msg` which we define here; `msg` expands a variable with "{{ <var_name }}" 

    - name: DISPLAY SERIAL NUMBER
      debug:
        msg: "The serial number is: {{ ansible_net_serialnum }}"
    - name: DISPLAY ALL DATA
      debug:
        msg: "Here's all that data: {{ all_output }}"
```

If we save that to `gather_ios_data.yml` we can run:

```sh
ansible-playbook gather_ios_data.yml
```

and get the following output

```sh

PLAY [GATHER INFORMATION FROM ROUTER] **********************************************************************************************************************************************************************

TASK [GATHER ROUTER FACTS] *********************************************************************************************************************************************************************************
ok: [rtr4]
ok: [rtr3]
ok: [rtr1]
ok: [rtr2]

PLAY RECAP *************************************************************************************************************************************************************************************************
rtr1                       : ok=4    changed=0    unreachable=0    failed=0
rtr2                       : ok=4    changed=0    unreachable=0    failed=0
rtr3                       : ok=4    changed=0    unreachable=0    failed=0
rtr4                       : ok=4    changed=0    unreachable=0    failed=0 
```
