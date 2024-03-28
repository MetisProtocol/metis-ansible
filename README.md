# Deploy RPC with Ansible

## System requirement

I have tested on following environment, you can use different one but I cannot guarantee that there will be any problems

1. Ubunut 22.04 LTS
2. AWS c5.xlarge with ipv4 network
3. Ansible 9.3.0 and Python 3.12.2
4. 20Gb free disk

## Sepolia

1. update `hosts.ini` file, add your remote ip and eth sepolia rpc endpoint there
2. install docker and docker-compose by `ansible-playbook playbooks/docker.yaml`
3. spin up your sepolia rpc by `ansible-playbook playbooks/sepolia.yaml`
4. install docker-autoheal by `ansible-playbook playbooks/autoheal.yaml`
