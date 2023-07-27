# THIS ROLE IS DEPRECATED AND NO LONGER DEVELOPED

The successor is now developed as part of the [community.sap_install](https://github.com/sap-linuxlab/community.sap_install) collection.

The new name is community.sap_install.sap_hana_install

# sap-hana-deployment ![Ansible Lint](https://github.com/redhat-sap/sap-hana-deployment/workflows/Ansible%20Lint/badge.svg?branch=master) ![Ansible Galaxy Import](https://github.com/redhat-sap/sap-hana-deployment/workflows/Ansible%20Galaxy%20Import/badge.svg?branch=master)

This role installs SAP HANA on a RHEL 7.x or 8.x system and applies a permament HANA License.

## Requirements

This role is intended to be used on a RHEL system on which SAP HANA software is to be installed.
So your system needs to be installed with the RHEL package groups required for SAP HANA, properly registered, and prepared for HANA installation.

It needs access to the software repositories required to install SAP HANA (see also: [How to subscribe SAP HANA systems to the Update Services for SAP Solutions](https://access.redhat.com/solutions/3075991))

You can use the [redhat_sap.sap_rhsm](https://galaxy.ansible.com/redhat_sap/sap_rhsm) Galaxy Role to automate this process.

To install SAP software on Red Hat Enterprise Linux you need some additional packages which come in a special repository.
To get this repository you need to have one of the following products:

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

For installing the required software and for configuring required system settings for SAP HANA, use the roles sap-preconfigure and
sap-hana-preconfigure from the RHEL System Roles for SAP package or the roles sap.rhel.preconfigure and sap.rhel.hana-preconfigure from
the sap.rhel collection on Red Hat Automation Hub or sap.linux.preconfigure and sap.linux.hana-preconfigure from the sap.linux collection
on Galaxy.

It is also important that your disks are setup according to the [SAP storage requirements for SAP HANA](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). This [BLOG](https://blogs.sap.com/2017/03/07/the-ultimate-guide-to-effective-sizing-of-sap-hana/) is also quite helpful when sizing HANA systems.

## Actions performed by the role
### Get a valid user and group id to be used for the SAP HANA installation
In case no user and group ID is provided by using variables sap_hana_deployment_hana_userid or sap_hana_deployment_hana_groupid,
a user and group ID is chosen according to certain rules.
### Install SAP HANA
#### 1\. Check or Set Permissions of relevant SAP directories
  The role first checks of sets the permissions of SAP directories /hana/shared, /hana/data, /hana/log, and /usr/sap. The role variables
  sap_hana_deployment_directories_permissions and sap_hana_deployment_set_permissions are used for this purpose.

#### 2\. Make the SAP HANA installation files available
  The SAP HANA installation files have to be made available on the managed node before the installation can start. The role can:

  - Use an existing HANA installation directory on the managed node

    In this case, role variable sap_hana_installdir has to be set to the directory in which the hdblcm program is located.

  - Use a SAP HANA installation bundle file (SAR or ZIP) on the managed node, from the control node, or from a third node.

    In this case, the following information has to be provided:

    - The location on the managed node to where the SAP HANA installation bundle file is to be extracted (role variable
      sap_hana_deployment_hana_extract_path).

    - The name and the existing or desired localtion of the SAP HANA installation bundle file (role variables
      sap_hana_deployment_bundle_file_name and sap_hana_deployment_bundle_path_mn). In case the installation bundle file is of type SAR,
      the file name of the SAPCAR executable and its exising or desired location on the managed node has to be specified as well
      (role variables sap_hana_deployment_sapcar_file_name and sap_hana_deployment_sapcar_path_mn).

    - Further information about where the SAP HANA installation bundle (and SAPCAR file, if applicable) are located on the control node
      or on a third node, if these files are available on the control node or on a third node.

#### 3\. Run the SAP HANA installation
  Once the SAP HANA installation files are available on the managed node, the installation is started on the managed node.
  By specifying a valid argument to variable `sap_hana_deployment_addhosts`, one or more SAP HANA hosts are added after the installation
  on the first node has completed, meaning that the role will create a SAP HANA scale-out system.

  If the variable `sap_hana_deployment_install_primary` is set to the value `n`, then instead of installing a fresh SAP HANA system,
  additional hosts are added to an existing SAP HANA installation instead, using the argument to variable
  `sap_hana_deployment_addhosts`.

#### 4\. Apply the SAP HANA license
  After a fresh SAP HANA installation has completed, the SAP HANA license can be applied.

## Role Variables

| variable |             info             | required? |
|:--------:|:----------------------------:|:---------:|
|sap_hana_deployment_directories_permissions| Permissions for /hana/shared, /hana/data, /hana/log, and /usr/sap. | Yes|
|sap_hana_deployment_set_permissions| Set or verify permissions for /hana/shared, /hana/data, /hana/log, and /usr/sap. If set to `yes`, permissions will be set. If set to `no`, permissions will be verified and the role will abort if one of the permissions is not set correctly. | Yes. Default is `no`.|
|sap_hana_deployment_install_primary|Whether you want to perform a fresh SAP HANA installation or add more hosts to an existing SAP HANA installation. The default is `y`.|yes|
|sap_hana_installdir|SAP HANA directory in which hdblcm is located |No, if the location of a SAP HANA installation bundle file is specified using some of the variables below|
|sap_hana_deployment_hana_extraction_path|Directory path on the managed node to where the SAP HANA installation bundle SAR or ZIP file is to be extracted|yes, if `sap_hana_installdir` is not defined|
|sap_hana_deployment_bundle_is_on_managed_node|Define if the SAP HANA installation bundle file is available on the managed node|yes, if `sap_hana_installdir` is not defined|
|sap_hana_deployment_bundle_file_name|File name of the SAP HANA installation bundle SAR or ZIP file|yes, if `sap_hana_installdir` is not defined|
|sap_hana_deployment_bundle_path_mn|Directory path on the managed node where the SAP HANA installation bundle SAR or ZIP file is located|yes, if `sap_hana_installdir` is not defined|
|sap_hana_deployment_sapcar_file_name|File name of the SAPCAR executable|yes, if `sap_hana_installdir` is not defined and if the HANA installation bundle file type is "SAR"|
|sap_hana_deployment_sapcar_path_mn|Directory path of the SAPCAR executable on the managed node|yes, if `sap_hana_installdir` is not defined and if the HANA installation bundle file type is "SAR"|
|sap_hana_deployment_bundle_is_on_control_node|Define if the SAP HANA installation bundle file is available on the control node|yes, if `sap_hana_installdir` is not defined|
|sap_hana_deployment_bundle_path_cn|Directory path on the control node where the SAP HANA installation bundle SAR or ZIP file is located|yes, if `sap_hana_installdir` is not defined and if sap_hana_deployment_bundle_is_on_control_node is set to `yes`|
|sap_hana_deployment_sapcar_path_cn|Directory path on the control node where the SAPCAR executable is located|yes, if `sap_hana_installdir` is not defined and if sap_hana_deployment_bundle_is_on_control_node is set to `yes` and if the HANA installation bundle file type is "SAR" |
|sap_hana_deployment_sap_software_remote_location|user, hostname, and directory to specify in which directory the SAP HANA installation bundle SAR or ZIP file is located on a third node|yes, if `sap_hana_installdir` is not defined and if sap_hana_deployment_bundle_is_on_managed_node is set to `no` and if sap_hana_deployment_bundle_is_on_control_node is set to `no`|
|sap_hana_deployment_hdblcm_extraargs|Define extra commandline arguments to hdblcm, such as `--ignore=check1[,check2]` | No |
|sap_hana_deployment_deploy_hostagent|Whatever you want to deploy SAP HostAgent or not|no, defaulted to `n` value|
|sap_hana_deployment_use_master_password|Use single master password for all users, created during installation|no, defaulted to `n` value|
|sap_hana_deployment_common_master_password|Common password for both OS users and DB Administrator user (SYSTEM)|no, only if sap_hana_deployment_use_master_password is `y`|
|sap_hana_deployment_root_password|Root User Password|yes|
|sap_hana_deployment_sapadm_password|SAP Host Agent User (sapadm) Password|no, will take the value from `sap_hana_deployment_common_master_password` when `sap_hana_deployment_use_master_password` is `y`|
|sap_hana_deployment_sidadm_password|Password for user \<sid>adm|no, will take the value from `sap_hana_deployment_common_master_password` when `sap_hana_deployment_use_master_password` is `y`|
|sap_hana_deployment_hana_db_system_password|Database User (SYSTEM) Password|no, will take the value from `sap_hana_deployment_common_master_password` when `sap_hana_deployment_use_master_password` is `y`|
|sap_hana_deployment_ase_user_password|SAP ASE Administrator Password|no|
|sap_hana_deployment_xs_org_password|XS Advanced Admin User Password|Only if `sap_hana_deployment_xs_install` is `y`|
|sap_hana_deployment_lss_user_password|Local Secure Store User Password|no|
|sap_hana_deployment_lss_backup_password|Local Secure Store Auto Backup Password|no|
|sap_hana_deployment_hana_install_path|Installation Path for SAP HANA|no, defaulted to `/hana/shared` value|
|sap_hana_deployment_hana_sid|SAP HANA System ID|yes|
|sap_hana_deployment_hana_instance_number|Instance Number|yes - **note the required double quotes while adding the variable to your inventory so this is interpreted as a string** |
|sap_hana_deployment_hana_env_type|System Usage, Valid values: production, test, development or custom|no, defaulted to `production` value|
|sap_hana_deployment_hana_mem_restrict|Restrict maximum memory allocation|no, defaulted to `y` value|
|sap_hana_deployment_hana_max_mem|Maximum Memory Allocation in MB|yes (unless `sap_hana_deployment_hana_mem_restrict` value is `n`)|
|sap_hana_deployment_certificates_hostmap|Hostname used for generation of self-signed SSL certificates for the SAP Host Agent|no|
|sap_hana_deployment_hana_userid|System Administrator User ID (sidadm)|no, defaulted to next available user ID|
|sap_hana_deployment_hana_groupid|ID of User Group|no, defaulted next available group ID|
|sap_hana_deployment_system_restart|Restart system after machine reboot|no, defaulted to `n`|
|sap_hana_deployment_create_initial_tenant|Create an initial tenant with the SAP HANA installation|yes, defaulted to `y`|
|sap_hana_deployment_hostname|Hostname for the installation (e.g.if a virtual name is to be used)|yes, defaulted to the physical hostname|
|sap_hana_deployment_addhosts|a valid 'hostname:role=...,hostname:role=...' string as per SAP HANA Server Installation and Updated Guide. Example: 'host02:role=worker:workergroup=wg01:group=g01,host03:role=worker'|Only for HANA scale-out installation or for adding additional hosts to an existing HANA installation|
|sap_hana_deployment_xs_install|Install XS Advanced in the default tenant database|no, defaulted to `n`|
|sap_hana_deployment_xs_path|XS Advanced App Working Path|Only if `sap_hana_deployment_xs_install` is `y`|
|sap_hana_deployment_xs_orgname|Organization Name For Space "SAP"|Only if `sap_hana_deployment_xs_install` is `y`, defaulted to `orgname`|
|sap_hana_deployment_xs_org_user|XS Advanced Admin User|Only if `sap_hana_deployment_xs_install` is `y`, defaulted to `XSA_ADMIN`|
|sap_hana_deployment_xs_prod_space|Customer Space Name|Only if `sap_hana_deployment_xs_install` is `y`, defaulted to `PROD`|
|sap_hana_deployment_xs_routing_mode|Routing Mode (Valid values: ports and hostnames)|Only if `sap_hana_deployment_xs_install` is `y`, defaulted to `ports`|
|sap_hana_deployment_xs_domain_name|XS Advanced Domain Name|Only if `sap_hana_deployment_xs_install` is `y`|
|sap_hana_deployment_xs_sap_space_user|XS Advanced SAP Space OS User ID|Only if `sap_hana_deployment_xs_install` is `y`|
|sap_hana_deployment_xs_customer_space_user|XS Advanced Customer Space OS User ID|Only if `sap_hana_deployment_xs_install` is `y`|
|sap_hana_deployment_xs_components|XS Advanced Components|Only if `sap_hana_deployment_xs_install` is `y`|
|sap_hana_deployment_xs_components_nostart|Do not start the selected XS Advanced components after installation|Only if `sap_hana_deployment_xs_install` is `y`, defaulted to `none`|
|sap_hana_deployment_lss_user|Local Secure Store User ID|no|
|sap_hana_deployment_lss_group|Local Secure Store User Group ID|no|
|sap_hana_deployment_apply_license_only|Whether to apply a license file only|no, defaulted to 'false'|
|sap_hana_deployment_apply_license|Whether to apply a license file after the SAP HANA installation|no, defaulted to 'false'|
|sap_hana_deployment_license_path|directory path on the managed node where the HANA DB license file located|no, required only if `sap_hana_deployment_apply_license` true|
|sap_hana_deployment_license_file_name|HANA DB license file name|no, required only if `sap_hana_deployment_apply_license` true|

## HANA Deployment and HANA License

The role supports two different scenarios: SAP HANA deployment in a RHEL system, with or without applying a SAP HANA DB license,
and applying a SAP HANA DB license in an existing deployment only.

## Dependencies

Before using this role, ensure that your system has been configured properly to run SAP applications and SAP HANA.

You can use the supported roles `sap-preconfigure` and `sap-hana-preconfigure` on RHEL 8 control nodes, which are part of the RHEL for SAP Solutions Subscription.

The upstream version of these role can be found [here](https://github.com/linux-system-roles/sap-preconfigure) and [here](https://github.com/linux-system-roles/sap-hana-preconfigure)

## Example Playbook

```yaml
    - hosts: servers
      roles:
      - role: sap-hana-deployment
```

## Example Inventory for an initial SAP HANA installation - HANA software is already extracted on the managed node

```yaml
sap_hana_installdir: /data/sap-install/SAP_HANA_DATABASE
sap_hana_deployment_hana_install_path: '/hana/shared'
sap_hana_deployment_root_password: "R3dh4t123"
sap_hana_deployment_sapadm_password: "R3dh4t123"
sap_hana_deployment_sidadm_password: "R3dh4t123"
sap_hana_deployment_hana_sid: RHE
sap_hana_deployment_hana_instance_number: "01"
sap_hana_deployment_hana_env_type: development
sap_hana_deployment_hana_mem_restrict: 'n'
sap_hana_deployment_hana_db_system_password: "R3dh4t123"
sap_hana_deployment_ase_user_password: "R3dh4t123"
sap_hana_deployment_apply_license: true
sap_hana_deployment_license_path: /data/sap-license
sap_hana_deployment_license_file_name: RHE.txt
```

## Example Inventory for an initial SAP HANA scale-out installation - HANA software SAR file is available on the control node

```yaml
sap_hana_deployment_bundle_is_on_managed_node: no
sap_hana_deployment_bundle_is_on_control_node: yes
sap_hana_deployment_bundle_path_mn: /data/sap-download
sap_hana_deployment_bundle_path_cn: /data/sap-download
sap_hana_deployment_bundle_file_name: IMDB_SERVER20_045_0-80002031.SAR
sap_hana_deployment_sapcar_path_mn: /usr/local/bin
sap_hana_deployment_sapcar_path_cn: /data/sap-download
sap_hana_deployment_sapcar_file_name: SAPCAR_1211-80000935.EXE
sap_hana_deployment_hana_extraction_path: /data/sap-install
sap_hana_deployment_hana_install_path: '/hana/shared'
sap_hana_deployment_root_password: "R3dh4t123"
sap_hana_deployment_sapadm_password: "R3dh4t123"
sap_hana_deployment_sidadm_password: "R3dh4t123"
sap_hana_deployment_hana_sid: RHE
sap_hana_deployment_hana_instance_number: "01"
sap_hana_deployment_hana_env_type: development
sap_hana_deployment_hana_mem_restrict: 'n'
sap_hana_deployment_hana_db_system_password: "R3dh4t123"
sap_hana_deployment_ase_user_password: "R3dh4t123"
sap_hana_deployment_addhosts: 'host02:role=worker:workergroup=wg01:group=g01,host03:role=worker'
sap_hana_deployment_apply_license: true
sap_hana_deployment_license_path: /data/sap-license
sap_hana_deployment_license_file_name: RHE.txt
```

## Example Inventory for adding a new host to an existing SAP HANA installation

```yaml
sap_hana_deployment_install_primary: no
sap_hana_deployment_hana_install_path: '/hana/shared'
sap_hana_deployment_root_password: "R3dh4t123"
sap_hana_deployment_sapadm_password: "R3dh4t123"
sap_hana_deployment_sidadm_password: "R3dh4t123"
sap_hana_deployment_hana_sid: RHE
sap_hana_deployment_hana_instance_number: "01"
sap_hana_deployment_hana_db_system_password: "R3dh4t123"
sap_hana_deployment_addhosts: 'host04:role=standby'
```

## License

Apache License 2.0

## Author Information

Red Hat SAP Community of Practice
