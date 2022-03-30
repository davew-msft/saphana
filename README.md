* [setup steps](./SAPHANA-setup.md)

## Connecting

azure sub:  davew-shared
rg:  davew-saphana.eastus.cloudapp.azure.com

server:  davew-saphana.
vm login:  hana/Password01!!

connecting:  
(jit is enabled, need to maybe fix that)
ssh hana@davew-saphana.eastus.cloudapp.azure.com


nsg rules changed to allow 39017 and 39041 (priority 2711)


from jdbc:  
jdbc:sap://<ip_address>:39017/?databaseName=<database_name>
jdbc:sap://<ip_address>:39041/?databaseName=<tenant_name>

from vscode:

...or install hana studio

