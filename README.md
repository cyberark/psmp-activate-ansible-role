# PSMP-Activate Ansible Role
This Ansible Role will activate the CyberArk Privileged Session Manager PSM-SSH against an existing vault on an exisiting PSM-SSH machine that is not activated
Do note that psmp-deploy can be ran prior to this role

## Requirements
------------

- CentOS / RHEL installed on the remote host
- SSH open on port 22
- The workstation running the playbook must have network connectivity to the remote host
- Administrator access to the remote host
- PSM-SSH deployed on the remote machine


### Flow Variables
Variable                         | Required     | Default                                   | Comments
:--------------------------------|:-------------|:------------------------------------------|:---------
psmp_validate_machine            | no           | false                                     | Run the validate machine PSMP phase
psmp_pre_activate                | no           | false                                     | Run the pre activate PSMP phase
psmp_activate                    | no           | false                                     | Run the activation PSMP phase
psmp_post_activate               | no           | false                                     | Run the post activation PSMP phase
psmp_validate_activation         | no           | false                                     | Run the validate activation PSMP phase
psmp_pre_deactivate              | no           | false                                     | Run the pre deactivate PSMP phase
psmp_deactivate                  | no           | false                                     | Run the deactivate PSMP phase
psmp_post_deactivate             | no           | false                                     | Run the post deactivate PSMP phase


### Deployment Variables
Variable                         | Required     | Default                                              | Comments
:--------------------------------|:-------------|:-----------------------------------------------------|:---------
accept_eula                      | yes          | **No**                                               | Accepting EULA condition (Yes/No)
vault_ip                         | yes          | None                                                 | IP of the vault to register to
vault_username                   | yes          | None                                                 | Vault username to be used for the registration, can be either credfile or username/password for the vault
vault_password                   | yes          | None                                                 | Vault password to be used for the registration, can be either credfile or username/password for the vault
credfile_path                    | yes          | None                                                 | Vault credfile to be used for the registration, can be either credfile or username/password for the vault
dr_vault_ip                      | no           | None                                                 | Disaster Recovery vault IP to use
vault_name                       | no           | **PSM SSH Vault**                                    | Name of the vault
vault_port                       | no           | **1858**                                             | Port of the vault
vault_comm_timeout               | no           | **10**                                               | Timeout of PSMP communication to the vault
psmp_create_psmp_env             | no           | **true**                                             | Whether to activate PSMP or not
psmp_create_adbridge_env         | no           | **true**                                             | Whether to activate PSMP ADBridge or not
psmp_app_user_name               | no           | **PSMPApp_<Hostname>**                               | Name of the PSMP app user to use
psmp_gw_user_name                | no           | **PSMPGW_<Hostname>**                                | Name of the PSMP gateway user to use
psmp_adb_user_name               | no           | **PSMP_ADB_<Hostname>**                              | Name of the PSMP ADBridge user to use
psmp_preauth_secured_session     | no           | **false**                                            | Whether to enable preauth secured session for LDAP / Radius connections
psmp_selfsigned_certificates     | no           | **false**                                            | Whether to allow self signed certificate connections via the vault
psmp_delete_credfile             | no           | **false**                                            | Whether to delete the cred file used for the activation, if password is used, the cred file will be deleted
psmp_fetch_activation_logs       | no           | **true**                                             | Whether to fetch the activation logs back to the host, will be fetched to either current logs dir or DEFAULT_LOG_PATH env var

## Dependencies
PSMP Installed on the machine

## Usage
The role consists of a number of different tasks which can be enabled or disabled for the particular
run.

`psmp_validate_params`

This task will validate and init parameters for activation and will also check if PSMP is installed or not

`psmp_validate_machine`

This task will validate that all the binaries and paths for the activation exists on the machine and whether PSMP was already activated or not

`psmp_pre_activate`

This task will prepare the vault ini file for the activation and create the cred file if username and password were given

`psmp_activate`

This task will run the activation for both PSMP and ADBridge

`psmp_post_activate`

This task will run the post activation for both PSMP and ADBridge

`psmp_validate_activation`

This task will validate that the activation was successful, and that PSMP / ADBridge are running properly

`psmp_pre_deactivate`

This task will create the cred file for the deactivation if username and password were given

`psmp_deactivate`

This task will perform the deactivation of the PSMP

`psmp_post_deactivate`

This task will run the post deactivation for both PSMP and ADBridge


## Example Playbook
Below is an example of how you can incorporate this role into an Ansible playbook
to call the PSMP Activate role with several parameters:

```
---
- include_role:
    name: psmp-activate
  vars:
    - psmp_validate_machine: true
    - psmp_pre_activate: true
    - psmp_activate: true
    - psmp_validate_activation: true
    - vault_ip: ""
    - vault_username: ""
    - vault_password: ""
    - psmp_enable_preauth_secured_session: true
    - psmp_enable_selfsigned_certificates: true
    - accept_eula: "Yes"
```

## Running the playbook:
For an example of how to incorporate this role into a complete playbook, please see the
**[pas-orchestrator](https://github.com/cyberark/pas-orchestrator)** example.

## License
Apache License, Version 2.0