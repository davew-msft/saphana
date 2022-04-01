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


CREATE USER admin PASSWORD Password01 NO FORCE_FIRST_PASSWORD_CHANGE
ALTER  USER admin  DISABLE PASSWORD LIFETIME;  

CREATE ROLE ADMIN_SYSTEM_ROLE;
  GRANT USER ADMIN    TO ADMIN_SYSTEM_ROLE;
  GRANT INIFILE ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT ADAPTER ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT AGENT ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT ATTACH DEBUGGER TO ADMIN_SYSTEM_ROLE;
  GRANT AUDIT ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT AUDIT OPERATOR TO ADMIN_SYSTEM_ROLE;
  GRANT SSL ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT CREATE SCHEMA TO ADMIN_SYSTEM_ROLE;
  GRANT BACKUP ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT BACKUP OPERATOR TO ADMIN_SYSTEM_ROLE;
  GRANT CATALOG READ TO ADMIN_SYSTEM_ROLE;
  GRANT CERTIFICATE ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT CREATE REMOTE SOURCE TO ADMIN_SYSTEM_ROLE;
  GRANT CREATE STRUCTURED PRIVILEGE TO ADMIN_SYSTEM_ROLE;
  GRANT CREDENTIAL ADMIN TO ADMIN_SYSTEM_ROLE;
 GRANT DATA ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT DATABASE ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT EXPORT, IMPORT TO ADMIN_SYSTEM_ROLE;
  GRANT EXTENDED STORAGE ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT INIFILE ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT LICENSE ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT LOG ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT MONITOR ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT OPTIMIZER ADMIN TO ADMIN_SYSTEM_ROLE;
  
  GRANT RESOURCE ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT ROLE ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT SAVEPOINT ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT SCENARIO ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT SERVICE ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT SESSION ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT SSL ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT STRUCTUREDPRIVILEGE ADMIN TO ADMIN_SYSTEM_ROLE;
  
  GRANT TENANT ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT TABLE ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT TRACE ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT TRUST ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT USER ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT VERSION ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT WORKLOAD ANALYZE ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT WORKLOAD CAPTURE ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT WORKLOAD REPLAY ADMIN TO ADMIN_SYSTEM_ROLE;
  GRANT EXECUTE ON "SYS"."REPOSITORY_REST" TO ADMIN_SYSTEM_ROLE;
  GRANT EXECUTE ON "PUBLIC"."GRANT_ACTIVATED_ROLE" TO ADMIN_SYSTEM_ROLE;
  GRANT EXECUTE ON "PUBLIC"."REVOKE_ACTIVATED_ROLE" TO ADMIN_SYSTEM_ROLE;

GRANT ADMIN_SYSTEM_ROLE TO admin





CREATE USER mtc PASSWORD Password01 NO FORCE_FIRST_PASSWORD_CHANGE
CREATE ROLE mtc;
ALTER  USER mtc  DISABLE PASSWORD LIFETIME;  

CREATE SCHEMA mtc;


GRANT SELECT     ON SCHEMA mtc   TO mtc;
GRANT CREATE ANY ON SCHEMA mtc   TO mtc;
GRANT INSERT     ON SCHEMA mtc TO mtc;
GRANT DELETE     ON SCHEMA mtc TO mtc;
GRANT UPDATE     ON SCHEMA mtc TO mtc;
GRANT EXPORT                    TO mtc;
GRANT IMPORT                    TO mtc;
GRANT MONITORING                TO mtc;
GRANT BACKUP OPERATOR           TO mtc;
GRANT CATALOG READ              TO mtc;