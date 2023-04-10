

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


- ansbile-playbook -i hosts.yml main.yml -t "setup, deploy"
- Change the coinbase address of the container to the address of the Node
- Change the address in toEncode file inside /files/toEncode.json (only important for initial node) -
ansbile-playbook -i hosts.yml main.yml -t "remove-data ,setup, deploy" (only important for initial node)
- inside the docker container execute "besu rlp encode --from=externalVolume/toEncode.json
type=IBFT_EXTRA_DATA" (only important for initial node) and copy the new extra data to the genesis
file (only important for initial node)
- ansbile-playbook -i hosts.yml main.yml -t "remove-data ,setup, deploy"

These steps outline the process for deploying a custom Hyperledger Sur node using Ansible playbook.
The first step is to run the Ansible playbook with the inventory file hosts.yml and main.yml playbook.
The "-t" option specifies the tags "setup" and "deploy" to execute only the tasks related to setup and
deployment. This command will set up the environment and deploy the application code to the specified
hosts.
The next step is to change the coinbase address of the container to the address of the node. This is
important because the coinbase address is the first address that will receive rewards for mining blocks
on the network. By changing it to the address of the node, the node owner will receive these rewards.
The third step is to change the address in toEncode file inside /files/toEncode.json. This step is only
important for the initial node. The toEncode file contains information about the node&#39;s identity,
including its address. By updating the address, the node will be identified correctly on the network.
The fourth step is to run the Ansible playbook again, but this time with the "remove-data" tag in
addition to the "setup" and "deploy" tags. This step is only important for the initial node. By removing
the data, the node will be reset to its initial state.
The fifth step is to execute "besu rlp encode --from=externalVolume/toEncode.json
type=IBFT_EXTRA_DATA" inside the docker container. This command will encode the data in the
toEncode file and generate new extra data. This step is only important for the initial node.
The final step is to copy the new extra data to the genesis file. The genesis file contains information
about the network&#39;s initial state. By updating it with the new extra data, the node will be recognized
correctly on the network.
The final step is to run the Ansible playbook again with the "remove-data", "setup", and "deploy" tags.
This step is necessary to restart the node with the updated configuration.
In summary, these steps outline the process of deploying a custom Hyperledger Sur node using Ansible
playbook. It involves updating various configuration files and running the playbook multiple times with
different tags to ensure the node is set up correctly.