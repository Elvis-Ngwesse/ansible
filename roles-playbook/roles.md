# Apache Install Role

A simple Ansible role to install and configure Apache HTTP Server (httpd) on various platforms, including EL 
(RedHat-based), Ubuntu, AWS, and GCP. This role ensures that the correct Apache package is installed based on 
the operating system and that the Apache service is started and enabled to run on boot.

## Why Use Roles in Ansible?

In Ansible, **roles** are a way to organize and modularize automation tasks. Using roles offers several benefits:

- **Reusability**: Roles allow you to reuse the same set of tasks and configurations across different playbooks. 
By separating functionality into different roles, you can easily manage and apply them to different environments 
without rewriting tasks.

- **Simplification**: Instead of writing long playbooks with all tasks in a single file, you can break them down into 
smaller, more manageable pieces. This makes it easier to maintain, debug, and scale your automation.

- **Maintainability**: By using roles, your Ansible code becomes more organized and easier to understand. 
Roles isolate specific functionalities, which helps teams collaborate without stepping on each other’s work.

- **Separation of Concerns**: Roles help you maintain a clear distinction between different aspects of your 
infrastructure (e.g., Apache installation, MySQL configuration, etc.). This keeps your code cleaner and more modular.

- **Community Best Practices**: Roles are an Ansible best practice, especially when working in teams or contributing 
to open-source projects. Many pre-built roles are available in Ansible Galaxy, and understanding how to create and 
use roles is key to effective Ansible usage.

## Requirements

- Ansible 2.9 or higher
- Supported platforms (see below for details):
    - RedHat 7/8
    - Ubuntu 20.04/22.04
    - AWS (any version)
    - GCP (any version)

## Role Variables

The following variables are defined within this role:

### Apache Package Map
The Apache HTTP Server package name based on the operating system.

- **RedHat / Amazon**: `httpd`
- **Debian / Ubuntu**: `apache2`

These values are automatically selected based on the system’s OS family.

### Apache Service Map
The name of the Apache service based on the operating system.

- **RedHat / Amazon**: `httpd`
- **Debian / Ubuntu**: `apache2`

These values are automatically selected based on the system’s OS family.

## Dependencies

This role does not have any dependencies.

## Example Playbook

Here's an example playbook that uses this role to install and configure Apache on your hosts:

```yaml
- name: Install and configure Apache on AWS and GCP
  hosts: all
  gather_facts: true
  roles:
    - apache_role
```

# Access apache
curl http://<REMOTE_IP>
