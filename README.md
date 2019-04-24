# cms-ars-3.1-moderate-aws-rds-oracle-mysql-ee-5.7-cis-overlay
InSpec profile overlay to validate the secure configuration of AWS RDS Oracle MySQL EE 5.7 against [CIS's](https://www.cisecurity.org/cis-benchmarks/) Oracle MySQL EE 5.7 Benchmark 1.0.0 tailored for [CMS ARS 3.1](https://www.cms.gov/Research-Statistics-Data-and-Systems/CMS-Information-Technology/InformationSecurity/Info-Security-Library-Items/ARS-31-Publication.html) for CMS systems categorized as Moderate. 

## Getting Started  
It is intended and recommended that InSpec and this profile overlay be run from a __"runner"__ host (such as a DevOps orchestration server, an administrative management system, or a developer's workstation/laptop) against the target remotely over __ssh__.

__For the best security of the runner, always install on the runner the _latest version_ of InSpec and supporting Ruby language components.__ 

Latest versions and installation options are available at the [InSpec](http://inspec.io/) site.

The following attributes must be configured in an attributes file for the profile to run correctly. More information about InSpec attributes can be found in the [InSpec Profile Documentation](https://www.inspec.io/docs/reference/profiles/).
 
```
# description: 'username MySQL DB Server'
user: 'root'

# description: 'password MySQL DB Server'
password: 'P@ssw0rd1'

# description: 'hostname MySQL DB Server'
host: 'localhost'

# description: 'port MySQL DB Server'
port: 3306

# description: 'List of MySQL database users'
mysql_users: ['root']   

# description: 'Set to true if the MySQL server has a slave configured'
is_mysql_server_slave_configured: true

# description: 'List of MySQL administrative users'
mysql_administrative_users: ['root'] 

# description: 'List of MySQL users allows to modify or create data structures'
mysql_users_allowed_modify_or_create: ['root'] 
```

## Running This Overlay
When the __"runner"__ host uses this profile overlay for the first time, follow these steps: 

```
mkdir profiles
cd profiles
git clone https://github.cms.gov/ispg-dev/cms-ars-3.1-moderate-aws-rds-oracle-mysql-ee-5.7-cis-overlay.git
git clone https://github.com/mitre/aws-rds-oracle-mysql-ee-5.7-cis-baseline.git
cd cms-ars-3.1-moderate-aws-rds-oracle-mysql-ee-5.7-cis-overlay
bundle install
cd ..
inspec exec cms-ars-3.1-moderate-oracle-aws-rds-mysql-ee-5.7-cis-overlay --attrs=<path_to_your_attributes_file/name_of_your_attributes_file.yml> --target=ssh://<your_target_host_name_or_ip_address> --user=<target_account_with_administrative_privileges> --password=<password_for_target_account> --reporter=cli json:<path_to_your_output_file/name_of_your_output_file.json> 
```

For every successive run, follow these steps to always have the latest version of this overlay and dependent profiles:

```
cd profiles/aws-rds-oracle-mysql-ee-5.7-cis-overlay
git pull
cd ../cms-ars-3.1-moderate-aws-rds-oracle-mysql-ee-5.7-cis-overlay
git pull
bundle install
cd ..
inspec exec cms-ars-3.1-moderate-aws-rds-oracle-mysql-ee-5.7-cis-overlay --attrs=<path_to_your_attributes_file/name_of_your_attributes_file.yml> --target=ssh://<your_target_host_name_or_ip_address> --user=<target_account_with_administrative_privileges> --password=<password_for_target_account> --reporter=cli json:<path_to_your_output_file/name_of_your_output_file.json> 
```

## Viewing the JSON Results

The JSON results output file can be loaded into __[heimdall-lite](https://mitre.github.io/heimdall-lite/)__ for a user-interactive, graphical view of the InSpec results. 

The JSON InSpec results file may also be loaded into a __[full heimdall server](https://github.com/mitre/heimdall)__, allowing for additional functionality such as to store and compare multiple profile runs.

## Authors
* Eugene Aronne
* Danny Haynes

## Special Thanks
* Alicia Sturtevant

## Getting Help
To report a bug or feature request, please open an [issue](https://github.cms.gov/ispg-dev/ccms-ars-3.1-moderate-aws-rds-oracle-mysql-ee-5.7-cis-overlay/issues/new).

## License
This is licensed under the [Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0) license. 

### NOTICE  

This software was produced for the U. S. Government under Contract Number HHSM-500-2012-00008I, and is subject to Federal Acquisition Regulation Clause 52.227-14, Rights in Data-General.  

No other use other than that granted to the U. S. Government, or to those acting on behalf of the U. S. Government under that Clause is authorized without the express written permission of The MITRE Corporation.

For further information, please contact The MITRE Corporation, Contracts Management Office, 7515 Colshire Drive, McLean, VA  22102-7539, (703) 983-6000.

### NOTICE

* This project is dual-licensed under the terms of the Apache license 2.0 (apache-2.0)
* This project is dual-licensed under the terms of the Creative Commons Attribution Share Alike 4.0 (cc-by-sa-4.0)