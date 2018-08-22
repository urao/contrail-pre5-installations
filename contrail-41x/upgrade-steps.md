## Steps to upgrade from 4.1.0 to 4.1.1

1. Download contrail 4.1.1 package from [here](https://www.juniper.net/support/downloads/?p=contrail#sw)
2. Copy the above downloaded packages into $HOME/images folder
3. Copy [sample-upgrade-image.json](https://github.com/urao/contrail-pre5-installations/blob/master/contrail-41x/sample-upgrade-image.json) into $HOME/jsons folder and modify the image name and location
4. Upgrade Contrail
```
server-manager add image –f $HOME/jsons/sample-upgrade-image.json
server-manager provision --cluster_id <cluster_id> <new_image_id>

<cluster_id> --> server-manager display cluster
<image_id> --> server-manager display image
```
8. Check status of the depolyment
```
watch /opt/contrail/contrail_server_manager/provision_status.sh
```
9. Debug logs
```
tail -f /var/log/contrail-server-manager/debug.log
```
