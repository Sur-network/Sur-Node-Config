# SUR Blockchain Network

### IF you want a smoother deployment please check the ansible-deployment.md file

## Getting started

To make start up your server you need to install docker, you can either use the docker ansible role or do it in any way that suits you.

## Deploying surnet docker Image

After pulling the surblockchain/sur:latest image you can create a container with the following configurations:

### Volumes to set:

#### path/to/your/bind/address:/opt/sur/externalVolume
* This is where sur stores its data, set this and deploy a container then proceed to the next steps because after the first creation a key is generated under /opt/sur/externalVolume/data/key, if you wish to redeploy the isntance do not delete this file beacuse a new key thus a new address is created, using node ansible role is recommened is this case

### Env vars to set:

#### BESU_BOOTNODES:
'enode://132f01b85efd10e213daa95a92446030dd6103f93c3e0b6ddf92bc082948b3b163e130947e1d3f3bb04fa2c4839002b277b4ce7f5f11be0a0ff3cee2fb52deea@node.surnet.org:30303'
* This variable is a list of bootnodes seperated by "git"

#### BESU_P2P_HOST: 
0.0.0.0
* This is the bind address of the app

#### BESU_MINER_ENABLED:
"true"
* This must be set to "ture" if the instance is miner

#### BESU_MINER_COINBASE:
"0xbc2df8f82ead9b73b10efdb7a42fbaa0c763ff4a"
* This variable is the address of the miner coinbase, you can check your instance address after creating your first instance with this command "besu --data-path=<node data path> public-key export-address --node-private-key-file=/home/me/me_node/myPrivateKey" or you can use the ansible role for node that exctracts the address automatically, then set this variable and redeploy your instance

#### BESU_RPC_HTTP_API: 
ETH,NET,IBFT,ADMIN,MINER
* This is the enabled apis seperated by "," 

### files to copy:
#### genesis.json:
This file is placed under /node/files/genesis.json, copy it in your bind address in a way that its accessible by the container under /opt/sur/externalVolume/genesis.json
* The ansible role does this automatically



### end note:
After setting these vars you can run your instance, keep in mind that to extract the adddress you need to fist have a key generated thus deploy an instance first then proceed to setting the correct values for env vars and redeploy again, for easier deployments you can use the ansible node role and change the env vars in create_container.yml. The role is designed to work with a domain (see webserver role) so if like to use IP address to access your node, either map the desired ports by besu 8545-8547/tcp 30303/tcp/udp or use host network mode.



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
important for the initial node. The toEncode file contains information about the node's identity,
including its address. By updating the address, the node will be identified correctly on the network.
The fourth step is to run the Ansible playbook again, but this time with the "remove-data" tag in
addition to the "setup" and "deploy" tags. This step is only important for the initial node. By removing
the data, the node will be reset to its initial state.
The fifth step is to execute "besu rlp encode --from=externalVolume/toEncode.json
type=IBFT_EXTRA_DATA" inside the docker container. This command will encode the data in the
toEncode file and generate new extra data. This step is only important for the initial node.
The final step is to copy the new extra data to the genesis file. The genesis file contains information
about the network's initial state. By updating it with the new extra data, the node will be recognized
correctly on the network.
The final step is to run the Ansible playbook again with the "remove-data", "setup", and "deploy" tags.
This step is necessary to restart the node with the updated configuration.
In summary, these steps outline the process of deploying a custom Hyperledger Sur node using Ansible
playbook. It involves updating various configuration files and running the playbook multiple times with
different tags to ensure the node is set up correctly.
