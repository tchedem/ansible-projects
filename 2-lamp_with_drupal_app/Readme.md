<<<<<<< HEAD
# Architecture of the projects

<!-- <img> -->
<!-- ![alt text](https://github.com/[username]/[reponame]/blob/[branch]/image.jpg?raw=true) -->
![alt text](https://github.com/tchedem/ansible-projects/blob/main/ressources/ansible-manul_drupal-infra.drawio.svg)

<!-- https://github.com/tchedem/ansible-projects/blob/main/ressources/ansible-manul_drupal-infra.drawio.svg -->
=======
# Deploiement process


<!-- You can find the project architecture [here](#project-architecture).
## Project Architecture -->


You can download the project architecture [here](https://raw.githubusercontent.com/tchedem/ansible-projects/main/ressources/ansible-manul_drupal-infra.drawio.png).

<hr/>

Start the project with the following commands :

```
# Run the VM
cd .2-lamp_with_drupal_app
vagrant up

# Move to Ansible provisionning directory and run the playbook
cd ./provisionning
ansible-playbook playbook.yml -i hosts.ini -u vagrant
```

### Architecture of the projects

![project-architecture](https://raw.githubusercontent.com/tchedem/ansible-projects/main/ressources/ansible-manul_drupal-infra.drawio.png)

>>>>>>> d02e358 (fix drupal installation pbb and update meilisearch deploiement script)
