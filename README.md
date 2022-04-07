* [setup steps](./SAPHANA-setup.md)

* [load tpc-ds data](./tpc.md)

## Connecting

azure sub:  davew-shared
rg:  davew-saphana.eastus.cloudapp.azure.com

server:  davew-saphana.
vm login:  hana/Password01!!

connecting:  
(jit is enabled, need to maybe fix that)
ssh hana@davew-saphana.eastus.cloudapp.azure.com


nsg rules changed to allow 39017 and 39041 (priority 2711)


SYSTEM pwd:  adfADFADFer4545!!!
## from jdbc:  
jdbc:sap://davew-saphana.eastus.cloudapp.azure.com:39017/?databaseName=SYSTEMDB
jdbc:sap://davew-saphana.eastus.cloudapp.azure.com:39041/?databaseName=HXE

## from vscode:
extension:  sap hana driver for SQLTools
click SQLTools on the left

mtc/Password01

![](./img/conn.png)


## ...or install hana studio
you can use my sapid



## sample queries
select * from "SYS"."M_DATABASES"
select * from tables




create table mtc.foobar (element char(1))		
INSERT INTO mtc.foobar VALUES ('F');
INSERT INTO mtc.foobar VALUES ('U');			
INSERT INTO mtc.foobar VALUES ('B');			
INSERT	INTO mtc.foobar VALUES ('A');			
INSERT	INTO mtc.foobar VALUES ('R');

select * from MTC.Foobar


## change password

* ssh to vm
* docker exec to container
* hdbsql -i 90 -d SYSTEMDB -u SYSTEM
* alter system stop database HXE
* ALTER USER SYSTEM PASSWORD Password01
* ALTER SYSTEM START DATABASE HXE