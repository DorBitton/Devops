# Introduction to Configuration

* Configuration file:
    * `/etc/ansible/ansible.cfg`
* Defining what Configuration file to use:
    * `$ANSIBLE_CONFIG=/opt/ansible-web.cfg ansible-playbook playbook.yml`
* Configuration Priority: (Highest to lowest)
    1) ENV variable (`$ANSIBLE_CONFIG=`)
    2) Local configuration from which the ansible playbook run
    3) `.ansible.cfg` file in USR directory
    4) Default ansible configuration file: `/etc/ansible/ansible.cfg`
* View Configuration
    * `ansible-config list` Lists all configuration
    * `ansible-config view` Shows the current config file
    * `ansible-config dump` Shows the current settings 