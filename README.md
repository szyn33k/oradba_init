# OraDBA Initialization Scripts

Setup and initialization scripts for Oracle environments on Docker, Vagrant
boxes or standalone installations. They are especially used in my GitHub 
repositories [oehrlis/docker](https://github.com/oehrlis/docker), 
[oehrlis/docker-database](https://github.com/oehrlis/docker-database) and
[oehrlis/vagrant-boxes](https://github.com/oehrlis/vagrant-boxes) for 
installation and configuration.

A few hints and rules for using the scripts:

* The scripts can be executed individually or sequentially.
* For a complete setup they have to be executed in sequence.
* Scripts with the same prefix are usually variants and mutually exclusive. e.g. you run either ``01_setup_os_db.sh``  or ``01_setup_os_oud.sh``.
* The configuration and parameterization is done via environment variables. The scripts do use default values which can be overwritten by setting the corresponding environment variables. e.g. default ORACLE_DATA is set to ``/u01`` if an alternative path should be used ORACLE_DATA has to be set prior executing the scripts.
* Customiszation can be done by setting the corresponding environment variable in ``00_setup_oradba_init.sh``. This script will be sourced on all following scripts.

## Scripts

Scripts are located in the *bin* folder.

| Script                                                 | Runas  | Description                                                                             |
| ------------------------------------------------------ | ------ | --------------------------------------------------------------------------------------- |
| [00_setup_oradba_init.sh](bin/00_setup_oradba_init.sh) | root   | Script to initialize and install oradba init scripts.                                   |
| [01_setup_os_db.sh](bin/01_setup_os_db.sh)             | root   | Script to configure Oracle Enterprise Linux for Oracle Database installations.          |
| [01_setup_os_java.sh](bin/01_setup_os_java.sh)         | root   | Script to install Oracle server jre.                                                    |
| [01_setup_os_oud.sh](bin/01_setup_os_oud.sh)           | root   | Script to configure Oracle Enterprise Linux for Oracle Unified Directory installations. |
| [10_setup_db.sh](bin/10_setup_db.sh)                   | oracle | Generic script to install Oracle databases binaries                                     |
| [10_setup_db_11.2.sh](bin/10_setup_db_11.2.sh)         | oracle | Wrapper script to install Oracle 11.2.0.4 databases binaries                            |
| [10_setup_db_12.1.sh](bin/10_setup_db_12.1.sh)         | oracle | Wrapper script to install Oracle 12.1.0.2 databases binaries                            |
| [10_setup_db_12.2.sh](bin/10_setup_db_12.2.sh)         | oracle | Wrapper script to install Oracle 12.2.0.1 databases binaries                            |
| [10_setup_db_18.3.sh](bin/10_setup_db_18.3.sh)         | oracle | Wrapper script to install Oracle 18.3.0.0 databases binaries                            |
| [10_setup_oud_11g.sh](bin/10_setup_oud_11g.sh)         | oracle | Script to install Oracle Unified Directory 11g                                          |
| [10_setup_oud_12c.sh](bin/10_setup_oud_12c.sh)         | oracle | Script to install Oracle Unified Directory 12c                                          |
| [10_setup_oudsm_12c.sh](bin/10_setup_oudsm_12c.sh)     | oracle | Script to install Oracle Unified Directory Service Manager 12c                          |
| [11_setup_db_patch.sh](bin/11_setup_db_patch.sh)       | oracle | Script to patch Oracle Database binaries. If necessary called by *10_setup_db.sh* |
| [11_setup_oud_patch.sh](bin/11_setup_oud_patch.sh)     | oracle | Script to patch Oracle Unified Directory binaries. If necessary called by *10_setup_oud_xx.sh* |
| [20_setup_basenv.sh](bin/20_setup_basenv.sh)           | oracle | Script to setup and configure TVD-Basenv                                                |
| [20_setup_oudbase.sh](bin/20_setup_oudbase.sh)         | oracle | Script to setup and configure OUD Base                                                  |
| [50_run_database.sh](bin/50_run_database.sh)           | oracle | Script to run an Oracle database (Docker). If necessary the script will create a new database |
| [50_start_database.sh](bin/50_start_database.sh)       | oracle | Script to start an Oracle database (Docker)                                                   |
| [52_create_database.sh](bin/52_create_database.sh)     | oracle | Script to create a database                                                  |
| [55_check_database.sh](bin/55_check_database.sh)       | oracle | Script to check an Oracle database (Docker)                                                  |
| [55_config_database.sh](bin/55_config_database.sh)     | oracle | Script to setup an Oracle database (Docker)                                                  |

## Response Files

Response files are located in the *rsp* folder.

| Response file                                                    | Description                                            |
| ---------------------------------------------------------------- | ------------------------------------------------------ |
| [base_install.rsp.tmpl](rsp/base_install.rsp.tmpl)               | Response file for Trivadis BasEnv installation         |
| [db_install.rsp.tmpl](rsp/db_install.rsp.tmpl)                   | Response file for Oracle database binary installations |
| [dbca.dbc.tmpl](rsp/dbca.dbc.tmpl)                               | Generic DB creation assistant (dbca) for 11.2, 12.1, 12.2 and 18c |
| [dbca.rsp.tmpl](rsp/dbca.rsp.tmpl)                               | Response file for DB creation assistant (dbca) for 12.2 and 18c |
| [dbca11.2.0.rsp.tmpl](rsp/dbca11.2.0.rsp.tmpl)                   | Response file for DB creation assistant (dbca) for 11.2.0 |
| [dbca12.1.0.rsp.tmpl](rsp/dbca12.1.0.rsp.tmpl)                   | Response file for DB creation assistant (dbca) for 12.1.0 |
| [ocm.rsp.tmpl](rsp/ocm.rsp.tmpl)                                 | Generic response file OPatch / Oracle Configuration Manager |
| [oud_install.rsp.tmpl](rsp/oud_install.rsp.tmpl)                 | Generic response file for OUD and OUDSM installations  |
| [db_examples_install.rsp.tmpl](rsp/db_examples_install.rsp.tmpl) | Response file for Oracle example installations         |

## Install the Scripts

The scripts can be downloaded directly from GitHub. Usually done to ``/tmp`` folder and extracted to ``/opt``.

### Script based Installation

The ``00_setup_oradba_init.sh`` script itself can be used to download and setup the init scripts. All you need to do is load the script from GitHub, set permissions and execute it.

```bash
sudo rm -rf /opt/oradba
curl -Lf https://github.com/oehrlis/oradba_init/raw/master/bin/00_setup_oradba_init.sh \
    -o /tmp/00_setup_oradba_init.sh
chmod 755 /tmp/00_setup_oradba_init.sh
sudo /tmp/00_setup_oradba_init.sh
ls -alR /opt/oradba
rm -rf /tmp/00_setup_oradba_init.sh
```

Now you can proceed to setup your environment using the scripts in the *bin* folder.

### Manual Installation

Manual installation with download from GitHub.

```bash
curl -f https://codeload.github.com/oehrlis/oradba_init/zip/master -o /tmp/oradba_init.zip
unzip -o /tmp/oradba_init.zip -d /opt
mv /opt/oradba_init-master /opt/oradba
mv /opt/oradba/README.md /opt/oradba/doc
rm -rf /tmp/oradba_init.zip
```

## Using the OraDBA init scripts

Navigate to the *bin* folder to start using the Scripts. If necessary you can customize in ``00_setup_oradba_init.sh`` eg. specify an alternative ORACLE_HOME folder.

```bash
cd /opt/oradba/bin
```

### Setup a Database Server

To setup a database server you have to run the following scripts

* customize *00_setup_oradba_init.sh* or set adequate environment variables for your software, patch, home etc.
* put the software respective ZIP files to a local folder e.g *SOFTWARE* or provide an url for a software repository e.g. *SOFTWARE_REPO*.
* execute *01_setup_os_db.sh* to configure your OS
* change to user oracle
* execute *10_setup_db_xx.x.sh* to install your DB binaries. The script will implicit call the patch script
* execute *20_setup_basenv.sh* to configure TVD-BasEnv
* execute *3x_setup_xxx.sh* to create a database 
* execute *4x_setup_xxx.sh* to configure your database

export DB_BASE_PKG="LINUX.X64_180000_db_home.zip"
export DB_EXAMPLE_PKG="LINUX.X64_180000_examples.zip"
export DB_PATCH_PKG="p28655784_180000_Linux-x86-64.zip"
export DB_OJVM_PKG="p28502229_180000_Linux-x86-64.zip"
export DB_OPATCH_PKG="p6880880_180000_Linux-x86-64.zip"
export ORACLE_HOME_NAME="18.4.0.0"
export ORACLE_HOME="/u00/app/oracle/product/${ORACLE_HOME_NAME}"

export DB_BASE_PKG="linuxx64_12201_database.zip"
export DB_EXAMPLE_PKG="linuxx64_12201_examples.zip"
export DB_PATCH_PKG="p28662603_122010_Linux-x86-64.zip"
export DB_OJVM_PKG="p28440725_122010_Linux-x86-64.zip"
export DB_OPATCH_PKG="p6880880_122010_Linux-x86-64.zip"
export ORACLE_HOME_NAME="12.2.0.1"
export ORACLE_HOME="/u00/app/oracle/product/${ORACLE_HOME_NAME}"
/opt/oradba/bin/10_setup_db_12.2.sh

```bash
/opt/oradba/bin/01_setup_os_db.sh
su - oracle
/opt/oradba/bin/10_setup_db_18.3.sh
/opt/oradba/bin/10_setup_db_12.2.sh
/opt/oradba/bin/20_setup_basenv.sh
```

### Setup a OUD Server

To setup a database server you have to run the following scripts

* customize *00_setup_oradba_init.sh* or set adequate environment variables for your software, patch, home etc.
* put the software respective ZIP files to a local folder e.g *SOFTWARE* or provide an url for a software repository e.g. *SOFTWARE_REPO*.
* execute *01_setup_os_oud.sh* to configure your OS
* execute *01_setup_os_java.sh* to configure your Java
* execute *10_setup_oud_xx.sh* to install your DB binaries. The script will implicit call the patch script
* execute *20_setup_oudbase.sh* to configure OUD BasEnv

```bash
/opt/oradba/bin/01_setup_os_oud.sh
su - oracle
/opt/oradba/bin/01_setup_os_java.sh
/opt/oradba/bin/10_setup_oud_12c.sh
/opt/oradba/bin/10_setup_oudsm_12c.sh
/opt/oradba/bin/20_setup_oudbase.sh
```

### Remove DB Stuff

For test purpose I did have to remove all the stuff a couple of times. Just be aware this is **highly critical**. You will remove your DB, binaries etc on your system.

```bash
rm -rf /u00 /u01 /u02
rm -rf /home/oracle
rm -rf /var/mail/oracle
rm -rf /docker-entrypoint-initdb.d
rm -rf /opt/oradba
userdel oracle
for i in $(grep -i '^os' /etc/group|cut -d: -f1) oinstall; do groupdel $i; done
yum -y erase oracle-rdbms-server-11gR2-preinstall \
    oracle-rdbms-server-12cR1-preinstall \
    oracle-database-server-12cR2-preinstall \
    oracle-database-preinstall-18c
```

## Customization

tbd

## Todo

* db create scripts
    * enable unified audit
    * setup links for network stuff
    * create scripts
    * create training folder
    * test kerberos
* oudbase issue bash profile update
* Windows AD
    * putty config 
    * putty agent auto start
    * 
