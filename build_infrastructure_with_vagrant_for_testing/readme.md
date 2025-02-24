You will see the infra here

![alt text](images/image.png)


Some ad-hoc command (For more details about params and options of the ansible command, type `ansible -h`)

```bash
# Get the hostname of each hosts in multi group
ansible multi -a "hostname" -f 1
# -a : args
# hostname : the action to run
# -f : Fork (Specify the number of parallel processes to use (default=5))

# Check available disk space on the servers
ansible multi -a "df -h" -f1

# Checkout available RAM memory
ansible multi -a "free -h" -f1

# Checkout date and time
ansible multi -a "date" -f 1

# Check the version of a package
ansible multi -m shell -a "python3 --version"

# To get a list of all echaustive details related to a host, you can use the command bellow
ansible [host-or-group] -m setup 


#### Make change using ansible modules 
# Update and upgrade all packages
 ansible-venv/bin/ansible  multi -m dnf -a "name=* state=latest"


# Install chrony 
ansible multi -b -m dnf -a "name=chrony state=present enabled=yes"
ansible multi -b -m service -a "name=chronyd enabled=yes state=started"

# Check if our servers are in time
ansible multi -b -a "chronyc tracking"

# To know witch NTP service is used on a server 
ps aux | grep -E 'chrony|ntpd|systemd-timesyncd'
```

##### Configure groups of servers, or individual servers 
###### Configure the Application servers
Here, we will work based on our architecture
```bash
# Install django
# NB: Based on you python version installed your installation proccess might be different
# ansible app -b -m dnf -a "name=python3-pip state=present"
# ansible app -b -m pip -a "executable=pip3 name=django<4 state=present"

ansible app -b -m dnf -a "name=python3.12-pip state=present" || ansible app -b -m shell -a "python3 -m ensurepip && python3 -m pip install --upgrade pip"
ansible app -b -m pip -a "executable=pip3 name=django<4 state=present"

# Check to make sure Django is working correctly
ansible app -a "python3 -m django --version"
```

###### Configure the Database servers
```bash
# Install mariad and started it
ansible db -b -m dnf -a "name=mariadb-server state=present"
ansible db -b -m service -a "name=mariadb state=started enabled=yes"

# COnfigure the system firewall to ensure that only the system can access the database
ansible db -b -m dnf -a "name=firewalld state=present"
ansible db -b -m service -a "name=firewalld state=started enabled=yes"
 ansible db -b -m firewalld -a "zone=database state=present permanent=yes"
 ansible db -b -m firewalld -a "source=192.168.56.0/24 zone=database state=enabled permanent=yes"
 ansible db -b -m firewalld -a "port=3306/tcp zone=database state=enabled permanent=yes"

# Configure MariaDB 
 ansible db -b -m dnf -a "name=python3-PyMySQL state=present"
 ansible db -b -m community.mysql.mysql_user -a "name=django host=% password=12345 priv=*.*:ALL state=present"

# take a look at p32 i section
```

###### Make changes to just one server
```bash
# let's just take an ipotetycally example, suppose you run a command on a group of 2 hosts to just realize that the command just work on one.
# Ex
ansible app -b -a "systemctl status chronyd"

# Then, you decided to run it agaist the affected server
ansible app -b -a "service chronyd restart" --limit "192.168.56.4"
# Or you can use a syntax like this
# Limit hosts with a simple pattern (asterisk is a wildcard).
$ ansible app -b -a "service chronyd restart" --limit "*.4"
# Limit hosts with a regular expression (prefix with a tilde).
$ ansible app -b -a "service chronyd restart" --limit ~".*\.4"
# The best way to deal with this kind of situation is to add your hosts in a new group and use the standard ansible command
ansible [host-group-name] -m module ...
```

###### Manage users and groups
```bash
# add an admin group on the `app` servers for the server administrator
ansible app -b -m group -a "name=admin state=present"
```

---


Example of an ansible task VS bash script
```bash
# The task in aad-hoc mode
ansible multi -b -m service -a "name=chronyd enabled=yes state=started"

#### The bash script
# Start chronyd if it's not already running.
if ps aux | grep -q "[c]hronyd"
then
    echo "chronyd is running." > /dev/null
else
    systemctl start chronyd.service > /dev/null
    echo "Started chronyd."
fi
# Make sure chronyd is enabled on system startup.
systemctl enable chronyd.service

```

Exemple of ansible simple playbook syntax without the name parameter
```yaml
# playboo.yaml
---

- hosts: all
  become: yes
    tasks:
    - dnf: name=chrony state=present
    - service: name=chronyd state=started enabled=yes
```

---

Sysadmin daily tasks

• Apply patches and updates via dnf, apt, and other package managers.
• Check resource usage (disk space, memory, CPU, swap space, network).
• Check log files.
• Manage system users and groups.
• Manage DNS settings, hosts files, etc.
• Copy files to and from servers.
• Deploy applications or run application maintenance.
• Reboot servers.
• Manage cron jobs.


--- 

### Quotes

The best way to prevent disaster, is to know when it could be comming and fix the problem before it happens.
We should use tools like Nagios, Cactic, Munin, Hyperic, to have idea of our server past and present resource usage

If we a running a website or web application available over the internet, we should use tools like: Uptime Robot or PingDom


Most applications are written with slight tolerances for per-server time jitter, but it’s always a good idea to make sure the times on the different servers are as close as possible, and the simplest way to do that is to use the Network Time Protocol.

thorough : approfondi
fare. : tarrif