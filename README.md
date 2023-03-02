Ansible Role: ansible_role_crond
=========

Configures cron files/entries under `/etc/cron.d`.

This role supports the following Linux distributions:

<ul>
<li>Enterprise Linux 7
</ul>

Requirements
------------

This role has no dependencies besides what is included in Ansible Core.

Role Variables
--------------


| Variable | Required | Default | Comments |
| -------- | -------- | ------- | -------- |
| `name` | Yes | | Name of the cronjob. |
| ` minute` | Yes | | Allowed values: `0-59` minute 0-59. `*` first-last (every minute). `,` a list of minutes;ie. 0,15 would be 0 and 15th minute. `-` a range of minutes; ie 0-3 would be minutes 0, 1, 2, 3. You can also specify a list of ranges 0-15,25-35. `/` step values will skip the specified number within a range; ie */5 is every 5 minutes, and 0-30/2 is every 2 minutes between 0 and 30 minutes. |
| `hour` | Yes | | Allowed values: `0-23` hour 0-23. `*` first-last (every hour). `,` a list of hours; ie 0,12 would be the 0 and 12th hours. `-` a range of hours; ie 19-23 would be hours 19,20,21,22 and 23. You can also specify a list of ranges 0-5,12-16. `/` step values will skip the specified number within a range; ie */4 is every 4 hours and 0-20/2 is every two hours between 0 and 20th hour. |
| `day_of_month` | Yes | | Allowed values: `1-31` day 1-31. `*` first-last (every day of the month) `,` a list of days; ie 1,15 would be 1st and 15th day of month `-` a range of days; ie 1-5 would be days 1,2,3,4 and 5. You can also specify a list of ranges 1-5,14-30. `/` step values will skip the specified number within a range; ie */4 is every 4 days, and 1-20/2 is every 2 days between 1st and 20th day of the month. |
| `month` | Yes | | Allowed values: `1-12` month 1-12. `JAN-DEC` month. `*` first-last (every-month). `,` a list of month; ie 1,6 would be jan and jun. `-` a range of months; ie 1-3 would be jan. feb and mar. You can also specify a list of ranges 1-4,8-12. `/` step values will skip the specifed number within a range; ie */4 is every 4 months, and 1-8/2 is every 2 months between jan and aug. |
| `day_of_week` | Yes | | Allowed values: `0-6` day of week where 0 represents sunday and 6 saturday. `SUN-SAT` day of week. `*` first-last (every day of the week). `,` a list of days; ie 1,5 would be mon and fri. `-` a range of days; ie 1-5 would mean mon, tue, wed, thu and fri. You can also specify a list of ranges 0-2,4-6. `/` step values will skip the specified number within a range; ie */4 is every 4 days, and 1-5/2 is every 2 days between mon and fri. |
| `user` | Yes | | Username of the user to run the cron job. |
| `state` | Yes | |  Allowed values: `present` the cron will be present in the `cron_file`. `absent` the `cron_file` will be removed including all jobs within the file. |
| `job` | Yes | | The command to execute. |
| `cron_file` | Yes | | Filename of the crontab-file, any paths will be stripped. Generally for compatibility it is adviced to make up the filenames of letters, digits, underscores ('_') or hyphens  ('-'). |
| `backup` | Yes | true | Allowed values: `true` if changed a backup of `cron_file` will be created. `false` if changed no backup of `cron_file` will be created. |
| `env_vars` | No | | The `env_vars` variables contains environmental variables to be added to the top of the `cron_file`. The variables will be added in reverse order; E.g. in the example playbook below the MAILTO variable will be at the top of the file. `name` name of the variable. `value`. value of the variable. `state` selects whether the environment variable will be added to the `cron_file`. Allowed values for `state` are: `present` the variable will be added to `cron_file` or `absent` the variable will be removed from `cron_file`. |


Dependencies
------------

None.

Example Playbook
----------------


    - hosts: servers

      vars:
        ansible_role_crond_options:
          - name: testjob
            minute: "5"
            hour: "14"
            day_of_month: "15"
            month: "5"
            day_of_week: "6"
            user: root
            state: present
            job: "/bin/false > /dev/null 2>&1"
            cron_file: testjob123
            backup: yes
            env_vars:
              - name: SHELL
                value: /bin/bash
                state: present
              - name: PATH
                value: /sbin:/bin:/usr/sbin:/usr/bin
                state: present
              - name: MAILTO
                value: root
                state: absent

      roles:
         - ansible_role_crond

License
-------

MIT

Author Information
------------------

Mattias Jonsson