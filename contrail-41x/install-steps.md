## Steps to deploy non-HA enivornment running 1 Contrail Controllers + 1 compute 

1. Bring up 2 Servers running Ubuntu 16.04.2 LTS Xenial OS with 12 vCPU, 128 GB of RAM and 500 GB of disk
2. Create bond interfaces if required for your topology, this example has bond interface used for ctrl/data interfaces
3. Create directories 
```
mkdir -p $HOME/images
mkdir -p $HOME/jsons
```
4. Download contrail 4.1.x package from [here](https://www.juniper.net/support/downloads/?p=contrail#sw)
4. Download contrail 4.1.x Ubuntu-16.04 server manager package from [here](https://www.juniper.net/support/downloads/?p=contrail#sw)
5. Copy the above downloaded packages into $HOME/images folder
6. Copy [sample-combined.json](https://github.com/urao/contrail-pre5-installations/blob/master/contrail-41x/sample-combined.json) into $HOME/jsons folder and modify the IP address
7. Deploy Contrail
```
cd $HOME/images
tar -zxvf contrail-cloud-docker_<version>-<openstack_version>_xenial.tgz
dpkg -i contrail-server-manager-installer_<version>_xenial.deb
mv /etc/apt/sources.list /tmp/
touch /etc/apt/sources.list
apt-get update
/opt/contrail/contrail_server_manager/provision_containers.sh -h
/opt/contrail/contrail_server_manager/provision_containers.sh -j $HOME/jsons/combined.json
```
8. Check status of the depolyment
```
watch /opt/contrail/contrail_server_manager/provision_status.sh
```
9. Debug logs
```
tail -f /var/log/contrail-server-manager/debug.log
```
