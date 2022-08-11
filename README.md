# Ansible Role: Ubuntu Setup
[![CI](https://github.com/skaary/ansible-role-ubuntu-setup/actions/workflows/ci.yml/badge.svg?branch=main&event=push)](https://github.com/skaary/ansible-role-ubuntu-setup/actions?query=workflow%3Ci)

An Ansible Role that performs various first-run installation and setup tasks for Ubuntu based systems.

## Installation

Download the role directly from git by typing into your terminal:

```bash
$ ansible-galaxy install git+https://github.com/skaary/ansible-role-ubuntu-setup.git
```

or

```bash
$ ansible-galaxy install git+https://github.com/skaary/ansible-role-ubuntu-setup.git,,ubuntu_setup
```

to change the installed role name from _ansible-role-ubuntu-setup_ to just _ubuntu_setup_.

Alternatively, install the role via a _requirements.yml_ file, e.g. when installing multiple roles at once. See [ansible galaxy documentation](https://galaxy.ansible.com/docs/using/installing.html#installing-multiple-roles-from-a-file) for more information.

## Role Variables

| Variable name                 | Default value          | Description                                                                                                                                                                                                          |
| ----------------------------- | ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `apt_packages`                | `[]`                   | A list of package names to install.                                                                                                                                                                                  |
| `apt_removals`                | `[]`                   | A list of package names to uninstall.                                                                                                                                                                                |
| `default_keyserver`           | `keyserver.ubuntu.com` | Default keyserver to use when adding PPAs.                                                                                                                                                                           |
| `ubuntu_release`              | `yakkety`              | Default ubuntu version name to use when adding PPAs.                                                                                                                                                                 |
| `ppas`                        | `[]`                   | A list of PPAs to install. Each item should include `source`, `fingerprint`, and `keyserver`. See `defaults/main.yml` for examples.                                                                                  |
| `no_ppa_packages`             | `[]`                   | A list of `*.deb` packages to install directly. May be a path on the remote machine or a URL (since Ansible 1.6).                                                                                                    |
| `default_user`                | `someuser`             | A valid username.                                                                                                                                                                                                    |
| `default_group`               | `somegroup`            | A valid groupname.                                                                                                                                                                                                   |
| `default_files`               | `[]`                   | A list of files, links, or directories containing (at minimum) `dest`, and `state` for each item. See Ansible's [File module](http://docs.ansible.com/ansible/file_module.html) and `defaults/main.yml` for details. |
| `locale`                      | ``                     | Sets locale                                                                                                                                                                                                          |
| `timezone`                    | ``                     | Sets timezone                                                                                                                                                                                                        |
| `ufw_wanted`                  | `false`                | If set to false, ufw setup is skipped.                                                                                                                                                                               |
| `ufw_default_incoming_policy` | `deny`                 | Set default incoming policy.                                                                                                                                                                                         |
| `ufw_default_outgoing_policy` | `allow`                | Set default outgoing policy.                                                                                                                                                                                         |
| `ufw_logging`                 | `on`                   | Set logging on/off.                                                                                                                                                                                                  |
| `ufw_rules`                   | `[]`                   | Allows for configuration of ufw rules.                                                                                                                                                                               |

## Example Playbook

```yaml
- hosts: all
  vars:
    apt_packages:
      - vim
      - libreoffice
  roles:
    - ansible-role-ubuntu-setup
  ubuntu_release: focal
  ppas: 
    - source: "deb https://ppa.launchpadcontent.net/phoerious/keepassxc/ubuntu {{ ubuntu_release }} main"
      fingerprint: "D89C66D0E31FEA2874EBD20561922AB60068FCD6"
      keyserver: "{{ default_keyserver }}"
  locale: en_US.UTF-8
  timezone: Europe/London
```

## Testing the role

### Vagrant

Vagrant can be used to test the role in order to graphically see it working in a virtual machine. Make sure Vagrant and VirtualBox are installed:

```bash
wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install vagrant
```

Use the following commands to run vagrant and boot up the virtual machine:

```bash
$ cd tests
$ vagrant up
```

Use `vagrant destroy` after you are done testing to delete the virtual machine. For more information about Vagrant and its commands, see the [Vagrant documentation](https://www.vagrantup.com/docs/cli).

### Molecule with Docker

Molecule can be used to test the role with a docker container. Make sure Molecule is installed:

```bash
$ python3 -m pip install --user "molecule[docker]"
```

Use the following commands to run Molecule in order to create the docker container and access the created container:
```bash
$ molecule converge && molecule login
```

For more information on how to use Molecule please consult the [Molecule documentation](https://molecule.readthedocs.io/en/latest/getting-started.html).

> Note: Python and Docker are required for the use of molecule. For more information, see [Molecule installation](https://molecule.readthedocs.io/en/latest/installation.html).

## License

MIT / BSD
