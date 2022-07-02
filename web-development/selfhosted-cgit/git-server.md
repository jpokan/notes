# Git server on a Raspberry Pi 2B

Create a new `git` user, its better to have a different user to manage their own job 

```sh
$ sudo adduser git
$ su git
```
--- recomended ---
Read [ssh](ssh.md) to secure access to your git-server.

**Create** a --bare repository or copy the .git directory inside your working project.

```sh
$ git clone --bare my_project my_project.git
```

**Or copy** the .git directory inside your working project.

```sh
$ cp -Rf my_project/.git my_project.git
```

After the new --bare repository has been created, you can upload it to the server like this:

```sh
$ scp -r my_project.git user@git.your_domain.com:/srv/git
```

> `scp` secure copy
>
> `-r` recursively (everything inside)
>
> `my_project.git` the newly created project folder
>
> `user` the user we created that has access to your server to manage git repos
>
> `git.your_domain.com:` the domain of the server
>
> `/srv/git` the path to upload the repository on the server.

##### reference links ---
- [git server](https://git-scm.com/book/en/v2/Git-on-the-Server-Getting-Git-on-a-Server)

--- tags ---
##### :git: :rasbperrypi: :server:
