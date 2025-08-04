# Deploy RPC with Ansible

## System requirement

- Linux (x86_64)
- docker
- Ethereum and beacon chain with full history
- Ansible 9 and Python 3.12

## Andromeda

### Recommended hardware

1. AWS c5.2xlarge with ipv4 network
2. 750Gb ebs gp3
3. open 30303 tcp/udp for p2p connections

### Steps

1. update `hosts.ini` file, add your remote ip and eth mainnet rpc endpoint there
2. install docker and docker-compose by `ansible-playbook playbooks/docker.yaml`
3. spin up your andromeda rpc by `ansible-playbook playbooks/andromeda.yaml`
4. install docker-autoheal by `ansible-playbook playbooks/autoheal.yaml`

### snapshots

We provided public aws ebs snapshot for you if you need them.

l2geth

snap-01ee4b45082e1c5d6

l1dtl

snap-034b067669773e518

You can get the snapshots on aws **us-east-2** region, and copy them to another region you are using.

## Sepolia

### Recommended hardware

1. AWS c5.xlarge with ipv4 network
2. 100Gb ebs gp3
3. open 30303 tcp/udp for p2p connections

### Steps

1. update `hosts.ini` file, add your remote ip and eth sepolia rpc endpoint there
2. install docker and docker-compose by `ansible-playbook playbooks/docker.yaml`
3. spin up your sepolia rpc by `ansible-playbook playbooks/sepolia.yaml`
4. install docker-autoheal by `ansible-playbook playbooks/autoheal.yaml`

### Snapshot

We provided public aws ebs snapshot for you if you need them.

l2geth

snap-0c9cb31ba6cfbe918

l1dtl

snap-009f6de71936f334b

You can get the snapshots on aws **us-east-1** region, and copy them to another region you are using.

## FAQ

1. How to enable `finalized` param in the `eth_getBlockByNumber` rpc call and so on?

Use `DATA_TRANSPORT_LAYER__SYNC_L1_BATCH=true` env value in your dtl service instead.

By the way, you have to re-sync the data if you don't use it at first.

2. l2geth `panic: Refund counter below zero`

It can happen when you have a new instance without the snapshots.

You can add `--cache.noprefetch=true` argument to your l2geth service

3. How to update the default rpc port

if you don't want to use the default 8545 port, you need to update the `LOCAL_L2_CLIENT_HTTP` env as well.

e.g. if you want to use 8549 instead.

```
RPC_PORT=8549
LOCAL_L2_CLIENT_HTTP=http://localhost:8549
```

4. `Synchronisation failed, retrying err="element not found"`

It means your l1dtl is syncing, you can just wait for its completion.

5. Blob data has already expired

Metis uses blob transactions to publish its state and blocks to Ethereum.

And the blob data are saved in beacon chain nodes, by default, they will be removed after 2 weeks for space saving.

You can use the latest snapshots or use a larger retention period.

If the snapshot has not been updated for too long time, you can file an issue, and we will update it asap.
