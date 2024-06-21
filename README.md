Neovim
=========

Deploy latest [neovim](https://github.com/neovim/neovim) and
[tree-sitter](https://github.com/tree-sitter/tree-sitter) from GitHub for x86_64
Debian-based systems.

_Optionally:_

- Install nightly neovim appimage as `nnvim`
- Sync local nvim configuration to remote machines
- Install supporting apt packages
- Install supporting pip packages (uses pipx)
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
neovim_version: "0.10.0"

treesitter: true
treesitter_version: "0.22.6"
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

- `neovim_pde` is the toggle that enables/disables this extended syncing mode.
  When this value is false (**default**) none of the rest of the other PDE
  features will run. No packages, No syncs.

#### Packages

There are currently 3 supported packaging systems:

- `neovim_apt_packages`
- `neovim_pip_packages`
- `neovim_npm_packages`

Each packaging variables should contain a list of packages to install. If you
select packages from a given source the package manager will also be installed.
Your variables file may look something like this.

```yml
neovim_pde: true
neovim_npm_packages:
  - neovim
neovim_pip_packages:
  - pynvim
  - flake8
```

In this case the 'npm' and 'pip' package managers will also be installed in
their latest state. There are finer-grain controls in the [default
vars](https://github.com/gikeymarcia/ansible-role-neovim/blob/master/defaults/main.yml)
under "digging deeper"

#### Configuration

When syncing configuration you need to define a `primary_user` who will receive
the configuration. Below I use the 'neovim_config_syncs' list to enumerate which
files and folders within my local `~/.config/nvim/` to synchronize to the remote
machine.

<br>

**Tip**: Folders you want to sync should have a trailing `/`.

<br>

```yml
neovim_pde: true

primary_user: prime

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
neovim_config_dirs:
  - "/home/{{ primary_user }}/.config/nvim"

```

If you need a folder to exist in the remote `$HOME` then it should be defined in
the `neovim_config_dirs` list using `/home/{{ primary_user }}` as the root of
the path. For example, above I make sure the nvim config folder exists at
`/home/{{ primary_user }}/.config/nvim`

<br>

Finally, if you need to copy configuration files that are outside your
~/.config/nvim you can use the `neovim_external_config` variable. This variable
should be a list of key-value pairs, 'src:' and 'dest:'.

- `src` is the path to the local file
- `dest` is the remote machine path which would receive the 'src' file.
  Generally these will begin with `/home/{{ primary_user }}`.

```yml
neovim_external_config:
  - src: ~/.config/flake8
    dest: "/home/{{ primary_user }}/.config/flake8"
  - src: ~/.config/shellcheckrc
    dest: "/home/{{ primary_user }}/.config/shellcheckrc"
```

Above I copy the local flake8 and shellcheck configuration files onto remote
hosts.

Dependencies
------------

None.

Example Playbook
----------------

Below I combine all of the above configuration into a single playbook.

```yml
---
- hosts: all
  become: true

  roles:
    - gikeymarcia.neovim
  vars:
    neovim_nightly: true
    primary_user: mikey
    neovim_pde: true
    neovim_npm_packages:
      - neovim
    neovim_pip_packages:
      - pynvim
      - flake8
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
    neovim_config_dirs:
      - "/home/{{ primary_user }}/.config/nvim"
    neovim_external_config:
      - src: ~/.config/flake8
        dest: "/home/{{ primary_user }}/.config/flake8"
      - src: ~/.config/shellcheckrc
        dest: "/home/{{ primary_user }}/.config/shellcheckrc"
...
```

License
-------

GPLv3

Author Information
------------------

Find me on GitHub @ https://github.com/gikeymarcia
