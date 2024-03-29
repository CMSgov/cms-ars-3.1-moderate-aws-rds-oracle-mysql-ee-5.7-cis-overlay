# cms-ars-3.1-moderate-aws-rds-oracle-mysql-ee-5.7-cis-overlay
**CMS’ ISPG (Information Security and Privacy Group) decided to discontinue funding the customization of MITRE’s Security Automation Framework (SAF) for CMS after September 2023. This repo is now in archive mode, but still accessible. For more information about SAF with current links, see https://security.cms.gov/learn/security-automation-framework-saf**

InSpec profile overlay to validate the secure configuration of AWS RDS Oracle MySQL EE 5.7 against [CIS's](https://www.cisecurity.org/cis-benchmarks/) Oracle MySQL EE 5.7 Benchmark 1.0.0 tailored for [CMS ARS 3.1](https://www.cms.gov/Research-Statistics-Data-and-Systems/CMS-Information-Technology/InformationSecurity/Info-Security-Library-Items/ARS-31-Publication.html) for CMS systems categorized as Moderate. 

## Getting Started  

### InSpec (CINC-auditor) setup
For maximum flexibility/accessibility, we’re moving to “cinc-auditor”, the open-source packaged binary version of Chef InSpec, compiled by the CINC (CINC Is Not Chef) project in coordination with Chef using Chef’s always-open-source InSpec source code. For more information: https://cinc.sh/

It is intended and recommended that CINC-auditor and this profile overlay be run from a __"runner"__ host (such as a DevOps orchestration server, an administrative management system, or a developer's workstation/laptop) against the target. This can be any Unix/Linux/MacOS or Windows runner host, with access to the Internet.

__For the best security of the runner, always install on the runner the _latest version_ of CINC-auditor.__ 

__The simplest way to install CINC-auditor is to use this command for a UNIX/Linux/MacOS runner platform:__
```
curl -L https://omnitruck.cinc.sh/install.sh | sudo bash -s -- -P cinc-auditor
```

__or this command for Windows runner platform (Powershell):__
```
. { iwr -useb https://omnitruck.cinc.sh/install.ps1 } | iex; install -project cinc-auditor
```
To confirm successful install of cinc-auditor:
```
cinc-auditor -v
```
> sample output:  _4.24.32_

Latest versions and other installation options are available at https://cinc.sh/start/auditor/.

### MySQL client setup

To run the MySQL profile against an AWS RDS Instance, CINC-auditor expects the mysql client to be readily available on the same runner system it is installed on.
 
For example, to install the mysql client on a Linux runner host:
```
sudo yum install mysql
```
To confirm successful install of mysql:
```
which mysql
```
> sample output:  _/usr/bin/mysql_
```
mysql –version
```		
> sample output:  *mysql  Ver 15.1 Distrib 5.5.68-MariaDB, for Linux (x86_64) using readline 5.1*

Test mysql connectivity to your instance from your runner host:
```
mysql -u <master user> -p<password>  -h <endpoint>.amazonaws.com -P 3306
```		
> sample output:
>  *Welcome to the MariaDB monitor.  Commands end with ; or \g.*
>  *Your MySQL connection id is 7536*
>  *Server version: 5.6.41 Source distribution*
>
>  *Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.*
>
>  *Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.*
>
>  *MySQL [(none)]
>  *quit*
>  *Bye*

For installation of mysql client on other operating systems for your runner host, visit https://www.mysql.com/
## Tailoring to Your Environment
The following inputs must be configured in an inputs ".yml" file for the profile to run correctly for your specific environment. More information about InSpec inputs can be found in the [InSpec Profile Documentation](https://www.inspec.io/docs/reference/profiles/).
 
```
# Description: Username MySQL DB Server (e.g., 'root')
user: ''

# Description: Password MySQL DB Server (e.g., 'P@ssw0rd1')
password: ''

# Description: Hostname MySQL DB Server (e.g., 'localhost')
host: ''

# Description: Port MySQL DB Server (e.g., 3306)
port: 3306

# Description: Approved version expected to be installed (e.g., ['5.7.31'])
approved_mysql_version: ''

# Description: List of MySQL database users (e.g., ['rdsadmin','testuser1','user2'])
mysql_users: ['rdsadmin']   

# Description: Set to true if the MySQL server has a slave configured
is_mysql_server_slave_configured: false

# Description: List of MySQL administrative users (e.g., ['rdsadmin','testadmin1','admin2'])
mysql_administrative_users: ['rdsadmin'] 

# Description: List of MySQL users allows to modify or create data structures (e.g., ['rdsadmin'])'
mysql_users_allowed_modify_or_create: [] 
```
## Note

It is assumed that the password complexity plugin: validate_password.so is installed, otherwise control 7.6 will fail.

## Running This Overlay Directly from Github

```
# How to run
cinc-auditor exec https://github.com/CMSgov/cms-ars-3.1-moderate-aws-rds-oracle-mysql-ee-5.7-cis-overlay/archive/master.tar.gz --input-file=<path_to_your_inputs_file/name_of_your_inputs_file.yml> --reporter json:<path_to_your_output_file/name_of_your_output_file.json>
```

### Different Run Options

  [Full exec options](https://docs.chef.io/inspec/cli/#options-3)

## Running This Overlay from a local Archive copy 

If your runner is not always expected to have direct access to GitHub, use the following steps to create an archive bundle of this overlay and all of its dependent tests:

(Git is required to clone the InSpec profile using the instructions below. Git can be downloaded from the [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) site.)

When the __"runner"__ host uses this profile overlay for the first time, follow these steps: 

```
mkdir profiles
cd profiles
git clone https://github.com/CMSgov/cms-ars-3.1-moderate-aws-rds-oracle-mysql-ee-5.7-cis-overlay.git
cinc-auditor archive cms-ars-3.1-moderate-aws-rds-oracle-mysql-ee-5.7-cis-overlay
cinc-auditor exec <name of generated archive> --input-file=<path_to_your_inputs_file/name_of_your_inputs_file.yml> --reporter json:<path_to_your_output_file/name_of_your_output_file.json> 
```

For every successive run, follow these steps to always have the latest version of this overlay and dependent profiles:

```
cd cms-ars-3.1-moderate-aws-rds-oracle-mysql-ee-5.7-cis-overlay
git pull
cd ..
cinc-auditor archive cms-ars-3.1-moderate-aws-rds-oracle-mysql-ee-5.7-cis-overlay --overwrite
cinc-auditor exec <name of generated archive> --input-file=<path_to_your_inputs_file/name_of_your_inputs_file.yml> --reporter json:<path_to_your_output_file/name_of_your_output_file.json> 
```

## Using Heimdall for Viewing the JSON Results

The JSON results output file can be loaded into __[heimdall-lite](https://heimdall-lite.mitre.org/)__ for a user-interactive, graphical view of the InSpec results. 

The JSON InSpec results file may also be loaded into a __[full heimdall server](https://github.com/mitre/heimdall)__, allowing for additional functionality such as to store and compare multiple profile runs.

## Authors
* Eugene Aronne - [ejaronne](https://github.com/ejaronne)
* Danny Haynes - [djhaynes](https://github.com/djhaynes)

## Special Thanks
* Alicia Sturtevant - [asturtevant](https://github.com/asturtevant)
* Shivani Karikar - [karikarshivani](https://github.com/karikarshivani)

## Contributing and Getting Help
To report a bug or feature request, please open an [issue](https://github.com/CMSgov/cms-ars-3.1-moderate-aws-rds-oracle-mysql-ee-5.7-cis-overlay/issues/new).

### NOTICE

© 2018-2020 The MITRE Corporation.

Approved for Public Release; Distribution Unlimited. Case Number 18-3678.

### NOTICE 

MITRE hereby grants express written permission to use, reproduce, distribute, modify, and otherwise leverage this software to the extent permitted by the licensed terms provided in the LICENSE.md file included with this project.

### NOTICE  

This software was produced for the U. S. Government under Contract Number HHSM-500-2012-00008I, and is subject to Federal Acquisition Regulation Clause 52.227-14, Rights in Data-General.  

No other use other than that granted to the U. S. Government, or to those acting on behalf of the U. S. Government under that Clause is authorized without the express written permission of The MITRE Corporation.

For further information, please contact The MITRE Corporation, Contracts Management Office, 7515 Colshire Drive, McLean, VA  22102-7539, (703) 983-6000.

### NOTICE 

CIS Benchmarks are published by the Center for Internet Security (CIS), see: https://www.cisecurity.org/.
