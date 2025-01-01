# ansible.builtin.file

Create file:
```yaml
- name: Create file
  ansible.builtin.file:
    path: /path/to/file.txt
    owner: some-user
    group: some-group
    state: touch
```

Delete file:
```yaml
- name: Delete file
  ansible.builtin.file:
    path: /path/to/file.txt
    state: absent
```

Create folder:
```yaml
- name: Create a directory if it does not exist
  ansible.builtin.file:
    path: /etc/some_directory
    state: directory
    owner: some-user
    group: some-group
    mode: '0755'
```

