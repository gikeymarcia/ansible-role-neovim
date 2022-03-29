Neovim
=========

Deploy neovim from GitHub.

Requirements
------------

None.

Role Variables
--------------

Below are the role variables and their defaults

```yml
neovim: true
neovim_ide: false
neovim_nightly: false
```

- `neovim_nightly: true` downloads latest nightly AppImage build from GitHub and
  install as `nnvim`

- `neovim_ide: true` copies lots of config from the Ansible control host and
  deploys them onto each remote machine.

If running this role with `neovim_ide: true` then you must also define
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
    - neovim
  vars:
    neovim_nightly: true
    neovim_ide: true
    primary_user: greezy
    primary_home: /home/greezy
```

License
-------

GPLv3

Author Information
------------------

Find me on github @ https://github.com/gikeymarcia
