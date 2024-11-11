# ansible.builtin.blockinfile

Replace a block of text in a file. 

Sample `text.txt`

```
123
#BeginEntries
bad_entry_1
bad_entry_2
#EndEntries
321
```

Sample playbook:
```yaml
- name: Replace block of text
  hosts: hostname
  tasks:
    - name: Replace block of text
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

Result `text.txt`
```
123
#BeginEntries
my awsome content with

even

multiple

blank

lines
#EndEntries
321
```

## References
[https://stackoverflow.com/posts/77369023/timeline](https://stackoverflow.com/posts/77369023/timeline)