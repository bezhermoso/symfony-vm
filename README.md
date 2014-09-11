symfony-vm
==========

A template for VM setups for your Symfony2 projects using [Vagrant](vagrantup.com) and [Puppet](puppetlabs.com).

###Usage

```
$ composer create-project symfony/framework-standard-edition awesome-project 2.3.*
$ cd awesome-project
$ git init

$ git add ...
$ git commit -m "My first commit!"

/* git add everything and create a first commit. Your working directory must be clean otherwise the next command won't work */

$ git remote add symfony-vm https://github.com/bezhermoso/symfony-vm.git
$ git subtree add --prefix=vm symfony-vm master
$ cd vm
$ vagrant up
```

(For existing projects, simply `cd` to your project root and skip directly to `git remote add...`)

A `vm` directory will be created in your project root. You can commit this directory along with your code-base and version it along with your project.

(You could use `git submodule` if you want, but see the pros of [git-subtree](http://git-scm.com/book/en/Git-Tools-Subtree-Merging) and give it a whirl.)

__Note on clearing cache__:

Make sure you `cache:clear` from within the VM and not from the host machine so that file paths within Symfony cache will correspond to the project's location within the VM, not your host machine:

```
$ vagrant ssh
$ cd /symfony
$ php app/console cache:clear
```

You have to do this at least once after `composer create-project`, since the cache is built during the installation process.

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

You can enable/disable any of these as you want by editing `vm/puppet/config.yaml`. See the `install` entry within each package section (i.e., `sendmail -> install`)

####Shared folders

Your Symfony2 application will be located in `/symfony` in the VM.

__For Windows users__: Since Windows does not support NFS, you will need to leave blank the `sync_type` values under `vagrantfile-local -> vm -> synced_folder` items in `puphpet/config.yaml`.

####IP and Ports

By default, the VM's IP is `47.37.13.37`. Apache2 is served through port `8080`, and can be accessed through the host machine's port `8000`, and lastly, SSH port `22` can be accessed through the host machine's port `2200`. 

You can access your Symfony application through your browser at `http://47.37.13.37:8000`, or `http://localhost:8000`.

See `Vagrantfile` and `puppet/config.yaml` for the relevant sections where these are configured. Modify them as needed.

####MySQL access:

```
User:     root
Password: root
Database: symfony
```

(Alternatively, there is also a `symfony` user with the password `symfony`)

-----------------------------------------
_The Puppet manifest contained herein is generated via [PuPHPet](https://puphpet.com)._
