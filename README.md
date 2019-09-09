# Oracle_Linux_Python_to_ADW/ATP
How to setup Oracle Linux to use python to execute SQL commands on an ADW/ATP Instance in Oracle Cloud.

## Gather the required files and install.
1. Download your cloud wallet zip files that contain your sqlnet.ora, tnsnames.ora and cwallet.sso files. These can be downloaded from the ATP/ADW instance from the web interface console.

2. Install https://www.oracle.com/database/technologies/instant-client/linux-x86-64-downloads.html and download the oracle instant client. Pay attention to the version that you are downloading, as the environment needs to be setup with the correct paths. In this version I am using oracle instant client 19.3, if you are using a newer version or an older version make sure you change the instantclient_X_X to your needs.

3. sudo yum -y install oraclelinux-developer-release-el7

4. sudo yum -y install python-cx_Oracle

5. sudo mkdir -p /opt/oracle

6. sudo unzip /home/opc/instant_client.zip

7. sudo mv instantclient_19_3/ /opt/oracle/

8. cd /opt/oracle/instantclient_19_3/

9. sudo yum install libaio

10. sudo sh -c "echo /opt/oracle/instantclient_19_3 > /etc/ld.so.conf.d/oracle-instantclient.conf"

11. export LD_LIBRARY_PATH=/opt/oracle/instantclient_19_3:$LD_LIBRARY_PATH

12. mkdir -p /opt/oracle/instantclient_19_3/network/admin

13. export TNS_ADMIN=/opt/oracle/instantclient_19_3/network/admin/

14. sudo ldconfig

15. sudo cp cwallet.sso  /opt/oracle/instantclient_19_3/network/admin/

16. sudo cp tnsnames.ora  /opt/oracle/instantclient_19_3/network/admin/

17. sudo cp sqlnet.ora /opt/oracle/instantclient_19_3/network/admin/


## Python
At this point you should have the python module installed. A test script to connect to your database is bellow:


#!/usr/bin/env python

import cx_Oracle

print('preconnection')

connection = cx_Oracle.connect('USERNAME', 'PASSWORD', 'SERVICE_NAME')

print('Connected')

cursor = connection.cursor()

cursor.execute('SELECT * from TEST')

print('Executed Select')

connection.close()
