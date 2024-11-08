Source: [https://stackoverflow.com/posts/77369023/timeline](https://stackoverflow.com/posts/77369023/timeline)

If your end goal is to replace content between those lines, you should use [ansible.builtin.blockinfile](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/blockinfile_module.html) module with `state: present` and replace it right away! 

**text.txt**

```
123
#BeginEntries
bad_entry_1
bad_entry_2
#EndEntries
321
```

**playbook.yaml**

```yaml
- name: Replace block
  hosts: localhost
  tasks:
    - name: Replace block
      ansible.builtin.blockinfile:
        path: "{{ playbook_dir }}/text.txt"
        marker: "#{mark}"
        marker_begin: "BeginEntries"
        marker_end: "EndEntries"
        state: present
        content: |
          my awsome content with

          even

          multiple

          blank

          lines
```