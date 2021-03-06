# drudconfig
A cli for configuration management.

## Use

Get the binary. Either build the source or head to the releases section.

Create a Config Set (drud.yaml).

```
- name: install
  env:
    site_name: MangoTango
    git_token: $GITHUB_TOKEN
  tasks:
  - cmd: echo "{{.site_name}}"
    wait: 1s
    repeat: 3
  - name: create directory
    cmd: mkdir testing
    ignore: yes
  - name: make file in new directory
    workdir: testing
    cmd: touch newfile.txt
  - name: make content
    workdir: testing
    write: |
        what what what
        what what {{.git_token}}
        whathwat
    dest: newfile.txt
    mode: 0777
  - name: look
    cmd: ls -l testing
  - name: echo "{{.git_token}}"

- name: uninstall
  tasks:
  - cmd: echo "testing this thing"
    name: say something cool
  - cmd: rm newfile.txt
    name: remove new file
    workdir: testing
  - name: rm directory
    cmd: rm -rf testing
  - cmd: echo "woot"
```

Now you can run all Config groups in sequential order or you can run jus tone group at a time.

```
dcfg run all
```
or
```
dcfg run install
dcfg run uninstall
```
or
```
dcfg run install uninstall
```
