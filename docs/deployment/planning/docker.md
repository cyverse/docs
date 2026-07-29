---
type: Deployment Procedure
title: "Docker"
description: "Installing Docker and running the deployment playbooks from inside a container."
tags: [deployment, planning, docker, containers]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
sources:
  - id: docker-docs
    resource: https://docs.docker.com/engine/install/
    title: Docker Engine installation
    author: team:docker
---

# Install

Follow the [Docker Engine installation
guide](https://docs.docker.com/engine/install/) for your platform. On macOS and
Windows that means Docker Desktop; on Linux, the distribution packages or Docker's
own repository.

You need Docker for two unrelated reasons: building service and tool images, and —
optionally — running the deployment playbooks in a container instead of installing
Ansible on your workstation.

# Running the playbooks in a container

The deployment repository includes a `Dockerfile` at the top level that builds an
image capable of running the playbooks. It is useful when your workstation cannot
easily run the Ansible version the playbooks expect.

```bash
# generate the SSH configuration the image expects
./create-ssh-configs.sh

# build the image
docker build -t cyverse-ansible .

# run the playbooks with the working tree mounted
docker run --rm -it -v "$(pwd)":/ansible -w /ansible cyverse-ansible /bin/bash
```

From the shell inside the container, run `ansible-playbook` as usual; see
[Ansible](./ansible.md).

!!! danger "Never push this image"

    The build embeds your SSH configuration, and a run mounts your inventory. That
    image is a copy of your credentials, and pushing it to any registry publishes
    them. Build it locally, use it locally, and do not tag it for a registry.

# Related

* [Ansible](./ansible.md)
* [Harbor](../04-kubernetes/harbor.md)
* [Prerequisites](./prerequisites.md)
