# ansible-php

* GCE側でcloudsql APIを有効化する
* CloudSQL側でGCEのサービスアカウントをCloudSQLクライアントとして登録する
* CloudSQL側のデータベースとユーザー権限を調整しておく

```
$ ansible-doc copy
$ ansible-playbook -u {user name} --private-key={private key file}  -i inventory.ini test_playbook.yaml
```

```
sudo lsblk
sudo mkfs.ext4 -m 0 -F -E lazy_itable_init=0,lazy_journal_init=0,discard /dev/sdb
sudo mkdir -p /contents
sudo mount -o discard,defaults /dev/sdb /contents
sudo chmod a+w /contents/
sudo cp /etc/fstab /etc/fstab.backup
sudo blkid /dev/sdb
sudo vim /etc/fstab
UUID=[UUID_VALUE] /contents ext4 discard,defaults,nofail 0 2
```

```
$ ansible-playbook -u {user name} --private-key={key file}  -i inventory.ini test_playbook.yaml
$ scp -i {key file} {user name}@{host name}:/etc/php-fpm.d/www.conf php/www.conf
```

```
sudo timedatectl set-timezone Asia/Tokyo
sudo setenforce Permissive
```

```
# vi /etc/selinux/config
SELINUX=enforcing

# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=enforcing
# SELINUXTYPE= can take one of three two values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are protected.
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted
```

```
sudo yum install nginx
sudo rpm --import http://rpms.famillecollet.com/RPM-GPG-KEY-remi
sudo rpm -ivh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
sudo yum install mysql
sudo yum install --enablerepo=remi,remi-php70 php-fpm
sudo yum install --enablerepo=remi,remi-php70 php-mysqlnd
sudo vim /etc/php-fpm.d/www.conf
sudo vim /etc/nginx/nginx.conf
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
sudo systemctl start nginx
sudo systemctl enable nginx
sudo vim /usr/share/nginx/html/test.php
sudo yum install wget
sudo curl -o cloud_sql_proxy https://dl.google.com/cloudsql/cloud_sql_proxy.linux.amd64
sudo mv cloud_sql_proxy /usr/local/bin/
sudo chmod +x /usr/local/bin/cloud_sql_proxy
sudo mkdir /cloudsql; sudo chmod 777 /cloudsql
# enable cloud sql at GCE
nohup cloud_sql_proxy -dir=/cloudsql -instances={db instance} &

```

```
curl -o wordpress-4.9.4-ja.tar.gz https://ja.wordpress.org/wordpress-4.9.4-ja.tar.gz
tar zxvf wordpress-4.9.4-ja.tar.gz
sudo chown -R nginx:nginx wordpress
```

```
certbot-auto certonly -n --webroot -w DOCUMENT_ROOT -d DOMAIN --email EMAIL
```


```
define('DB_NAME', 'wordpress');

/** MySQL database username */
define('DB_USER', {proxy user});

/** MySQL database password */
define('DB_PASSWORD', {password});

/** MySQL hostname */
define('DB_HOST', 'localhost:/cloudsql/{db instance}');

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');
```
