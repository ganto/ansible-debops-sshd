## Ansible Role: sshd

**IMPORTANT**
_This role is a fork of the [debops.sshd](https://github.com/debops/debops/tree/master/ansible/roles/debops.sshd)
Ansible role. It's not supported by the DebOps project nor anyone else. Don't
open any issues! For the original postfix role checkout the [DebOps](https://github.com/debops/debops)
repository._

[OpenSSH](http://www.openssh.com/) is a secure replacement for `telnet`
and other remote control programs. It allows you to connect to remote hosts
over a encrypted communication channel and perform a variety of tasks. It's
also the main communication channel used by Ansible.


### Documentation

More information about `debops.sshd` can be found in the
[official debops.sshd documentation](http://docs.debops.org/en/latest/ansible/roles/ansible-sshd/docs/).


### Role dependencies

- `debops.secret`


### Example Playbook

```yaml
- name: Manage OpenSSH Server
  hosts: [ 'debops_service_sshd' ]
  become: True

  environment: '{{ inventory__environment | d({})
                   | combine(inventory__group_environment | d({}))
                   | combine(inventory__host_environment  | d({})) }}'

  roles:

    - role: debops.sshd/env
      tags: [ 'role::sshd', 'role::ldap' ]

    - role: debops.apt_preferences
      tags: [ 'role::apt_preferences', 'skip::apt_preferences' ]
      apt_preferences__dependent_list:
        - '{{ sshd__apt_preferences__dependent_list }}'

    - role: debops.ferm
      tags: [ 'role::ferm', 'skip::ferm' ]
      ferm__dependent_rules:
        - '{{ sshd__ferm__dependent_rules }}'

    - role: debops.tcpwrappers
      taos: [ 'role::tcpwrappers', 'skip::tcpwrappers' ]
      tcpwrappers_dependent_allow:
        - '{{ sshd__tcpwrappers__dependent_allow }}'

    - role: debops.python
      tags: [ 'role::python', 'skip::python', 'role::ldap' ]
      python__dependent_packages3:
        - '{{ ldap__python__dependent_packages3 }}'
      python__dependent_packages2:
        - '{{ ldap__python__dependent_packages2 }}'

    - role: debops.ldap
      tags: [ 'role::ldap', 'skip::ldap' ]
      ldap__dependent_tasks:
        - '{{ sshd__ldap__dependent_tasks }}'

    - role: debops.pam_access
      tags: [ 'role::pam_access', 'skip::pam_access' ]
      pam_access__dependent_rules:
        - '{{ sshd__pam_access__dependent_rules }}'

    - role: debops.sshd
      tags: [ 'role::sshd', 'skip::sshd' ]
```

### Authors and license

`sshd` role was written by:
- Maciej Delmanowski | [e-mail](mailto:drybjed@gmail.com) | [Twitter](https://twitter.com/drybjed) | [GitHub](https://github.com/drybjed)

License: [GPLv3](https://tldrlegal.com/license/gnu-general-public-license-v3-%28gpl-3%29)
