Format
```yaml
---
- hosts: hostname
  become: true
  tasks:
    - name: Start a container
      docker_container:
        name: 'container name'
        image: 'image source'
        state: 'started'
        ports:
         - "xxxx:xxxx"
        env:
          item: 'value'
        volumes:
          - 'form:to'
        labels:
          label.name: 'value'
      tags: tag1
```

Example:
```yaml
---
- hosts: vm-test01
  become: true
  vars_files:
    - vm-test01-vars.yml
  tasks:
    - name: Start calibre-web
      docker_container:
        name: 'calibre-web'
        image: 'lscr.io/linuxserver/calibre-web'
        state: 'started'
        ports:
         - "8083:8083"
        env:
          PUID: "1000"
          PGID: "1000"
          TZ: "America/Detroit"
        volumes:
          - "/home/shree/calibre-web/config:/config"
          - "/home/shree/calibre-web/books:/books"
        labels:
          homepage.group: 'MEDIA'
          homepage.name: 'Calibre-web-TEST'
          homepage.icon: 'si-calibreweb'
          homepage.href: 'http://10.10.200.152:8083'
          homepage.widget.type: 'calibreweb'
          homepage.widget.url: 'http://10.10.200.152:8083'
      tags: calibre-web

    - name: DockerProxy
      docker_container:
        name: 'dockerproxy'
        image: 'ghcr.io/tecnativa/docker-socket-proxy'
        state: 'started'
        ports:
          - '2375:2375'
        env:
          CONTAINERS: '1'
          SERVICES: '0'
          TASKS: '0'
          POST: '0'
        volumes:
          - '/var/run/docker.sock:/var/run/docker.sock:ro'
      tags: dockerproxy
```