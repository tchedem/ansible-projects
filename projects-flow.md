There is a lot of paradigms with Ansible.

My advices will be to learn datastructure with YAML 

Learn basic concept like 

- use ansible in cli mode for basic operations (Ad-hoc mode)
- Simple playbook writing 
- How to install virtual environment
  - It will help you to havev multiple version of ansible on your Control Node (CN)
- Roles implementations
  - Variables
    - Environemnt variable
    - Variable precedence
  - Loops 
  - Conditional executing 

With this basics, you are good enough to build great project with the tool.

---

### 00-Starter kit

>> This is a scafolding of a simple Ansible project.

You can use it to start your automation projects



### Build infrastructure with Vagrant for testing p19
Code: `build_infrastructure_with_vagrant_for_testing`

Some error notes [p24]
```bash
export ANSIBLE_INVENTORY=hosts.ini
export ANSIBLE_HOST_KEY_CHECKING=False
```


----