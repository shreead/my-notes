# ansible.builtin.apt

Update apt cache
```yaml
- name: Perform apt update
  ansible.builtin.apt:
    update_cache: true
    cache_valid_time: 3600
```

Dist-upgrade
```yaml
- name: Perform apt dist-upgrade
  ansible.builtin.apt:
    upgrade: dist
```

Install
```yaml
- name: Install packages
  ansible.builtin.apt:
    name:
      - git
      - tree
    state: latest # or present
```

Uninstall
```yaml
- name: Uninstall packages
  ansible.builtin.apt:
    name: tree
    state: absent
```