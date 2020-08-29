---
title: Setup HammerDB for Database Performance Testing in Ubuntu 20
subTitle: Oracle Database Performance Testing
category: "Database"
tags: [Database, HammerDB]
cover: hammerDB-H-Logo-FB.jpg
---
Setup HammerDB for Oracle Database Performance Testing in Ubuntu 20

## Download and install HammerDB


``` 
export HAMMER_VERSION=3.3
export HAMMER_URI=https://github.com/TPC-Council/HammerDB/releases/download
 
sudo apt-get install -y curl
curl -kLJO {$HAMMER_URI}/v{$HAMMER_VERSION}/HammerDB-{$HAMMER_VERSION}-Linux.tar.gz
tar -zxvf HammerDB-$HAMMER_VERSION-Linux.tar.gz \
  && rm -rf HammerDB-$HAMMER_VERSION-Linux.tar.gz \
  && mv HammerDB-$HAMMER_VERSION hammerdb 
  
```

![](/assets/img/20200804/01_Install_HammerDB.jpg)

## Download and install Oracle Instant Client

Download and install Oracle instant Client here:
[https://www.oracle.com/database/technologies/instant-client/linux-x86-64-downloads.html](https://www.oracle.com/database/technologies/instant-client/linux-x86-64-downloads.html)

![](/assets/img/20200804/02_install_Oracle_instant_client.jpg)

Setup Oracle database connection details in tnsnames.ora:

![](/assets/img/20200804/06_hammerdb_oracle_tns_setup.jpg)


## HammerDBcli librarycheck
Setup environment variables and run librarycheck in hammerdbcli:

``` 
export LD_LIBRARY_PATH=/home/alo/instantclient_19_8
export ORACLE_LIBRARY=/home/alo/instantclient_19_8/libclntsh.so
export PATH=$LD_LIBRARY_PATH:$PATH
``` 

![](/assets/img/20200804/03_librarycheck_failed_for_oracle.jpg)

If you encounter following error, install libaio libraries to resolve:
Oratcl_Init(): Failed to load /home/alo/instantclient_19_8/libclntsh.so with error libaio.so.1: cannot open shared object file: No such file or directory

``` 
sudo apt-get install libaio1 libaio-dev
``` 

![](/assets/img/20200804/04_install_libaio1.jpg)

![](/assets/img/20200804/05_library_check_oracle_check_ok.jpg)



## HammerDB Schema Build
Before start the schema build, setup the service name (need to align with tnsnames.ora), system username and password in the build option 

![](/assets/img/20200804/07_build_option.jpg)

Need to pre-create the tablespace in the database before schema build, otherwise, will get error.

![](/assets/img/20200804/08_create_tpcc_tablespace.jpg)

Start schema build:

![](/assets/img/20200804/09_hammer_db_schema_build.jpg)

Schema Build completed:

![](/assets/img/20200804/10_hammer_schema_build_complete.jpg)

## HammerDB Timed Workload with Time Profiles

Under Driver Options select Timed Driver Script:
![](/assets/img/20200804/11_oracle_tcp_c_timed_driver.jpg)


## Run the workload and the result:

Virtual User Output:
![](/assets/img/20200804/12_virtual_user_output.jpg)

Oracle Metrics:
![](/assets/img/20200804/13_Oracle_Metrisc_output.jpg)

Time Profiles Log:
![](/assets/img/20200804/14_virtual_user_log.jpg)

![](/assets/img/20200804/15_virtual_user_time_profile_log.jpg)
