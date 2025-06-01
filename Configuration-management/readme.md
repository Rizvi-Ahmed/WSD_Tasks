This is the answer to the task 1 which is configuration managment. The ansible playbook has been divided into roles to exhibit the resusabilty of ansible playbooks.

Each task has its own playbook with its corresponding role in roles folder.

## For ansible host configuration as per Task 1 the following command is executed. The output is given in task-1-output.txt

```bash
ansible -i inventory/servers.ini all  -l remote-servers-ntp --user rizvi -m setup
```
The -l flag is used to limit the host on which the playbook will be executed.

## For task 2 the following command is executed.

```bash
ansible-playbook -i inventory/servers.ini playbooks/cron-setup.yaml -l remote-servers-ntp --become --user rizvi --become-method sudo --become-user root --ask-become-pass
```

The cron setup from one the server is given below

```bash
root@app-vm1:/home/rizvi# crontab -l
#Ansible: Logrotate every 10 mins between 2h - 4h
*/10 2-3 * * * /usr/sbin/logrotate /etc/logrotate.conf
root@app-vm1:/home/rizvi#

```
## For task 3 to setup ntpd with the given config the following command is executed.

```bash
ansible-playbook -i inventory/servers.ini playbooks/ntp-config.yaml -l remote-servers-ntp --become --user rizvi --become-method sudo --become-user root --ask-become-pass
```

The ntpd config from one the server is given below

```bash
rizvi@db-vm1:~$ cat /etc/ntpd.conf
tinker panic 0
restrict default kod nomodify notrap nopeer noquery
restrict -6 default kod nomodify notrap nopeer noquery
restrict 127.0.0.1
restrict -6 ::1
server   192.168.0.252 minpoll 4 maxpoll 8
server   192.168.0.251 minpoll 4 maxpoll 8
server   192.168.0.0 # local clock
fudge    192.168.0.0 stratum 10
driftfile /var/lib/ntp/drift
keys     /etc/ntp/keys
```


#### For nagios configuration with given template the following ansible command is used.

```bash
ansible-playbook -i inventory/servers.ini playbooks/nagios-config.yaml -l monitoring-server --become --user rizvi --become-method sudo --become-user root --ask-become-pass
```

The nagios template configuration is generated using a template that is rendered for each that needs to be monitored.

The template config that is generated  given below

```bash
rizvi@monitoring:~$ cd /usr/local/nagios/etc/objects/
rizvi@monitoring:/usr/local/nagios/etc/objects$ ls
app-vm1.fra1.internal.cfg  db-vm1.fra1.db.cfg  web-vm1.fra1.web.cfg
rizvi@monitoring:/usr/local/nagios/etc/objects$ cat app-vm1.fra1.internal.cfg
define host {
    host_name               app-vm1.fra1.internal
    address                 192.168.0.150
    check_command           check-ping
    active_checks_enabled   1
    passive_checks_enabled  1
}

define service {
    service_description     ntp_process
    host_name               app-vm1.fra1.internal
    check_command           check_ntp
    check_interval          10
}

rizvi@monitoring:/usr/local/nagios/etc/objects$

```



