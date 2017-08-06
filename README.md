Docker Stack Deploy
=========

Simple role for deploying stack to docker swarm using variable generated `docker-compose.yml` file.

Requirements
------------

You need to run this on node with docker swarm mode enabled see https://docs.docker.com/engine/swarm/

Role Variables
--------------

| Var Name         | Required | Desc |
| ---------------- | :------: | ---- |
| `stack_name`     | Y        | Name of stack which you are building |
| `instance_name`  | Y        | Name of instance when you need to have multiple stacks deployed |
| `stack_services` | Y        | Dict of stack services see [example playbook](#Example-Playbook)
| `docker_network` | N        | Name of external docker network

Dependencies
------------

No dependencies so far ;)

Example Playbook
----------------

    - hosts: docke_swarm_master
      become: true
      vars:
        cluster_name: appstack-poc
        instance_name: "deploy01"
        docker_network: "{{ cluster_name }}-{{ instance_name }}"
      roles:
        - role: docker_stack_deploy
          stack_name: mega_kitten_app
          stack_services:
            webserver:
              image: nginx
            database_server:
              image: arangodb/arangodb
              environment:
                ARANGO_NO_AUTH: 1
              entrypoint: /bin/wait-for-db.sh && /bin/start.sh
              deploy:
                mode: replicated
                replicas: 2

License
-------

MIT

Author Information
------------------

Mario Vejlupek
