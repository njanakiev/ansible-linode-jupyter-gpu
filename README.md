# ansible-linode-gpu-jupyter-setup

Automatic provisioning and setup for [Linode](https://www.linode.com/?r=e54e6c8185f5399de2527f9f3bc7cde39bbcc624) GPU cloud instances with Ansible. The setup includes installation of the following packages and libraries:

- [CUDA Toolkit 11.5](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=20.04&target_type=deb_local) for Ubuntu 20.04
- [Python 3.9](https://www.python.org/)
- [PyTorch 1.10.1](https://pytorch.org/)
- [JupyterLab](https://jupyterlab.readthedocs.io/en/stable/) including installation as a systemd service

Note, the installation also runs on CPU-only Linodes, but without CUDA and GPU support. This can be useful for testing.

# Installation

This project uses the [linode.cloud](https://github.com/linode/ansible_linode) collection. You need a [Linode](https://www.linode.com/?r=e54e6c8185f5399de2527f9f3bc7cde39bbcc624) account with enabled API Token and [Ansible](https://www.ansible.com/) installed to run the following scripts. These scripts were tested with `Ansible 2.11.7` running on `Python 3.7.11`.

Install the required dependencies to run the scripts with:

```bash
ansible-galaxy collection install linode.cloud
pip install linode_api4==5.2.1
pip install polling==0.3.2
```

# Usage

First, make sure to export your `LINODE_API_TOKEN` and desired server credentials with:

```bash
export LINODE_API_TOKEN=XXXXXXX
export SERVER_ROOT_PASSWORD=XXXXXXXX
export SERVER_USERNAME=user
export SERVER_PASSWORD=XXXXXXXX
```

The configuration for the server can be found in [config.yml](config.yml). The ssh key loaded to the server is `~/.ssh/linode.pub`. Change the `ssh_keys` variable in the configuration if yours is in a different location. The server configuration is:

```yml
server_name: gpu_server
server_type: g1-gpu-rtx6000-1
server_image: linode/ubuntu20.04
server_region: us-east
```

Note, that currently only `linode/ubuntu20.04` is supported for this script. All the available server types can be found by querying the [Linode Types](https://www.linode.com/docs/api/linode-types/). All available regions can be found in [Regions](https://www.linode.com/docs/api/regions/). Not all regions offer GPU instances and might not be available at all times.

Create and setup a Linode server with:

```bash
ansible-playbook -v linode-create-server.yml
```

List information about the server with:

```bash
ansible-inventory --list
```

Destroy the running Linode server with:

```bash
ansible-playbook -v linode-destroy-server.yml
```


# License 
This project is licensed under the MIT license. See the [LICENSE](LICENSE) for details.
