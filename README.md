# Ansible Role: Git

[![CI](https://github.com/geerlingguy/ansible-role-git/workflows/CI/badge.svg?event=push)](https://github.com/geerlingguy/ansible-role-git/actions?query=workflow%3ACI)

Installs Git, a distributed version control system, on any RHEL/CentOS or Debian/Ubuntu Linux system.

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    workspace: /root

Where certain files will be downloaded and adjusted prior to git installation, if needed.

    git_enablerepo: ""

This variable, a well as `git_packages`, will be used to install git via a particular `yum` repo if `git_install_from_source` is false (CentOS only). Any additional repositories you have installed that you would like to use for a newer/different Git version.

    git_packages:
      - git

The specific Git packages that will be installed. By default, only `git` is installed, but you could add additional git-related packages like `git-svn` if desired.

    git_install_from_source: false
    git_install_path: "/usr"
    git_version: "2.26.0"

Whether to install Git from source; if set to `true`, `git_version` is required and will be used to install a particular version of git (see all available versions here: https://www.kernel.org/pub/software/scm/git/), and `git_install_path` defines where git should be installed.

    git_install_from_source_force_update: false

If git is already installed at an older version, force a new source build. Only applies if `git_install_from_source` is `true`.

### Configuration
Use `git_global_config` to enable git configuration on a users basis. There must be one list entry for each user.
Username is mandatory.

    git_global_config:
      - username: jan
        config:
          user:
            name: <name>
            email: <email>
          alias:
            tree: 'log --decorate --pretty=oneline --abbrev-commit --graph'
            co: 'checkout'
            s: 'status'
            sw: 'switch'

#### Set alternative `user` and `email` for different domains
With the help of templates and the post-checkout hook, git can automatically set a local user and email for each repository which is cloned.
Under `git_global_config`, one can configure a list of domains with corresponding username and email. This settings will be applied to the repo's local config after cloning it.

    clone_init_domains:
      - name: github.com
        username: <username>
        email: 40234802+<username>@users.noreply.github.com

For this to work, `templatedir` in the gitconfig section `init` must also be set.

## Dependencies

None.

## Example Playbook

    - hosts: servers
      roles:
        - { role: geerlingguy.git }

## License

MIT / BSD

## Author Information

This role was created in 2014 by [Jeff Geerling](https://www.jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).
