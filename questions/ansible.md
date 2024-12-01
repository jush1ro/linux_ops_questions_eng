## Ansible

1. What difference *raw*, *command* and *shell*?

<details>
  <summary>Answer</summary>

The *raw* module differs from *command* and *shell* in that it does not perform additional processing on command execution. This additional processing is present in almost every Ansible module. The *raw* module passes the command as is, in "raw" form, without checks. The *command* and *shell* modules differ in that in the *command* module, the command is executed without going through the `/bin/sh` shell. Therefore, variables defined in the shell and redirections - pipes will not work. The *shell* module executes commands through the default shell `/bin/sh`. Therefore, shell variables and redirections will be available there.

</details>

2. All servers must have a set of users with access via ssh key, the standard user module does not allow adding an ssh key to authorized_keys. Suggest a solution.

<details>
  <summary>Answer</summary>

1. Use the `authorized_key` module to add keys.
2. Use the `shell` module to manually add the key using the `cat {{ PUBLIC_SSH_KEY }} >> /home/{{ USER }}/.ssh/authorized_keys` command. In this case, the Jinja2 templates PUBLIC_SSH_KEY and USER must be specified.

</details>

3. There are user groups that should not be created on all servers. How to limit the creation of users?

<details>
  <summary>Answer</summary>

Group the servers on which user groups should be created in the inventory or write a condition in the playbook that receives a list of servers on which the task must be performed.

</details>

4. The new server does not have Python installed, which is required for Ansible to work. How do I install Python on the server using Ansible?

<details>
  <summary>Answer</summary>

Use the `raw` module, which you pass the command to install python on the server. The `raw` module takes the command without any additional Python processing and executes it on the server.

</details>

5. What is a role in Ansible? What does an Ansible role contain?

<details>
  <summary>Answer</summary>

An Ansible role is a structured playbook containing, at a minimum, a set of tasks and, additionally, event handlers, variables (default and vars), files, templates, descriptions and dependencies (metadata), and tests.

</details>

6. Ansible roles have *vars* and *default* directories. What do they contain and how do they differ?

<details>
  <summary>Answer</summary>

Ansible applies an order of precedence to variables. The list below is in ascending order of precedence.

1. command line values (for example, -u my_user, these are not variables)
2. role defaults (defined in role/defaults/main.yml)
3. inventory file or script group vars
4. inventory group_vars/all
5. playbook group_vars/all
6. inventory group_vars/*
7. playbook group_vars/*
8. inventory file or script host vars
9. inventory host_vars/*
10. playbook host_vars/*
11. host facts / cached set_facts
12. play vars
13. play vars_prompt
14. play vars_files
15. role vars (define in role/vars/main.yml)
16. block vars (only tasks in `block`)
17. task vars (only tasks)
18. include_vars
19. set_facts / registered vars
20. role (and include_role) params
21. include params
22. extra vars (for example, -e "user=my_user")(всегда приоритетнее)

Accordingly, variables in *vars* will have higher priority than those in *defaults*.

</details>

7. Ansible roles have *file* and *templates* directories. What do they contain and how do they differ?

<details>
  <summary>Answer</summary>

*files* - contains files that will be copied to the configured hosts; it can also contain scripts that will later be run on the hosts.

*templates* - contains file templates with variables.

</details>

8. By default, Ansible runs all tasks in the list in parallel on all hosts specified in `hosts`. How can I make tasks run sequentially across hosts?

<details>
  <summary>Answer</summary>

You need to set the `serial: 1` parameter to define the number of hosts on which the tasks will be executed in parallel. The value 1 will mean that all tasks will be executed in parallel on 1 host at a time.

Documentation link: https://docs.ansible.com/ansible/latest/user_guide/playbooks_strategies.html#setting-the-batch-size-with-serial

</details>