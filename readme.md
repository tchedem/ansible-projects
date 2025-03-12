### Purpose of the repo 

My initial goal was to present the tool try it out with different projects that I found in [Jeff Geerling portfolio](https://github.com/geerlingguy/ansible-for-devops). But my goal changed and  unfortunatly, all of his project won't necessary be usefull for me! 

So, I learn main concept and I'm using the tool on daily basis at my work place in different automation projects we have.

But if I found/think something will be valuable to share with you, I will do it in this repo. Just take time to read the projects-flow file to explore them.


### What is Ansible ?

More simply, Ansible is an IT automation tool writtin in Python.

The project is owned by RedHat and is used for orchestration tasks like environment provisioning, system configuration managments and application deployments. A real `Swiss army kniff`.

> Before you dive deep into the tool, I recommand you to get in touch with system management/administration. Then you will trully see the strengh and the different possibily the tool have to offer!

##### How it work ?

It work in `push` mode and can be run in `ad-hoc` command mode or in `playbook` mode.

> push mode: Doesn't require any agent to be installed on `Managed Nodes` and directly run the command (task) to each machine over SSH;

> Ad-hoc mode: You can run one-time tasks quickly directly from the CLI;

> Paybook mode: You can organize your tasks in collection of configuration rules (ussually written in YAML) and use Ansible to run them to manage your server and make sure that each machine in your infra reach a certain level of state (in term of configuration);

##### In what environment can I use it ?

It is switable for any kind of OS 
RHEL/CentOS, Debian, OpenSuse, Fedora, Windows, MAC.


### Ressources : 

- The official documentation of ansible (User/Dev/Sec/Sys/Ops/... friendly. It is well structured with example and will be so much helpfull if you take time to read it!) - [Click](https://docs.ansible.com/)

- Xavki videos (Great introduction resource) - [Click](https://www.youtube.com/watch?v=Cisg9bLhLkk)

- Openclassroom tutorial (Great introduction resource too. And they explain very well the concept of virtual environments) - [Click](https://openclassrooms.com/fr/courses/2035796-utilisez-ansible-pour-automatiser-vos-taches-de-configuration)

- The book of Jeff Geerling (This is my fav.♥️ But try out the tool before comming here!) - [Click](https://leanpub.com/ansible-for-devops/c/CTVMPCbEeXd3)
  >> The book is free but support him. He does a very great and advanced job



Happy learning ! 🚀
And Big thanks to the community.