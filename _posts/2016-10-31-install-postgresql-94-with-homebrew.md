---
layout: post
title: Install PostgreSQL 9.4 with Homebrew
tags: mac,postgresql
---

`Homebrew` is a great tool on Mac to install 3rd-party softwares and unix softwares like `libxml2`, `MySQL`, `Redis`, etc. Today let's install `PostgreSQL 9.4` with it.

1. First, install it with `Homebrew` command:

    ```bash
    brew install homebrew/versions/postgresql94
    ```

2. And then, start `postgresql` service with following command:

    ```
    brew services start homebrew/versions/postgresql94
    ```

    This command will make sure `postgresql` is running every time you restart your computer. If you just want to start it one time, you could try command: `postgres -D /usr/local/var/postgres`

3. Last thing you need to do is to create a superuser `postgres`:

    ```
    createuser -d -a -s -P postgres
    ```

    After entering your password, you have a running postgresql now.
