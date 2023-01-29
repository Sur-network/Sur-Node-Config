# SUR Blockchain Network



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