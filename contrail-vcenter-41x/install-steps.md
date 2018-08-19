## Steps to deploy non-HA Contrail Controller services + vCenter only 

0. Setup running 1 Contrail Controllers + 3 ESXi compute + 1 Vcenter with no separate ctrl/data interfaces
1. Bring up 1 Server running Ubuntu 16.04.2 LTS Xenial OS with 12 vCPU, 128 GB of RAM and 500 GB of disk
2. Bring up vCenter running 6.0, ESXI compute nodes running 6.0, make sure hostname does not have dashes and special characters
3. On ESXI server, make sure single uplink vswitch0, no LACP or MPI/O, 300GB storage and 128GB RAM
4. Infrastructure requirements 
	1. IP reachability between contrail controller and vcenter and no Firewall between them
	2. Enable DHCP Option 12 (Hostname) on the DHCP server, to assign hostnames for Contrail vRouter VMs
	3. Have internet access 
5. Create directories 
```
mkdir -p $HOME/images
mkdir -p $HOME/jsons
```
6. Download contrail vcenter 4.1.1 package from [here](https://www.juniper.net/support/downloads/?p=contrail#sw)
7. Download contrail 4.1.1 Ubuntu-16.04 server manager package from [here](https://www.juniper.net/support/downloads/?p=contrail#sw)
8. Download contrail vm tar file for specific contrail release [here](https://www.juniper.net/support/downloads/?p=contrail#sw)
9. Copy the above downloaded packages into $HOME/images folder
10. Copy [sample-combined.json](https://github.com/urao/contrail-pre5-installations/blob/master/contrail-vcenter-41x/sample-combined.json) into $HOME/jsons folder and modify IP address, username, passwords
11. Deploy Contrail
```
cd $HOME/images
dpkg -i contrail-server-manager-installer_<version>_xenial.deb
mv /etc/apt/sources.list /tmp/
touch /etc/apt/sources.list
apt-get update
/opt/contrail/contrail_server_manager/provision_containers.sh -h
/opt/contrail/contrail_server_manager/provision_containers.sh -j $HOME/jsons/combined.json
```
12. Check status of the depolyment
```
watch /opt/contrail/contrail_server_manager/provision_status.sh
```
13. Debug logs
```
tail -f /var/log/contrail-server-manager/debug.log
```
14. Connect to vCenter Contrail GUI to create Virtual Networks, otherwise use regular Contrail GUI
```
vCenter Contrail GUI: https://<Controller_IP>:8143/vcenter  
credentials: administrator@vsphere.local/<vcenter_password>
```
```
Contrail GUI: https://<Controller_IP>:8143  
credentials: admin/c0ntrail123
```
