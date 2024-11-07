# ansible.posix.mount
Example:
```
- name: Create cifs-credentials for media
  ansible.builtin.copy:
    content: |
      username={{ cifs_username_media }}
      password={{ cifs_password_media }}
    dest: '/etc/cifs-credentials-media'
    mode: '0600'

- name: Setup media share in /etc/fstab
  ansible.posix.mount:
    src: //10.0.0.10/media
    path: /mnt/media
    opts: "credentials=/etc/cifs-credentials-media,uid=user,gid=user"
    fstype: cifs
    state: mounted
    dump: 0
    passno: 0
```