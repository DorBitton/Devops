# Playbook Run Options

* Check Mode or Dry Run: 
    * `ansible-playbook playbook.yml --check`
* Start at:
    * `ansible-playbook playbook.yml --start-at-task "task name"` 
        * Start at Tags: `ansible-playbook playbook.yml --tags "tag"`
            * Skip Tag:
                * `ansible-playbook playbook.yml --skip-tags "tag"`