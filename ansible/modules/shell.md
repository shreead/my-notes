# ansible.builtin.shell

Run shell command and show result:
```yaml
- name: check system info
  ansible.builtin.shell:
    shell: 
      "lsb_release -a"
    register: os_info

- debug:
  msg: "{{ os_info.stdout_lines }}"
  
```