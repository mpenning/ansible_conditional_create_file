# Ansible: Conditionally create a file

Use ansible to conditionally create a file.  There are two options for doing this...

## Option 1: shell module

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

## Option 2: copy module

This uses a side-effect of the ansible `copy` module, which can create a file

``` yaml
---
- name: Add a new file
  hosts: foo.localdomain
  tasks:
    - name: Conditionally create a file if it does not exist
      copy:
        content: "insert-sensitive-data"
        dest: data.txt
        force: no
        owner: mpenning
        group: mpenning
        mode: 0400
```
