---
type: Deployment Procedure
title: "Ansible"
description: "Installing Ansible and the inventory, group_vars, and playbook conventions the CyVerse deployment expects."
tags: [deployment, planning, ansible, automation]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
sources:
  - id: ansible-docs
    resource: https://docs.ansible.com/ansible/latest/
    title: Ansible documentation
    author: team:ansible
---

# How CyVerse uses Ansible

[Ansible](https://www.ansible.com/) is agentless: it runs from your workstation
over `ssh` and needs nothing installed on the servers. CyVerse uses it for
everything from host preparation to deploying the DE service set, driven by tags
on a single `kubernetes.yml` playbook plus a handful of standalone playbooks.

Read the [Ansible documentation](https://docs.ansible.com/ansible/latest/) if you
have not used it before — this document covers only the CyVerse conventions.

# Install

Any of these works; pick whichever your workstation already manages packages
with:

```bash
# pip
pip install --user ansible

# homebrew
brew install ansible

# distribution package
apt-get install ansible
```

## Third-party roles

Some playbooks depend on roles from Ansible Galaxy. From the playbook repository:

```bash
ansible-galaxy install --force -r requirements.yaml
```

# Configuration

CyVerse's variables are structured as YAML hashes, and overriding a single key
inside one requires hash merging rather than replacement:

```bash
export ANSIBLE_HASH_BEHAVIOUR="merge"
```

Set it in your shell profile, or set `hash_behaviour = merge` in a local
`ansible.cfg` — the second is more reliable, because it travels with the
repository instead of with your shell.

# Inventory layout

Inventories are **not** in the public repository, and files matching `*.cfg` in
the inventories directory are gitignored deliberately: an inventory names every
host in the deployment.

Keep yours in a private repository. Start from the example inventory shipped with
the playbooks and fill in the groups; for the two-node pilot shape, see
[cluster](../04-kubernetes/cluster.md#inventory).

## group_vars

The default variables live in `inventories/group_vars/all` in the playbook
repository, with every variable the roles use set to a default. You create your
own `group_vars/all.yml` in your inventory and override only what differs.

The example file is organized **in phases**, and that ordering is meaningful: some
variables cannot be filled in until earlier phases have produced their values.
The clearest case is Keycloak — its eight client secrets do not exist until the
clients have been created in the running Keycloak, so phase 5 variables are filled
in partway through the deployment. Fill in phases 1 through 4 before starting, and
expect to come back.

# Playbook layout

The repository diverges from Ansible's recommended layout in two documented ways:

* **`group_vars` location.** In `inventories/group_vars` rather than beside the
  playbooks, so that inventories carry their own variables.
* **Role-separated playbooks.** Kept in `playbooks/`; what sits in the top level
  of `ansible/` is composite or one-off playbooks. To run a single role, use the
  `single-role.yaml` playbook — its own comments document its use.

# Running playbooks

```bash
# check every host is reachable
ansible -i /path/to/inventory -m ping all

# run a phase by tag
ansible-playbook -i /path/to/inventory --tags prep-nodes kubernetes.yml

# a standalone playbook
ansible-playbook -i /path/to/inventory rabbitmq_configure.yml
```

Useful flags: `-K` to prompt for the `sudo` password, `-u <user>` to override the
remote user, `--check` for a dry run, and `--diff` to see what would change.

# Preparing servers

Modern targets need little preparation beyond what `prep-nodes` does. Two
requirements are worth knowing about because their failure mode is a confusing
Ansible error rather than a clear one:

* **Python 3 on every host**, which is what Ansible's modules run under.
* **`python3-psycopg2` on the database host**, which the PostgreSQL modules
  import.

Older CyVerse notes also call for `python-simplejson`, `python-httplib2`, and
`curl` on CentOS 5/6 and early Ubuntu hosts. Those instructions apply only to
distributions that are now end of life; on a current OS the packages above are
enough.

# SSH access

```bash
# generate a key if you do not have one
ssh-keygen -t ed25519

# install it on each host
ssh-copy-id -i ~/.ssh/id_ed25519.pub <SSH_USER>@<HOST>
```

Then `ssh` to each host once by fully qualified name, so its host key lands in
`~/.ssh/known_hosts` — Ansible fails on an unknown host key, and doing this
up front turns a run-time failure into a five-minute setup step. An `~/.ssh/config`
entry per host saves typing later.

`ed25519` is the current default; older notes generate RSA keys, which still work
but are no longer the recommendation.

# Related

* [Docker-based setup](./docker.md)
* [Cluster](../04-kubernetes/cluster.md)
* [Cluster resources](../04-kubernetes/resources.md)
* [Database migrations](../02-databases/migrations.md)
