# Ansible Role: `procyon_decompiler`

Installs or uninstalls [Procyon Java
Decompiler](https://bitbucket.org/mstrobel/procyon/wiki/Java%20Decompiler).
Creates or removes a wrapper to start the program from the shell.

## Requirements

- A recent version of Ansible. Tested on 2.9. It might work on previous versions.
- Fedora 31. It might work on other versions.
- Access to a Procyon Java decompiler jar. Either by URL to download or path to
  a file.

## Role Variables

All variables which can be overridden are stored in
[defaults/main.yml](defaults/main.yml) file and listed in a table bellow as
well.

| Name                                    | Default Value      | Description  |
| --------------------------------------- | ------------------ | ------------ |
| `procyon_decompiler_jar_src`            | ""                 | Path to a jar file of procyon decompiler on a control host or URL to download from. |
| `procyon_decompiler_state`              | present            | By default installs the program. Set to "absent" to uninstall. |
| `procyon_decompiler_top_dest_name`      | procyon-decompiler | Base name of installation directory. |
| `procyon_decompiler_install_parent_dir` | /opt               | Parent directory in which program will be installed. See also `procyon_decompiler_top_dest_name`. |
| `procyon_decompiler_force_remove`       | no                 | If set to "yes", contents of installation directory is not compared against contents of `procyon_decompiler_jar_src` when uninstall, upgrade or downgrade. |

`procyon_decompiler_jar_src` must be overriden from its default value.

## Example Playbook

### Install

To install the program, define path to a Procyon Java decompiler jar from the
internet:

```yaml
- hosts: all
  roles:
    - role: scrool.procyon_decompiler
      vars:
        procyon_decompiler_jar_src: https://bitbucket.org/mstrobel/procyon/downloads/procyon-decompiler-0.5.36.jar
```

### Uninstall

To uninstall, set same values as for install. By default role removes content
only if installed directory and jar archive matches what it would install.
Finally set state variable to "absent".

```yaml
- hosts: all
  roles:
    - role: scrool.procyon_decompiler
      vars:
        procyon_decompiler_jar_src: /tmp/procyon-decompiler-0.5.36.jar
        procyon_decompiler_state: absent
```

Alternatively you can enable option `procyon_decompiler_force_remove` and again
set state variable to "absent". In this case role won't need nor even check
`procyon_decompiler_jar_src`. This effectively removes unversioned symlink to a
jar, installed jar, installation directory and executable. Example:

```yaml
- hosts: all
  roles:
    - role: scrool.procyon_decompiler
      vars:
        procyon_decompiler_state: absent
        procyon_decompiler_force_remove: yes
```

## License

This project is licensed under MIT License. See [LICENSE](LICENSE) for more
details.

See [Procyon repository](https://bitbucket.org/mstrobel/procyon/src/default/)
for license information.

## Author Information

- [Pavol Babinčák](https://github.com/scrool)
