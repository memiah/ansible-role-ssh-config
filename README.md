Ansible Role: SSH Config
==================

Configure system users, groups and SSH access, optionally based on GitHub users.

Requirements
------------

None.

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):

    ssh_config_users: []
      # You can specify an object with 'name' (required) and 'groups' (optional):
      # - name: example
      #   groups: www-data,memiah
      #   authorized_keys:
      #     - "ssh-rsa ..."
    
      # Or you can specify a GitHub username:
      # - github: memiah
      
A list of users to add to the server; the username will be the name. You can add the user to one or more groups (in 
addition to the [username] group) by adding them as a comma-separated list in groups. SSH keys can be 
added using the authorized_keys options with a list of keys. Specify a github username to fetch authorised keys from
GitHub.

     ssh_config_groups_additional: []

Additional system users that can be assigned to specific inventories or roles, useful when using `ssh_config_users` as a common default user list.


    ssh_config_users_absent: []
      # You can specify an object with 'name' (required):
      # - name: example
    
      # Or you can specify a username directly:
      # - example
      
A list of users who should not be present on the server and should be removed.

    ssh_config_groups: []
      # - name: example
      #   passwordless_sudo: True
      
System groups that should be created. These can be assigned to users defined in `ssh_config_users`. If the group should
be allowed passwordless_sudo, optionally set that here.

    ssh_config_groups_additional: []

Additional system groups that can be assigned to specific inventories or roles, useful when using `ssh_config_groups` as a common default group list.

    ssh_config_groups_absent: []
      # You can specify an object with 'name' (required):
      # - name: example
    
      # Or you can specify a group directly:
      # - example

A list of groups that should not be present on the server and should be removed.

    ssh_config_github_url: https://github.com

By default, use public GitHub (i.e. https://github.com) as the source for users/keys. Override this to use a different 
GitHub instance/endpoint (e.g. GitHub Enterprise).

    ssh_config_ssh_auth_sock: True

For SSH agent forwarding, maintain the SSH_AUTH_SOCK environment variable.

Dependencies
------------

  - geerlingguy.security

Example Playbook
----------------

    - hosts: servers
      become: yes
      vars:
        ssh_config_users:
          # You can specify an object with 'name' (required) and 'groups' (optional):
          - name: jane-doe
            groups: www-data,example
            authorized_keys:
              - "ssh-rsa ..."
          # Or you can specify a GitHub username:
          - github: john-doe
          
        ssh_config_users_absent:
            - johndoe
            - name: jane
        
      roles:
        - memiah.ssh-config

License
-------

MIT / BSD

Author Information
------------------

This role was created in 2018 by [Memiah Limited](https://github.com/memiah).
