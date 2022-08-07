---
layout: post
title:  "Test Blog Post"
date:   2022-06-18 09:12:31 -0400
categories: jekyll update
---

```yaml
- name: Test for RHEL8 Supported EE
  hosts: workstation
  gather_facts: no
  become: false
  remote_user: student
  tasks:
    - name: Retrieve Container Images
      containers.podman.podman_image_info:
        name: ee-supported-rhel8:2.0
      register: container_info
      
    - name: Show Container Dumped Information
      debug:
        msg: "{{ container_info }}"

    - name: Show Container Name
      debug:
        msg: "{{ container_info.images | map(attribute='Config') | map(attribute='Labels') | map(attribute='name') | list }}"

    - name: Set Container Name
      set_fact:
        container_name: "{{ container_info.images | map(attribute='Config') | map(attribute='Labels') | map(attribute='name') | list }}"

    - name: Convert container_name to string
      set_fact:
        container_name_string: "{{ container_name | join(',') }}"

    - name: Show Container Name
      debug:
        msg: The container name list is {{ container_name }} and the container name string is {{ container_name_string  }}.

    - name: Container Name Found in Output
      debug:
        msg: "Looking for container name"
      until: '"early" in container_name_string'
      retries: 5
      delay: 2
```