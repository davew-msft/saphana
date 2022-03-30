sapid (if someone needs it to download software):  dave@davewentzel.com/Password01!!

docs:  https://developers.sap.com/group.hxe-install-binary.html




## How to Build the VM 

* from AMP use SUSE Enterprise for SAP 15
* B series with 24GB ram is sufficient
* take defaults
* ssh to box and follow these instructions:  
https://hub.docker.com/_/sap-hana-express-edition/plans/f2dc436a-d851-4c22-a2ba-9de07db7a9ac?tab=instructions
* here's the steps that actually work:

```bash
sudo su
docker login
dwentzel
Password01!!
systemctl start docker
docker run --name helloWorld alpine echo hello

cat <<EOF >> /etc/sysctl.d/hana.conf 
fs.file-max=20000000
fs.aio-max-nr=262144
vm.memory_failure_early_kill=1
vm.max_map_count=135217728
net.ipv4.ip_local_port_range=40000 60999
EOF

exit


# update hosts file
sudo sh -c 'echo 10.0.0.4  hxehost >> /etc/hosts'

# data folder steps
mkdir -p ~/hanadata

cat <<EOF >> ~/hanadata/hana.json
{
"master_password" : "adfADFADFer4545!!!"
}
EOF

sudo chown 12000:79 ~/hanadata
sudo chmod 777 ~/hanadata
sudo chmod 777 ~/hanadata/hana.json
sudo chown 12000:79 ~/hanadata/hana.json

sudo docker pull store/saplabs/hanaexpress:2.00.057.00.20220119.1

sudo docker run \
    -p 39013:39013 \
    -p 39017:39017 \
    -p 39041-39045:39041-39045 \
    -p 1128-1129:1128-1129 \
    -p 59013-59014:59013-59014 \
    -v ~/hanadata:/hana/mounts \
    --ulimit nofile=1048576:1048576 \
    --sysctl kernel.shmmax=1073741824 \
    --sysctl net.ipv4.ip_local_port_range='40000 60999' \
    --sysctl kernel.shmmni=4096 \
    --sysctl kernel.shmall=8388608 \
    --name saphana \
    store/saplabs/hanaexpress:2.00.057.00.20220119.1 \
    --passwords-url file:///hana/mounts/hana.json \
    --agree-to-sap-license

sudo docker start saphana
sudo docker ps

# need to blow away container???
# sudo docker rm saphana
# sudo rm -rf hanadata
# then need to rebuild hanadata above every time the container is blown away 

sudo docker exec -it saphana bash
whoami
HDB info

# adfADFADFer4545!!!
hdbsql -i 90 -d SYSTEMDB -u SYSTEM 
select * from "SYS"."M_DATABASES"
quit
hdbsql -i 90 -d HXE -u SYSTEM


```


Start "HXE" tenant database. 
hana clients:  https://developers.sap.com/group.hxe-install-clients.html  


-----------------------
Log files copied to '/hana/mounts/trace/b79049c24cf2' on host 'b79049c24cf2'.