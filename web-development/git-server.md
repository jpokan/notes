# Git server on a Raspberry Pi 2B

Create a new `git` user, its better to have a different user to manage their own job 

```sh
    $ sudo adduser git
    $ su git
```

Read [ssh](ssh.md) if you want to setup passwordless login to your git user.

Create a --bare repository or copy the .git directory inside your working project.

```sh
    $ git clone --bare my_project my_project.git
```

or

```sh
    $ cp -Rf my_project/.git my_project.git
```

After the new --bare repository has been created, you can upload it like this:

```sh
    $ scp -r my_project.git user@git.example.com:/srv/git
```


##### reference links ---
[git server](https://git.zx2c4.com/cgit/about/)

--- tags ---
##### :git: :rasbperrypi: :server:
