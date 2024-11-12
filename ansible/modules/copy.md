# ansible.builtin.copy

```yaml
- name: copy file
  ansible.builtin.copy:
    src: file-to-copy.txt
    dest: /location/to/copy/to/name.txt
    owner: username
    group: groupname
    mode: 0644
```