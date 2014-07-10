symfony-vm
==========

Vagrant setup template for Symfony2 projects.

The Puppet manifests contained herein is generated via [PuPHPet](https://puphpet.com).

###Usage

```
$ composer create-project symfony/framework-standard-edition awesome-project 2.3.*
$ cd awesome-project
$ git init
$ git remote add symfony-vm https://github.com/bezhermoso/symfony-vm.git
$ git subtree add --prefix=vm symfony-vm master
$ cd vm
$ vagrant up
```

(For existing projects, simply `cd` to your project root and skip directly to `git remote add...`)

A `vm` directory will be created in your project root, which you can commit along with your code-base and therefore version it along with your project.

(You could use `git submodule` if you want, but see the pros of [git-subtree](http://git-scm.com/book/en/Git-Tools-Subtree-Merging) and give it a chance.)

__Note__:

Make sure you `cache:clear` from within the VM and not from the host machine so that file paths within Symfony cache will correspond to the project's location within the VM, not your host machine:

```
$ vagrant ssh
$ cd /symfony
$ php app/console cache:clear
```

###Information

The VM is provisioned with the following packages:

* PHP 5.5 (with XDebug, cli, curl, int, mcrypt, memcached, mysql, imagick, gd, and sqlite)
* MySQL
* SQLite
* Git
* Composer
* Sendmail
* RabbitMQ
* ElasticSearch

####Shared folders

Your Symfony2 application will be located in `/symfony` in the VM.

__For Windows users__: Since Windows does not support NFS, you will need to leave blank the `sync_type` values under `vagrantfile-local -> vm -> synced_folder` items in `puphpet/config.yaml`.

####IP and Ports

By default, the VM's IP is `47.37.13.17`. Apache2 is served through port `8080`, and can be accessed through the host machine's port `8000`, and lastly, SSH port `22` can be accessed through the host machine's port `2200`. 

You can access your Symfony application through your browser at `http://47.37.13.17:8000`.

See `Vagrantfile` and `puppet/config.yaml` for the relevant sections where these are configured. Modify them as needed.

####MySQL access:

```
User:     root
Password: root
Database: symfony
```

(Alternatively, there is a `symfony` MySQL user with the password `symfony`)


