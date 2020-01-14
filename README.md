# sap-hana-deployment [![Build Status](https://travis-ci.com/redhat-sap/sap-hana-deployment.svg?branch=master)](https://travis-ci.com/redhat-sap/sap-hana-deployment)

This role installs SAP HANA on a RHEL 7.x or 8.x system and applies a permament HANA License.

## Requirements

This role is intended to use on a RHEL system that gets SAP software.
So your system needs to be installed with at least the RHEL core packages, properly registered and prepared for HANA or Netweaver installation.

It needs access to the software repositories required to install SAP HANA (see also: [How to subscribe SAP HANA systems to the Update Services for SAP Solutions](https://access.redhat.com/solutions/3075991))

You can use the [redhat_sap.sap_rhsm](https://galaxy.ansible.com/redhat_sap/sap_rhsm) Galaxy Role to automate this process

To install SAP software on Red Hat Enterprise Linux you need some additional packages which come in a special repository. To get this repository you need to have one
of the following products:

- [RHEL for SAP Solutions](https://access.redhat.com/solutions/3082481) (premium, standard, developer Edition)
- [RHEL for Business Partner NFRs](https://partnercenter.redhat.com/NFRPageLayout)

[Click here](https://developers.redhat.com/products/sap/download/) to achieve a personal developer edition of RHEL for SAP Solutions. Please register as a developer and download the developer edition.

- [Registration Link](http://developers.redhat.com/register) :
  Here you can either register a new personal account or link it to an already existing **personal** Red Hat Network account.
- [Download Link](https://access.redhat.com/downloads/):
  Here you can download the Installation DVD for RHEL with your previously registered account

*NOTE:* This is a regular RHEL installation DVD as RHEL for SAP Solutions is no additional
 product but only a special bundling. The subscription grants you access to the additional
 packages through our content delivery network(CDN) after installation.

It is also important that your disks are setup according to the [SAP storage requirements for SAP HANA](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). This [BLOG](https://blogs.sap.com/2017/03/07/the-ultimate-guide-to-effective-sizing-of-sap-hana/) is also quite helpful when sizing HANA systems.

## Role Variables

| variable | info | required? |
|:--------:|:----:|:---------:|
|bundle_path|Target host directory path where SAP Installation Bundle SAR file is located|yes|
|bundle_sar_file_name|Installation Bundle SAR file name|yes|
|sapcar_path|Target host directory directory path where SAPCAR tool file is located|yes|
|sapcar_file_name|SAPCAR tool file name|yes|
|deploy_hostagent|Whatever you want to deploy SAP HostAgent or not|no, defaulted to `n` value|
|use_master_password|Use single master password for all users, created during installation|no, defaulted to `n` value|
|common_master_password|Common password for both OS users and DB Administrator user (SYSTEM)|no, only if use_master_password is `y`|
|root_password|Root User Password|yes|
|sapadm_password|SAP Host Agent User (sapadm) Password|no, will take the value from `common_master_password` when `use_master_password` is `y`|
|sidadm_password|Password for user \<sid>adm|no, will take the value from `common_master_password` when `use_master_password` is `y`|
|hana_db_system_password|Database User (SYSTEM) Password|no, will take the value from `common_master_password` when `use_master_password` is `y`|
|ase_user_password|SAP ASE Administrator Password|yes|
|xs_org_password|XS Advanced Admin User Password|Only if `xs_install` is `y`|
|lss_user_password|Local Secure Store User Password|yes|
|lss_backup_password|Local Secure Store Auto Backup Password|yes|
|hana_install_path|Installation Path for SAP HANA|no, defaulted to `/hana/shared` value|
|hana_sid|SAP HANA System ID|yes|
|hana_instance_number|Instance Number|yes - **note the required double quotes while adding the variable to your inventory so this is interpreted as a string** |
|hana_env_type|System Usage, Valid values: production, test, development or custom|no, defaulted to `production` value|
|hana_mem_restrict|Restrict maximum memory allocation|no, defaulted to `y` value|
|hana_max_mem|Maximum Memory Allocation in MB|yes (unless `hana_mem_restrict` value is `n`)|
|hana_userid|System Administrator User ID|no, defaulted to next available user ID|
|hana_groupid|ID of User Group|no, defaulted next available group ID|
|system_restart|Restart system after machine reboot|no, defaulted to `n`|
|xs_install|Install XS Advanced in the default tenant database|no, defaulted to `n`|
|xs_path|XS Advanced App Working Path|Only if `xs_install` is `y`|
|xs_orgname|Organization Name For Space "SAP"|Only if `xs_install` is `y`, defaulted to `orgname`|
|xs_org_user|XS Advanced Admin User|Only if `xs_install` is `y`, defaulted to `XSA_ADMIN`|
|xs_prod_space|Customer Space Name|Only if `xs_install` is `y`, defaulted to `PROD`|
|xs_routing_mode|Routing Mode (Valid values: ports and hostnames)|Only if `xs_install` is `y`, defaulted to `ports`|
|xs_domain_name|XS Advanced Domain Name|Only if `xs_install` is `y`|
|xs_sap_space_user|XS Advanced SAP Space OS User ID|Only if `xs_install` is `y`|
|xs_customer_space_user|XS Advanced Customer Space OS User ID|Only if `xs_install` is `y`|
|xs_components|XS Advanced Components|Only if `xs_install` is `y`|
|xs_components_nostart|Do not start the selected XS Advanced components after installation|Only if `xs_install` is `y`, defaulted to `none`|
|lss_user|Local Secure Store User ID|yes|
|lss_group|Local Secure Store User Group ID|yes|
|apply_license|Whether to apply a License File to the deployed HANA instance|no, defaulted to 'false'|
|license_path|Target host directory path where HANA license file located|no, required if `apply_license` true|
|license_file_name|HANA license file name|no, required if `apply_license` true|

## HANA Deploy and HANA Lincese

While using this role 2 different scenarios can be covered. These are SAP HANA deployment in a new RHEL Server and set the HANA DB License in an existing deployment.

In order the role to run the first scenario, SAP HANA deployment in a new RHEL Server, the variable `apply_license` must be `false`.

In order the role to run the second scenario, set the HANA DB License in an existing deployment, the variable `apply_license` must be `true`.

Variables required for both scenarios are the ones specified already.

## Dependencies

Before using this role ensure your system has been configured properly to run SAP applications and HANA.

You can use the supported roles `sap-preconfigure` and `sap-hana-preconfigure` comming with RHEL 7 and 8 with RHEL for SAP Solutions Subscription

The upstream version of these role can be found [here](https://github.com/linux-system-roles/sap-preconfigure) and [here](https://github.com/linux-system-roles/sap-hana-preconfigure)

## Example Playbook

```yaml
    - hosts: servers
      roles:
      - role: sap-hana-deployment
```

## Example Inventory

```yaml
bundle_path: /usr/local/src
bundle_sar_file_name: IMDB_SERVER20_045_0-80002031.SAR
sapcar_path: /usr/local/src
sapcar_file_name: SAPCAR_1311-80000935.EXE
root_password: "mysecretpassword"
sapadm_password: "mysecretpassword"
hana_sid: RHE
hana_instance_number: "01"
hana_env_type: development
hana_mem_restrict: 'n'
hana_master_password: "mysecretpassword"
hana_db_system_password: "mysecretpassword"
lss_user: lsadm
lss_group: lsgrp
lss_user_password: "mysecretpassword"
lss_backup_password: "mysecretpassword"
ase_user_password: "mysecretpassword"
apply_license: true
license_path: /usr/local/src
license_file_name: RHE.txt
```

## License

GPLv3

## Author Information

Red Hat SAP Community of Practice
