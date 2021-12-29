# ansible-linode-gpu-setup

Automatic provisioning and setup for Linode GPU cloud instances with Ansible.

# Installation

This project uses the [linode.cloud](https://github.com/linode/ansible_linode) collection. You need a [Linode](https://www.linode.com/?r=e54e6c8185f5399de2527f9f3bc7cde39bbcc624) account with enabled API Token and [Ansible](https://www.ansible.com/) installed. These scripts were tested with `Ansible 2.11.7` running on `Python 3.7.11`.

Install the required dependencies to run the scripts with:

```bash
ansible-galaxy collection install linode.cloud
pip install linode_api4==5.2.1
pip install polling==0.3.2
```

# Usage

First, make sure to export your `LINODE_API_TOKEN` with:

```bash
export LINODE_API_TOKEN=XXXXXXX
```

The configuration for the server can be found in [config.yml](config.yml). The ssh key loaded to the server is `~/.ssh/linode.pub`. Change the `ssh_keys` variable in the configuration if yours is in a different location. The server configuration is:

```yml
server_name: gpu_server
server_type: g1-gpu-rtx6000-1
server_image: linode/ubuntu20.04
server_region: us-east
```

Both `server_name` and `server_region` can be changed without breaking the scripts.

Create and setup a Linode GPU server with:

```bash
ansible-playbook -v linode-create-gpu-server.yml \
  --extra-vars="username=user password=YOUR_PASS root_password=YOUR_ROOT_PASS"
```

List information about the server with:

```bash
ansible-inventory --list
```

Destroy the running Linode GPU server with:

```bash
ansible-playbook -v linode-destroy-gpu-server.yml
```


# License 
This project is licensed under the MIT license. See the [LICENSE](LICENSE) for details.
