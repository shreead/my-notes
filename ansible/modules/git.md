# ansible.builtin.git
Clone git repo:
```yaml
- name: Clone git repo
  ansible.builtin.git:
    repo: 'https://github.com/user/repo.git'
    dest: /path/to/repo/
    clone: yes
    update: yes
```