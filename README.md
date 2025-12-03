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
| `bitcoin_config` | `{}` | Dict for extra config (see example) |

Variables may be changed in a new version of the role. I'm not sure which to put as defaults.

## Example Playbook

```yaml
all:
  children:
    bitcoin_nodes:
      hosts:
        node-btc-01:
          ansible_host: 192.168.0.10
          bitcoin_variant: "knots"
          bitcoin_version: "29.2.knots20251110"
          bitcoin_rpc_password: "supersecurepassword"
          bitcoin_enable_indexes: false
          bitcoin_prune_size: 4096
          bitcoin_dbcache: 4096
          bitcoin_config:
            uacomment: "MyKnotsNode"
        node-btc-02:
          ansible_host: 192.168.0.20
          bitcoin_variant: "core"
          bitcoin_version: "29.2"
          bitcoin_rpc_password: "supersecurepassword"
          bitcoin_enable_indexes: false
          bitcoin_prune_size: 4096
          bitcoin_dbcache: 4096
          bitcoin_config:
            uacomment: "MyCoreNode"
```
