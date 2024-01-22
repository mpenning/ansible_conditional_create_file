# Ansible: Conditionally create a file

Use ansible to conditionally create a file.  There are two options for doing this...



## Option 1: copy module with sudo

This uses a side-effect of the ansible `copy` module and `force: no`, which can create a file as root.

``` yaml
---
- name: Add a new file
  hosts: foo.localdomain
  tasks:
    - name: Conditionally create a file if it does not exist
      copy:
        content: "insert-sensitive-data"
        dest: "{{ lookup('env', 'HOME') }}/data.txt"
        force: no
        owner: root
        group: root
        mode: 0400
        become: true
        become_with: sudo
        become_user: root
```

## Option 2: shell module

The shell module is sometimes discouraged, but it also can create the file conditionally.  You can also sudo with `shell` if you want.

```yaml
---
- name: Add a new file
  hosts: foo.localdomain
  tasks:
    - name: Conditionally create a file if it does not exist
      shell: echo "insert-sensitive-data" > $HOME/data.txt
      args:
        creates: "$HOME/data.txt"

```
