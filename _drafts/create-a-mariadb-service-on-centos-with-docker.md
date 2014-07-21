---
title: Create a mariadb service on CentOS with Docker
layout: post
---

Learning Docker for a long time, this is my first time to create a service with it. In Docker's concept, each service should be in a container, and your application may comes with many containers.

This time, I am gonna create a mariadb service.

1. First of all, we need a `Dockerfile` to create our container:

        FROM centos:latest
        MAINTAINER David Xie "david.scriptfan@gmail.com"

        EXPOSE 3306

2. With this file, we can get a clean CentOS. Let's try to install mariadb. After surfing from internet, we know that if we want to install the latest version of mariadb, we need to use its own repo. This is easy, just create a repo file and use it during installing mariadb. Our `Dockerfile` will look like this:

        FROM centos:latest
        MAINTAINER David Xie "david.scriptfan@gmail.com"

        ADD mariadb.repo /etc/yum.repos.d/mariadb.repo

        RUN yum install -y hostname MariaDB-server
        RUN yum clean all

        EXPOSE 3306

    And `mariadb.repo`:

        [mariadb]
        name = MariaDB
        baseurl = http://yum.mariadb.org/10.0/centos6-amd64
        gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
        gpgcheck=1

3. Now we have a CentOS with mariadb installed, but how can I connect to it, and how to set password for `root`? Let's add more files for it.

    `server.cnf` is used to make sure we are not blocked by mariadb:

        [mysqld]
        bind-address=0.0.0.0
        console=1
        general_log=1
        general_log_file=/dev/stdout
        log_error=/dev/stderr
        collation-server=utf8_unicode_ci
        character-set-server=utf8

    `mariadb.sql` is used to update password for `root` to `root` and grant all privilliges to all IPs.

        USE mysql;
        GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
        FLUSH PRIVILEGES;
        UPDATE user SET password=PASSWORD("root") WHERE user='root';
        FLUSH PRIVILEGES;

    `mariadb.sh` is used to apply all updates and start mariadb service for us:

        #!/bin/sh
        chown -R mysql:mysql /var/lib/mysql
        mysql_install_db --user mysql > /dev/null

        mysqld_safe --user mysql &

        sleep 5s

        mysql -v < /root/mariadb.sql

        sleep 5s

        ps -wef | grep mysql | grep -v grep | awk '{print $2}' | xargs kill -9

        mysqld_safe --user mysql

4. Final thing, we need to update our `Dockerfile`:

        # application container for MySQL
        # VERSION 2014-07-14

        FROM centos:latest
        MAINTAINER David Xie "david.scriptfan@gmail.com"

        ADD mariadb.repo /etc/yum.repos.d/mariadb.repo
        ADD mariadb.sql /root/mariadb.sql
        ADD server.cnf /etc/my.cnf.d/server.cnf
        ADD mariadb.sh /root/mariadb.sh

        RUN yum install -y hostname MariaDB-server
        RUN yum clean all
        RUN chmod +x /root/mariadb.sh

        EXPOSE 3306

        CMD ["/root/mariadb.sh"]

5. We already have everything we need, we can create our container now.

        docker build --rm=true --no-cache=true -t mariadb .
        docker run -d -p 3306:3306 mariadb

    After 10s, our container is ready to use. If you are using Docker on `boot2docker`, use `boot2docker ip` to get correct IP for it. Then you can connect your mariadb with `IP:3306`.