monit
=========

Install the monit program and configure with some basic pid monitoring targets.
Target processes are restarted if they stop/fail.

Create as a basic install role to build more complicated monitoring solutions.
Consider using systemd is all you need are process retarts.

Tested using Ansible 2.1.2 on:

CentOS 7
Debian Jessie
Ubuntu Trusty

Requirements
------------

Ansible 2+

Requirements
------------

Obviously it requires that the target programs are installed

Role Variables
--------------

Default variables:

```yaml
---
# add a system monitor for cpu/memory usage
monitor_system: yes

# default install directories
monit_conf_directory: /etc/monit/conf.d/
monit_control_file: /etc/monit/monitrc
monit_state_directory: /var/lib/monit/

# file: monit/defaults/main.yml

monit_email_enable: yes
monit_notify_email: "me@localhost"

monit_logfile: "syslog facility log_daemon"

monit_poll_period: 60
monit_poll_start_delay: 120

monit_eventqueue_enable: yes
monit_eventqueue_directory: "/var/lib/monit/events"
monit_eventque_slots: 100

monit_mailformat_from: "monit@{{inventory_hostname}}"
monit_mailformat_subject: "$SERVICE $EVENT"
monit_mailformat_message: "Monit $ACTION $SERVICE at $DATE on $HOST: $DESCRIPTION."

monit_mailserver_host: "localhost"
# monit_mailserver_port:
# monit_mailserver_username:
# monit_mailserver_password:
# monit_mailserver_encryption:
monit_mailserver_timeout: 60

monit_process_list: []

# Example:
#monit_process_list:

  # The pid path is absolute, this is required.
  #- pid: '/var/run/foo.pid'

    # The process is simply the process name, defaults to the pid's basename.
    #process: 'foo'

    # Set a timeout, defaults to 60 seconds.
    #timeout: 60

    # The systemd style to start/stop a process, you can change this per process.
    #start: 'service process start'
    #stop: 'service process stop'

monit_port: 3737
monit_address: "localhost"
monit_allow: ["localhost"]
# monit_username:
# monit_password:
monit_ssl: no
monit_cert: "/etc/monit/monit.pem"

monit_monitors_sshd_port: 22
```
OS specific variables. Processes are named differently and installation directories are slightly different between OS. These files serve as an override of the main default config set, and to specify the processes you want to monitor.

```yaml
monit_conf_directory: /etc/monit.d/
monit_control_file: /etc/monitrc

monit_process_list:

    - pid: '/var/run/sshd.pid'
      process: 'sshd'
      timeout: 60
      start: '/sbin/service ssh start'
      stop: '/sbin/service ssh stop'
    - pid: '/var/run/crond.pid'
      process: 'crond'
      timeout: 60
      start: '/sbin/service crond start'
      stop: '/sbin/service crond stop'

```


Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      become: true
         - monit

License
-------

MIT
