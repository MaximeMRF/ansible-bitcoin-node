# Ansible Role to deploy Bitcoin Knots / Core

Installs and configures **Bitcoin Knots** or **Bitcoin Core** on Debian-based servers.

## Features

- **Multi-Flavor**: Supports Bitcoin Knots and Bitcoin Core seamlessly.
- **Versioning**: Clean upgrade/downgrade system, you just specify the version.
- **Flexible**: Use the `bitcoin_config` dictionary to add ANY `bitcoin.conf` parameter without changing the role code.

## Requirements

- Ansible 2.9+
- Debian 13 or compatible OS (it can work on others but is untested).

## Installation

Install via Ansible Galaxy:

```bash
ansible-galaxy role install MaximeMRF.ansible-bitcoin-node
```

## Role Variables

See `defaults/main.yml` for full list. Key variables:

| Variable | Default | Description |
|----------|---------|-------------|
| `bitcoin_variant` | `knots` | Choose between `core` or `knots` |
| `bitcoin_version` | `29.2.knots20251110` | Version to install |
| `bitcoin_architecture` | `x86_64` | Architecture to install (e.g. `x86_64`, `arm64`) |
| `bitcoin_enable_indexes` | `true` | Enable or disable indexes |
| `bitcoin_config` | `{}` | Dict for extra config (see example) |

Variables may be changed in a new version of the role. I'm not sure which to put as defaults.

## Example Playbook

Create an inventory file `hosts.yaml`:

```yaml
all:
  children:
    bitcoin_nodes:
      vars:
        ansible_user: "debian"
      hosts:
        node-btc-01:
          ansible_host: 192.168.0.10
          bitcoin_variant: "knots"
          bitcoin_version: "29.2.knots20251110"
          # optional, default is "x86_64"
          bitcoin_architecture: "x86_64"
          bitcoin_enable_indexes: false
          bitcoin_config:
            uacomment: "MyKnotsNode"
            server: 1
            listen: 1
            logips: 1
            bind: "0.0.0.0"
            rpcbind: "0.0.0.0"
            rpcallowip: "0.0.0.0/0"
            rpcuser: "bitcoinrpc"
            rpcpassword: "myverysecurepassword"
            prune: 4096
            dbcache: 4096
            maxmempool: 300
            zmqpubrawblock: "tcp://0.0.0.0:28332"
            zmqpubrawtx: "tcp://0.0.0.0:28333"
        node-btc-02:
          ansible_host: 192.168.0.20
          bitcoin_variant: "core"
          bitcoin_version: "29.2"
          bitcoin_enable_indexes: false
          bitcoin_config:
            uacomment: "MyCoreNode"
            server: 1
            listen: 1
            logips: 1
            bind: "0.0.0.0"
            rpcbind: "0.0.0.0"
            rpcallowip: "0.0.0.0/0"
            rpcuser: "bitcoinrpc"
            rpcpassword: "myverysecurepassword"
            prune: 4096
            dbcache: 4096
            maxmempool: 300
            zmqpubrawblock: "tcp://0.0.0.0:28332"
            zmqpubrawtx: "tcp://0.0.0.0:28333"
```

Create a playbook file `playbook.yaml`:

```yaml
# playbook.yaml
---
- name: Deploy Bitcoin Node
  hosts: bitcoin_nodes
  become: yes

  roles:
    - role: MaximeMRF.ansible-bitcoin-node
```

Then run:

```bash
ansible-playbook -i hosts.yaml playbook.yaml
```

## License
MIT