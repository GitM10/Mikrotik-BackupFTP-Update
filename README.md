# Mikrotik-BackupFTP-Update
This Ansible playbook is designed to automate the backup process of MikroTik RouterOS (ROS) system and configuration files, upload them to an FTP server, and conditionally update the RouterOS software based on the success of the FTP uploads.

## Requirements
- Ansible 2.9 or higher: This playbook has been developed and tested using Ansible 2.9. It may work on newer versions but compatibility is not guaranteed.
- MikroTik Router with RouterOS: Access to a MikroTik Router running RouterOS is necessary for performing backups and updates.
- FTP Server: An operational FTP server is required to store the backup files. Ensure that the RouterOS device has network access to the FTP server.
- Ansible Collections:
 - community.routeros: This collection includes the modules necessary for interacting with RouterOS. Install it using ansible-galaxy collection install community.routeros.
 - ansible.builtin: This includes core modules such as set_fact and pause, which are part of the default Ansible installation.
## Configuration
### Variables File
Vault Variable File Path: Specify the path to your Ansible Vault file that contains sensitive information such as FTP credentials. This path is to be replaced in the vars_files section.
### Variables
- ansible_connection: Set to network_cli to use the network CLI connection type.
- ansible_network_os: Set to routeros for MikroTik RouterOS.
- ansible_user: The username for connecting to the RouterOS device. Default is admin.
- ftp_server: The IP address or hostname of the FTP server.
- ftp_user: Username for FTP server authentication.
- ftp_password: Password for FTP server authentication.
- ftp_directory: The directory on the FTP server where backup files will be stored.
- ftp_upload_successful: A boolean flag to track the success of FTP uploads. Initialized to true.
## Tasks
- 1 Set Fact for Current Time: Captures the current time to append to backup filenames.
- 2 Starting ROS System Backup: Executes a system backup on the RouterOS device.
- 3 Starting ROS Configuration Backup: Exports the RouterOS configuration to a file.
- 4 Wait for Backup to be Ready: Pauses execution for 10 seconds to ensure backup files are ready.
- 5 Upload ROS System Backup to FTP Server: Uploads the system backup file to the configured FTP server.
- 6 Check FTP Upload System Backup Success: Sets the ftp_upload_successful flag to false if the system backup upload fails.
- 7 Upload ROS Configuration Backup to FTP Server: Uploads the configuration backup file to the FTP server.
- 8 Check FTP Upload Config Backup Success: Sets the ftp_upload_successful flag to false if the configuration backup upload fails.
- 9 Update RouterOS: Conditionally executes a RouterOS update if both backups were successfully uploaded to the FTP server.
## Usage
- 1 Replace placeholder values (marked with -----------------) with your actual data.
- 2 Run the playbook using the ansible-playbook command, specifying your inventory and playbook file name.
## Example Command
ansible-playbook --ask-vault-pass your_playbook_name.yml

## Notes
This playbook uses the community.routeros.command module to interact with MikroTik RouterOS devices. Ensure this collection is installed and up to date.
The playbook includes a step to conditionally update RouterOS based on the success of the FTP uploads. Ensure this operation is suitable for your environment as updates can cause devices to reboot and may lead to service disruption.
Adjust the FTP credentials and target directory according to your setup.
The playbook does not gather facts to speed up execution. If device information is needed for other tasks, enable gather_facts.
For more details on Ansible playbooks for MikroTik RouterOS, visit the Ansible documentation.
