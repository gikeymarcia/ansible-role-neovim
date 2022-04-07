Neovim
=========

Deploy latest [neovim](https://github.com/neovim/neovim) and
[tree-sitter](https://github.com/tree-sitter/tree-sitter) from GitHub for x86_64
Debian-based systems.

_Optionally:_

- Install nightly neovim appimage as `nnvim`
- Sync local nvim configuration to remote machines
- Install supporting apt packages
- Install supporting pip packages
- Install supporting npm packages

Requirements
------------

None.

Role Variables
--------------

### Default configuration

By default this role will install neovim and tree-sitter from GitHub. You can
toggle either behavior off and/or change which version of each application is
installed with variables.

```yml
neovim: true
neovim_version: "0.6.1"

treesitter: true
treesitter_version: "0.20.6"
```

### Nightly Neovim Release

If you want the latest nightly neovim appimage release from GitHub you can set
'neovim_nightly: true' (**default: false**)

```yml
neovim_nightly: true
```

### Neovim PDE (Personal Development Environment)

As most vim users will tell you, it becomes a tool fit to your needs over time.
The trouble with this tool is keeping all the moving parts in sync across
multiple machines. The 'neovim_pde' toggle seeks to help you capture your
development environment configuration in one place and move them onto multiple
machines seamlessly

**Below are the default values of the role.**

```yml
neovim_pde: false

# packages
neovim_apt_packages: []
neovim_pip_packages: []
neovim_npm_packages: []

# configuration
neovim_external_config: []
neovim_config_dirs: []
neovim_config_syncs: []
```

- `neovim_pde` is the toggle that enables/disables the extended syncing and
  mode.
- `neovim_apt_packages` a list of packages that should be installed from the apt
  repository
- `neovim_pip_packages` list of pip packages to have installed at their latest
  state
- `neovim_npm_packages` a list of packages to be instal


This is what the variables look like when I'm using, shellcheck, npm, and some
pip modules for python development.

```yml
neovim_pde: true
neovim_apt_packages:
  - shellcheck
  - nodejs
  - npm
neovim_pip_packages:
  - black
  - flake8
  - flake8-bugbear
  - virtualenv
neovim_config_dirs:
  - "{{ primary_home }}/.config/nvim"
  - "{{ primary_home }}/.config/coc/extensions"
neovim_external_config:
  - src: ~/.config/shellcheckrc
    dest: "{{ primary_home }}/.config/shellcheckrc"
  - src: ~/.config/flake8
    dest: "{{ primary_home }}/.config/flake8"
neovim_config_syncs:
  - init.vim
  - coc-settings.json
  - autoload/
  - ftplugin/
  - lua/
  - plug-config/
  - snips/
  - spell/
  - syntax/
```



- `neovim_pde: true` copies lots of config from the Ansible control host and
  deploys them onto each remote machine.

If running this role with `neovim_pde: true` then you must also define
`primary_user` and `primary_home`.

```yml
primary_user: greezy
primary_home: /home/greezy
```

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yml
- hosts: all
  become: true

  roles:
    - gikeymarcia.neovim
  vars:
    neovim_nightly: true
    neovim_pde: true
    primary_user: greezy
    primary_home: /home/greezy
```

License
-------

GPLv3

Author Information
------------------

Find me on github @ https://github.com/gikeymarcia
