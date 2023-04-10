#Title: Deploying a Custom Hyperledger Sur Node Using Ansible Playbook

##Introduction:
In this document, we will outline the process for deploying a custom Hyperledger Sur node using Ansible playbook. This guide will help you set up your node correctly and join the network with the appropriate configuration.

###Prerequisites:

- Ansible installed on your local machine
- A hosts.yml file containing the target hosts
- A main.yml file containing the Ansible playbook
###Deployment Steps:

1. Run the Ansible playbook with the inventory file (hosts.yml) and the main.yml playbook:
- ansible-playbook -i hosts.yml main.yml -t "setup, deploy"
The "-t" option specifies the tags "setup" and "deploy" to execute only the tasks related to setup and deployment. This command will set up the environment and deploy the application code to the specified hosts.

2. Change the coinbase address of the container to the address of the node. This is important because the coinbase address is the first address that will receive rewards for mining blocks on the network. By changing it to the address of the node, the node owner will receive these rewards.

3. Change the address in the toEncode file inside /files/toEncode.json (only important for the initial node). The toEncode file contains information about the node's identity, including its address. By updating the address, the node will be identified correctly on the network.

4. Run the Ansible playbook again with the "remove-data" tag in addition to the "setup" and "deploy" tags (only important for the initial node):

- ansible-playbook -i hosts.yml main.yml -t "remove-data, setup, deploy"

By removing the data, the node will be reset to its initial state.

5. Execute the following command inside the Docker container (only important for the initial node):

- besu rlp encode --from=externalVolume/toEncode.json type=IBFT_EXTRA_DATA

This command will encode the data in the toEncode file and generate new extra data.

6. Copy the new extra data to the genesis file (only important for the initial node). The genesis file contains information about the network's initial state. By updating it with the new extra data, the node will be recognized correctly on the network.

7. Run the Ansible playbook again with the "remove-data", "setup", and "deploy" tags:
- ansible-playbook -i hosts.yml main.yml -t "remove-data, setup, deploy"

This step is necessary to restart the node with the updated configuration.

##Conclusion:
In this document, we outlined the process for deploying a custom Hyperledger Sur node using Ansible playbook. By following these steps, you can set up your node correctly and join the network with the appropriate configuration.