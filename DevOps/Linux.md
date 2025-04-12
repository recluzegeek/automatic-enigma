# Commands

## cut, awk, sed

```bash

man less | grep '\-R'

grep -R SELINUX /etc/*

grep -R SELINUX /etc/* --color=always| grep -v '\#' | less -R

date | cut -d' ' -f8

cut -d: -f1 /etc/passwd

date | awk -F' ' '{ print $7 }'

uptime | awk -F' ' '{ print $5, $6 }'

find /etc/  -name host*

updatedb

locate /etc/ -name host*

```

## Redirection

- `1>>` refers to standard output
- `2>>` refers to standard error
- `&>>` redirect everything, whether its output or error

## User Management

- `useradd ansible`, `useradd docker`, `useradd aws` for creating users
- `groupadd devops` for creating groups
- `usermod -aG devops ansible` for adding ansible to devops groups (supplementary)
- `usermod -ag devops ansible` for adding ansible to devops groups (primary), see the difference of `g`
- `lsof -u ansible` lists opened files by user. Useful for viewing what user is doing before doing any modifications to user.
- `userdel -r aws` for deleting users along with their home directory
- `id ansible` provides uid, gid (primary group id) and supplementary groups user belongs to.

## Permissions

`chown aws:devops /tmp/devops` for changing owner to aws user and devops group
`chmod go-rx /tmp/devops` for removing permissions of read and execute for group and others
`chmod 722 /tmp/devops` for doing the above thing but with numeric method
`ls -ld some_directory` provides listing metadata for directory and not its content

Read bit = 4
Write bit = 2
Execute bit = 1

722 => first is for user, second is for the group and third is for others

`chmod 640 myfile` gives user read and write permissions, read to group and none permission to others.

Adding users or groups to the sudoers group or file. There's file `/etc/sudoers` that contains information about sudo or users
that've admin privileges set. We should only be editing this file via only `visudo` as it provides extra protection against
human errors or syntax error after saving modifications. Its' better to edit the `/etc/sudoers.d/` directory, copy existing files
and modify them to your needs like:

```bash

cp /etc/sudoers.d/vagrant /etc/sudoers.d/devops

# now edit the /etc/sudoers.d/devops file
vagrant ALL=(ALL) NOPASSWD:ALL # NOPASSWD ensures that you won't be asked for password when you want to switch to root or high privilege actions

# you can also add groups to sudoers.d as well
# we've added devops group to sudoers and every member of the devops group is now an admin
# and they won't be asked for passwd as well
%devops ALL=(ALL) NOPASSWD:ALL
```

## Services

## Processes

- `ps aux`, `top` are some commands to see resource utilization. `systemd` is the first process to start and then it
starts other processes. In older system, it used to be `init` service.
- [kthreads] processes in `[]` are kernel threads. `ps -ef` tells the ppid of the process as well.
- `kill pid` asks the process to stops its operations and all of its children as well.
- `kill -9 pid` forcefully quits the process, but the children becomes orphan and are adopted by process of pid 1 usually systemd.

```bash
ps -ef | grep 'httpd' | grep -v 'grep' | awk '{ print $2 }' | xargs kill -9 # kills the orphan processes as well
```

The above command will filter the httpd, takes the pid and forcefully kill all of them.

## Archiving

```bash
# archive syntax: tar -czvf archive_name file_or_directory_to_archive
tar -czvf httpd_logs_10032025.tar.gz /var/log/httpd/

# archive extraction syntax: tar -xzvf 
tar -xzvf httpd_logs_10032025.tar.gz

zip -r httpd_logs_10032025.zip /var/log/httpd/

unzip httpd_logs_10032025.zip
```

## Server Management

## Tips and tricks

- `export EDITOR=vim` for setting default editor
- `touch file{1..10}.txt` creates 10 files of the naming format file1.txt file2.txt, like a list
- Try `echo {a..f}` or `echo {{a..b}}` or `echo {1..299}`
- If we create `touch file{1..200}.txt`, and then wanna remove first 100 files, we can use lists like this, `rm file{1..100}.txt`

## Building Systemctl configuration for Apache Tomcat

Systemd files are located in `/etc/systemd/system` folder. First, we need to create user tomcat to start, stop the tomcat.

```bash
useradd --home-dir /opt/tomcat --shell /sbin/nologin tomcat
cp -r tomcat/* /opt/tomcat
chown -R tomcat.tomcat /opt/tomcat # change ownership

nvim /etc/systemd/system/tomcat.service # create systemd service for tomcat
```

Needs to create 3 sections, `[Unit]`, `[Service]`, and `[Install]`.

- `[Unit]` section includes global defaults/settings includes Description, or dependencies our service rely on.

    ```bash
        [Unit]
        Description=Tomcat Server
        After=network.target
    ```

- `[Service]` section tells which user will initiate the service, any environment variables, setting the working directory, `ExecStart` and `ExecStop` params defining script path which will start and stop the service

    ```bash
        [Service]
        Type=forking
        User=tomcat
        Group=tomcat
        WorkingDirectory=/opt/tomcat

        Environment=JAVA_HOME=/usr/lib/jvm/jre
        ...and other

        ExecStart=/opt/tomcat/bin/startup.sh
        ExecStop=/opt/tomcat/bin/shutdown.sh
    ```

After making changes to systemd, we need to reload its daemon by, `systemctl daemon-reload` and start our new service to check for any syntax errors, using `systemctl start tomcat`. Enable the service file at boot time using `systemctl enable tomcat`
