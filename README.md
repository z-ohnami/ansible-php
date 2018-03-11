# ansible-php

```
$ ansible-playbook -u {user name} --private-key={key file}  -i inventory.ini test_playbook.yaml
$ scp -i {key file} {user name}@{host name}:/etc/php-fpm.d/www.conf php/www.conf
```

```
sudo yum install nginx
sudo yum install --enablerepo=remi,remi-php70 php-fpm
sudo vim /etc/php-fpm.d/www.conf
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
sudo vim /etc/nginx/nginx.conf
sudo systemctl start nginx
sudo systemctl enable nginx
sudo vim /usr/share/nginx/html/test.php
```
