#### What is Meilisearch ?

Meilisearch is a search engine. It was build focus on fast, revelant search capabilities and easy to integrate in web or mobile applications.

My Goal here, is to show you how you can install deploy it on your Linux server and use it in your applications.

*This file show you how you can do it manually. So just enjoy it.*

***NB:** Sometimes, you might need to have multiple instance of meilisearch in different environement. If you have many server, you can install one instance on each server. But if you don't, you will need to install different instance of meiliseach on the same server for each environment.*

---

Update system packages database & Install `curl` on the system

```BASH
sudo apt update

sudo apt install curl -y
```


Download meilisearch

```BASH
curl -L https://install.meilisearch.com | sh
```


Make meilisearch executable

```BASH
sudo chmod +x meilisearch
```


If you want, you can directly run meilisearch ***(Not necessary)***

```BASH
# run it
./meilisearch 

# Or you can run it and specify the address and the port you want to use to expose the service
./meilisearch --http-addr 192.168.56.10:7701
```


Configure `Meilisearch` as a `Linux` service

```BASH
# move meilisearch in Linux executable or scripts folder
sudo mv ./meilisearch /usr/local/bin/meilisearch

# Create meilisearch user with the specified home directory and restricted shell
sudo useradd -d /var/lib/meilisearch -b /bin/false -m -r meilisearch

# To delete the user aacount, you can use this command :
# $ sudo rm -rf /var/lib/mpu; sudo userdel mpu;

# Change the owner of the meilisearch app
# sudo chown mpu:mpu /usr/local/bin/meilisearch_production


#########################################
# MEILISEARCH configuration file
Here, you have two solutions. You can download meilisearch default configuration file and customize it based on your need

Or, you can use my file
#########################################

# Solution 1: Download meilisearch configuration file, move it in packages configurations folder and customize it
sudo curl https://raw.githubusercontent.com/meilisearch/meilisearch/latest/config.toml | sudo tee /etc/meilisearch_production.toml > /dev/null

# Solution 2: Use the content bellow
sudo bash -c 'cat <<EOF > /etc/meilisearch.toml
db_path = "/var/lib/meilisearch/data"
env = "development"

# Here, you can use you ip address on a interface to make meilisearch dirrectly accessible.
http_addr = "localhost:7700"
master_key = "c3c4ef18-1852-11ef-bcef-508492d5c357"

http_payload_size_limit = "100 MB"
log_level = "INFO"
max_indexing_memory = "2 GiB"

dump_dir = "/var/lib/meilisearch/dumps"
ignore_missing_dump = false
ignore_dump_if_db_exists = false

schedule_snapshot = false

snapshot_dir = "/var/lib/meilisearch/snapshots"
ignore_missing_snapshot = false
ignore_snapshot_if_db_exists = false

ssl_require_auth = false
ssl_resumption = false
ssl_tickets = false

experimental_enable_metrics = false
experimental_reduce_indexing_memory_usage = false
EOF'


# Create meilisearch service file
sudo bash -c 'cat << EOF > /etc/systemd/system/meilisearch.service
[Unit]
Description=Meilisearch
After=systemd-user-sessions.service

[Service]
Type=simple
WorkingDirectory=/var/lib/meilisearch
ExecStart=/usr/local/bin/meilisearch --config-file-path /etc/meilisearch.toml
User=meilisearch
Group=meilisearch

[Install]
WantedBy=multi-user.target
EOF'
```

Reload systemd manager
```BASH
sudo systemctl daemon-reload
```

After it's done, we'll enable the meilisearch service
```BASH
sudo systemctl enable meilisearch
```

Start the meilisearch service
```BASH
sudo systemctl start meilisearch
```

Check the status of meilisearch service to be sure that it is actually running
```BASH
sudo systemctl status meilisearch
```

If you have a problem, you can try to fix it and reload meilisearch service
```BASH
sudo systemctl restart meilisearch
```


To visualise your process, you can use the command:
```BASH
ps -aux|grep meil
``` 


After all these steps done, you can configure a Virtual Host On Nginx, who will redirect all your traffic from your domain name to your local instance of meilisearch.

Self manage the SSL/TLS certificats on your domaine with Certbot or use [Cloudflare](https://www.cloudflare.com/)


--------

<br/><br/><br/><br/><br/><br/><br/>

If you want tto reconfigure it as another service on the same server:
```BASH

sudo apt update

sudo apt install curl -y

curl -L https://install.meilisearch.com | sh

sudo chmod +x meilisearch

sudo mv ./meilisearch /usr/local/bin/meilisearch_production

sudo useradd -d /var/lib/meilisearch_production -b /bin/false -m -r meilisearch_production

# sudo chown mpu:mpu /usr/local/bin/meilisearch_production

# meilisearch configuration file
sudo nano /etc/meilisearch_production.toml


#### Paste the content bellow in the file: `/etc/meilisearch_production.toml`

# This file shows the default configuration of Meilisearch.
# All variables are defined here: https://www.meilisearch.com/docs/learn/configuration/instance_options#environment-variables

# Designates the location where database files will be created and retrieved.
# https://www.meilisearch.com/docs/learn/configuration/instance_options#database-path
#db_path = "./data.ms"
db_path = "/var/lib/meilisearch_production/data"

# Configures the instance's environment. Value must be either `production` or `development`.
# https://www.meilisearch.com/docs/learn/configuration/instance_options#environment
# env = "development"
env = "production"

# The address on which the HTTP server will listen.
http_addr = "localhost:7701"
#http_addr = "199.168.56.10:7700"

# Sets the instance's master key, automatically protecting all routes except GET /health.
# https://www.meilisearch.com/docs/learn/configuration/instance_options#master-key
# master_key = "YOUR_MASTER_KEY_VALUE"
master_key = "3d9c85c6-1830-11ef-bcef-508492d5c357"

# Deactivates Meilisearch's built-in telemetry when provided.
# Meilisearch automatically collects data from all instances that do not opt out using this flag.
# All gathered data is used solely for the purpose of improving Meilisearch, and can be deleted at any time.
# https://www.meilisearch.com/docs/learn/configuration/instance_options#disable-analytics
# no_analytics = true

# Sets the maximum size of accepted payloads.
# https://www.meilisearch.com/docs/learn/configuration/instance_options#payload-limit-size
http_payload_size_limit = "100 MB"

# Defines how much detail should be present in Meilisearch's logs.
# Meilisearch currently supports six log levels, listed in order of increasing verbosity:  `OFF`, `ERROR`, `WARN`, `INFO`, `DEBUG`, `TRACE`
# https://www.meilisearch.com/docs/learn/configuration/instance_options#log-level
log_level = "INFO"

# Sets the maximum amount of RAM Meilisearch can use when indexing.
# https://www.meilisearch.com/docs/learn/configuration/instance_options#max-indexing-memory
max_indexing_memory = "2 GiB"

# Sets the maximum number of threads Meilisearch can use during indexing.
# https://www.meilisearch.com/docs/learn/configuration/instance_options#max-indexing-threads
# max_indexing_threads = 4


#############
### DUMPS ###
#############
# Sets the directory where Meilisearch will create dump files.
# https://www.meilisearch.com/docs/learn/configuration/instance_options#dump-directory
#dump_dir = "dumps/"
dump_dir = "/var/lib/meilisearch_production/dumps"

# Imports the dump file located at the specified path. Path must point to a .dump file.
# https://www.meilisearch.com/docs/learn/configuration/instance_options#import-dump
# import_dump = "./path/to/my/file.dump"

# Prevents Meilisearch from throwing an error when `import_dump` does not point to a valid dump file.
# https://www.meilisearch.com/docs/learn/configuration/instance_options#ignore-missing-dump
ignore_missing_dump = false

# Prevents a Meilisearch instance with an existing database from throwing an error when using `import_dump`.
# https://www.meilisearch.com/docs/learn/configuration/instance_options#ignore-dump-if-db-exists
ignore_dump_if_db_exists = false


#################
### SNAPSHOTS ###
#################
# Enables scheduled snapshots when true, disable when false (the default).
# If the value is given as an integer, then enables the scheduled snapshot with the passed value as the interval
# between each snapshot, in seconds.
# https://www.meilisearch.com/docs/learn/configuration/instance_options#schedule-snapshot-creation
schedule_snapshot = false

# Sets the directory where Meilisearch will store snapshots.
# https://www.meilisearch.com/docs/learn/configuration/instance_options#snapshot-destination
#snapshot_dir = "snapshots/"
snapshot_dir = "/var/lib/meilisearch_production/snapshots"

# Launches Meilisearch after importing a previously-generated snapshot at the given filepath.
# https://www.meilisearch.com/docs/learn/configuration/instance_options#import-snapshot
# import_snapshot = "./path/to/my/snapshot"

# Prevents a Meilisearch instance from throwing an error when `import_snapshot` does not point to a valid snapshot file.
# https://www.meilisearch.com/docs/learn/configuration/instance_options#ignore-missing-snapshot
ignore_missing_snapshot = false

# Prevents a Meilisearch instance with an existing database from throwing an error when using `import_snapshot`.
# https://www.meilisearch.com/docs/learn/configuration/instance_options#ignore-snapshot-if-db-exists
ignore_snapshot_if_db_exists = false


###########
### SSL ###
###########
# Enables client authentication in the specified path.
# https://www.meilisearch.com/docs/learn/configuration/instance_options#ssl-authentication-path
# ssl_auth_path = "./path/to/root"

# Sets the server's SSL certificates.
# https://www.meilisearch.com/docs/learn/configuration/instance_options#ssl-certificates-path
# ssl_cert_path = "./path/to/certfile"

# Sets the server's SSL key files.
# https://www.meilisearch.com/docs/learn/configuration/instance_options#ssl-key-path
# ssl_key_path = "./path/to/private-key"

# Sets the server's OCSP file.
# https://www.meilisearch.com/docs/learn/configuration/instance_options#ssl-ocsp-path
# ssl_ocsp_path = "./path/to/ocsp-file"

# Makes SSL authentication mandatory.
# https://www.meilisearch.com/docs/learn/configuration/instance_options#ssl-require-auth
ssl_require_auth = false

# Activates SSL session resumption.
# https://www.meilisearch.com/docs/learn/configuration/instance_options#ssl-resumption
ssl_resumption = false

# Activates SSL tickets.
# https://www.meilisearch.com/docs/learn/configuration/instance_options#ssl-tickets
ssl_tickets = false


#############################
### Experimental features ###
#############################
# Experimental metrics feature. For more information, see: <https://github.com/meilisearch/meilisearch/discussions/3518>
# Enables the Prometheus metrics on the `GET /metrics` endpoint.
experimental_enable_metrics = false

# Experimental RAM reduction during indexing, do not use in production, see: <https://github.com/meilisearch/product/discussions/652>
experimental_reduce_indexing_memory_usage = false

# Experimentally reduces the maximum number of tasks that will be processed at once, see: <https://github.com/orgs/meilisearch/discussions/713>
# experimental_max_number_of_batched_tasks = 100


# meilisearch service file
sudo bash -c 'cat << EOF > /etc/systemd/system/meilisearch_production.service
[Unit]
Description=Meilisearch
After=systemd-user-sessions.service

[Service]
Type=simple
WorkingDirectory=/var/lib/meilisearch_production
ExecStart=/usr/local/bin/meilisearch_production --config-file-path /etc/meilisearch_production.toml
User=meilisearch_production
Group=meilisearch_production

[Install]
WantedBy=multi-user.target
EOF'


# Set the service meilisearch_production
sudo systemctl enable meilisearch_production

# Start the meilisearch_production service
sudo systemctl start meilisearch_production

# If you have a probleme
sudo systemctl restart meilisearch_production

# Verify that the service is actually running
sudo systemctl status meilisearch_production

```