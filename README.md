How To install Sphinx SE AS plugin Mysql on Ubuntu 16.04
==========

### Installation:

#### Install package

```bash
	sudo apt-get install openssl cmake gcc cpp bison gcc
```
#### Go to your homedir
```bash
	cd ~/
```

#### Download MySQL-5.7

```bash
	wget http://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.12.tar.gz
	tar -xzvf mysql-5.7.12.tar.gz
```

#### Download Sphinx-Se plugin compatible with MySQL-5.7

```bash
	git clone git@github.com:riden/sphinx.git
	cd sphinx
	git checkout mysqlse-mysql-5.7
```
#### Copy the mysqlse directory from sphinx to mysql:
```bash
	cd ..
	cp -R sphinx/mysqlse/ mysql-5.7.12/storage/sphinx
```
#### Place into mysql Folder:

```bash
	cd mysql-5.7.12
	sh BUILD/autorun.sh
	cmake . -DCMAKE_INSTALL_PREFIX=/opt/mysql-5.7.12 \
      -DMYSQL_DATADIR=/var/mysql -DDOWNLOAD_BOOST=ON \
      -DWITH_BOOST=/var/tmp/my_boost
	make
```

#### Copy the Sphinx .SO files to your MySQL plugin directory:
```bash
	sudo cp storage/sphinx/ha_sphinx.so /usr/lib/mysql/plugin/
```

#### Login to mysql console as root user.

```bash
mysql -u root -p -h localhost
```

#### Install the Sphinx plugin
##### mysql>

```sql
	INSTALL PLUGIN sphinx SONAME 'ha_sphinx.so';
```

	SPHINX YES Sphinx storage engine 2.3.2-dev NO NO NO

```sql
	SHOW ENGINES;
	quit
```

#### If you need to uninstall the sphinx plugin for some reason later on, this is how you do it:
##### mysql>
```sql
	UNINSTALL PLUGIN sphinx;
	SHOW ENGINES;
	quit
```