

### Ansible Playbook for Node Deployment in a Blockchain Network
## This document describes an Ansible playbook for deploying a node in a blockchain network. The playbook covers the following tasks:

Creating directories for the node.
Removing any existing data.
Copying genesis and toEncode files to the target machine.
Creating Docker containers for the node.
Extracting the node address and enode address.
Generating extra data for the IBFT consensus mechanism.
Configuring the firewall.

## 1. Creating directories for the node
The directories.yml file should contain tasks to create necessary directories on the target machine. This file is included in the main playbook using the include module.

## 2. Removing any existing data
The playbook finds and registers any existing directories in the specified path, and then deletes them. This ensures a clean environment for the new node deployment.

## 3. Copying genesis and toEncode files
The genesis file and the toEncode file are copied from the local machine to the target machine. The copy module is used to set the correct permissions, owner, and group for these files.

## 4. Creating Docker containers for the node
The create_container.yml file should contain tasks to create Docker containers for the node. This file is included in the main playbook using the include module.

## 5. Extracting the node address and enode address
The extract_address.yml and extract_enode_address.yml files contain tasks to extract the node address and enode address, respectively. These files are included in the main playbook using the include module.

## 6. Generating extra data for the IBFT consensus mechanism
The generate_extra_data.yml file contains tasks to generate extra data for the IBFT consensus mechanism. This file is included in the main playbook using the include module.

## 7. Configuring the firewall
The firewall.yml file contains tasks to configure the firewall on the target machine. This file is included in the main playbook using the include module.

To deploy a node using this playbook, run the following command:
    ansible-playbook -i inventory.ini main.yml --tags "setup,remove-data,restart,deploy,address,extradata,firewall"
    
This command will run the playbook with the specified tags, executing the tasks in the appropriate order.
