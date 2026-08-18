# EgyKode lab environment

The machines the [EgyKode](https://egykode.com) labs run on: a controller you
work from, and a managed node for it to configure.

```bash
./egykode start          # controller + node
./egykode start k8s      # ...and a three-node Kubernetes cluster
./egykode shell          # a prompt inside the controller
./egykode doctor <lab>   # what a given lab needs, and what is missing
./egykode stop           # stop, keeping your work
```

On Windows use the same commands — `egykode.cmd` makes `./egykode` work in
PowerShell and CMD. You need [Docker](https://docs.docker.com/get-docker/) and
[Git](https://git-scm.com/downloads); everything else runs inside the
containers.

## What you get

| | |
| --- | --- |
| `egykode-controller` | git, ansible, terraform, kubectl, helm, docker CLI |
| `egykode-node` | a second machine over SSH, running real systemd |

Two containers rather than one, because a control node with nothing to manage
can only target localhost — which hides inventories, SSH and every failure mode
worth learning. The node runs systemd so `systemctl`, `journalctl` and
Ansible's service modules do what they do on a server.

Your work lives in a Docker volume and survives `./egykode stop`. `./egykode
reset` deletes it, and asks first.

## This repository is generated

The sources live in [the main EgyKode repository](https://github.com/Waleeddarwesh/EgyKode)
under `docker/`, `clusters/` and the `egykode` script. Edit them there;
`scripts/sync-lab-env.mjs` regenerates this mirror, and CI fails when the two
drift. Changes made directly here are lost on the next sync.
