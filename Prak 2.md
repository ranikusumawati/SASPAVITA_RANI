**Laporan Modul 2**

- Pavita Sherintama Giantoro (1202190051)
- Rani Kusumawati (1202192029)

---

1. **Rubah LXC landing dengan ubuntu focal (destroy n create, same ip, same name)**

   - Menghapus container ubuntu_landing sebelum membuat container baru dengan nama yang sama 
     ```
     lxc-destroy ubuntu_landing
     ```
     ![1](https://user-images.githubusercontent.com/78127403/144471046-fbe09e50-430b-4fc8-a128-a7caa77c2a72.jpg) 

     ![2](https://user-images.githubusercontent.com/78127403/144471674-fb0bbc80-d473-406a-ae6e-90472838087d.jpg)

     ![4](https://user-images.githubusercontent.com/78127403/144471859-3825b7b3-35f3-4d74-a677-06bea2a69c5e.jpg)  


   - Buat container lxc ubuntu versi focal baru 
     ```
     lxc-create -n ubuntu_landing -t download -- --dist ubuntu --release focal --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
     lxc-start ubuntu_landing
     lxc-attach ubuntu_landing
     ```

     ![5](https://user-images.githubusercontent.com/78127403/144472116-c910974b-af64-4c42-ac19-2c9dcb720cd2.jpg)
     

   - Konfigurasi IP static di dalam nano
     ```
     nano /etc/netplan/00-installer-config.yaml
     ```

     ![6](https://user-images.githubusercontent.com/78127403/144472667-97d37a95-cb31-4760-8c8f-b52df6fdd823.jpg)


     ```
     netplan apply
     ```
     ![7](https://user-images.githubusercontent.com/78127403/144473058-6f4c51e3-f91e-4d79-b65f-43ce22d126a0.jpg)

     

     

   - Kemudian install open ssh server 
     ```
     apt-get install openssh-server -y
     ```
     ![8](https://user-images.githubusercontent.com/78127403/144473064-97dad895-d08d-44cb-930f-7c6e52e283e5.jpg)

     
   
   - Lalu lakukan konfigurasi ssh
     ```
     nano /etc/ssh/sshd_config
     ```
     
     ![9](https://user-images.githubusercontent.com/78127403/144473295-7019f43f-1750-4e2f-9685-2acaa6b43a61.jpg)

     
     
   - Buat password baru
     ```
     service sshd restart
     passwd
     ```
     ![10](https://user-images.githubusercontent.com/78127403/144473432-b485cc2c-2b1c-4e7e-99d2-e95fc78e383e.jpg)

     
     
     
   - Cek ssh 
     ```
     ssh root@10.0.3.103
     ```
     ![11](https://user-images.githubusercontent.com/78127403/144473477-e5ba51df-7b31-4075-bcd4-8b988d0bfe0d.jpg)

     
     





2. **Rubah LXC php7 dengan ubuntu focal (destroy n create, same ip, same name)**

   - menghapus container ubuntu php 7.4
     ```
     lxc-destroy ubuntu_php7.4 -f
     ```
     ![1](https://user-images.githubusercontent.com/78127403/144475789-d66aee25-992b-487d-80a9-91074618056f.jpg)


     

   - buat container lxc ubuntu versi focal 

     ```
     lxc-create -n ubuntu_php7.4 -t download -- --dist ubuntu --release focal --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
     ```

     

     ```
     lxc-start ubuntu_php7.4
     lxc-attach ubuntu_php7.4
     apt-get install nano net-tools curl
     ```

     ![2](https://user-images.githubusercontent.com/78127403/144476232-de9cc625-b4df-4677-a701-f1d48f9ce88c.jpg)


   - konfigurasi menjadi IP statik 
     ```
     nano /etc/netplan/10-lxc.yaml
     ```
     ![3](https://user-images.githubusercontent.com/78127403/144476410-81027bca-d7ae-4ffb-9a76-7a079e7e5edd.jpg)



     ```
     netplan apply
     ```
     ![4](https://user-images.githubusercontent.com/78127403/144476482-6074d40d-0ee5-4fab-8b6c-83048d9e685f.jpg)


     

   - kemudian install open ssh server 
     ```
     lxc-start ubuntu_php7.4
     lxc-attach ubuntu_php7.4
     apt-get install openssh-server -y
     ```
     ![5](https://user-images.githubusercontent.com/78127403/144476786-d60d0afb-c8dc-4f6b-bdd1-b9b695e1b12e.jpg)


   - lalu lakukan konfigurasi ssh
     ```
     nano /etc/ssh/sshd_config
     ```
   
     ![6](https://user-images.githubusercontent.com/78127403/144476922-6cab201e-616f-4408-ab22-3dc141c6a891.jpg)

   
    - buat password baru
      ```
      service sshd restart
      passwd
      ```

      ![7](https://user-images.githubusercontent.com/78127403/144477113-aff507be-dbf5-4a08-befd-2c2876eafd73.jpg)

     
    - cek ssh      
      ```
      ssh root@10.0.3.103
      ```
     
     ![8](https://user-images.githubusercontent.com/78127403/144477196-91d7540c-b93c-4a4a-a9fa-e0467ba2be93.jpg)

     
     

3. **vm.local/**

   - Masuk mosul2-ansible dan buat file dengan nama install-laravel.yml
     ![1](https://user-images.githubusercontent.com/78127403/144485504-b86a2c70-8e0a-4b3d-87b7-5ea6d61b3f21.jpg)

     ![3](https://user-images.githubusercontent.com/78127403/144485508-9209c792-2ead-42b3-9f8e-2b20dc583994.jpg)

     ![2](https://user-images.githubusercontent.com/78127403/144485751-beaceef7-db91-49fe-b2b6-256c5a6107e8.jpg)

   - Install PHP
     ```
     ![4](https://user-images.githubusercontent.com/78127403/144486023-d1faa200-c864-469a-9c6b-535dacc53b1f.jpg)

     ![5](https://user-images.githubusercontent.com/78127403/144486029-42bb946e-d0a8-44e5-afa7-bc87c6a8e512.jpg)

     

   - membuat hosts untuk lxc
     ```
     nano hosts
     ubuntu_landing ansible_host=lxc_landing.dev ansible_ssh_user=root ansible_become_pass=1234
     ```
    

     

   - buat direktori yang akan dijalankan pada folder php dam install di nginxphp.yml
     ```
     ---
     - hosts: all
       become : yes
       tasks:
         - name: install nginx nginx extras
           apt:
            pkg:
              - nginx
              - nginx-extras
            state: latest
         - name: start nginx
           service:
            name: nginx
            state: started
         - name: menginstall tools
           apt:
            pkg:
              - curl
              - software-properties-common
              - unzip
            state: latest
              - name: "Repo PHP 7.4"
            apt_repository:
              repo="ppa:ondrej/php"
         - name: "Updating the repo"
           apt: update_cache=yes
         - name: Installation PHP 7.4
           apt: name=php7.4 state=present
         - name: install php untuk laravel
           apt:
            pkg:
               - php7.4-fpm
                   - php7.4-mysql
                   - php7.4-mbstring
                   - php7.4-xml
                   - php7.4-bcmath
                   - php7.4-json
                   - php7.4-zip
                   - php7.4-common
                        state: present
      ```
     

   - instalasi
     ```
     ansible-playbook -y hosts nginxphp.yml -k
     ```
     

   - buat folder installcomposer.yml
     ```
     ---
     -hosts: all
       become : yes
       tasks:
        - name: Download and install Composer
          shell: curl -sS https://getcomposer.org/installer | php
          args:
           chdir: /usr/src/
           creates: /usr/local/bin/composer
           warn: false
        - name: Add Composer to global path
          copy:
           dest: /usr/local/bin/composer
           group: root
           mode: '0755'
           owner: root
           src: /usr/src/composer.phar
           remote_src: yes
        - name: Composer create project
          become_user: root
          composer:
           command: create-project
           arguments: laravel/laravel landing 
           working_dir: /var/www/html
           prefer_dist: yes
          environment:
             COMPOSER_NO_INTERACTION: "1"
        - name: mengkopi file .env.example jadi .env
          copy:
           dest: /var/www/html/landing/.env.example
           src: /var/www/html/landing/.env
           remote_src: yes
        - name: mengganti konfigurasi .env
          lineinfile:
           path: /var/www/html/landing/.env
           regexp: "{{ item.regexp }}"
           line: "{{ item.line }}"
           backrefs: yes
          loop:
           - { regexp: '^(.*)DB_HOST(.*)$', line: 'DB_HOST=10.0.3.200' }
           - { regexp: '^(.*)DB_DATABASE(.*)$', line: 'DB_DATABASE=landing' }
           - { regexp: '^(.*)DB_USERNAME(.*)$', line: 'DB_USERNAME=arafah' }
           - { regexp: '^(.*)DB_PASSWORD(.*)$', line: 'DB_PASSWORD=1234 ' }
           - { regexp: '^(.*)APP_URL(.*)$', line: 'APP_URL=http://vm.local' }
           - { regexp: '^(.*)APP_NAME=(.*)$', line: 'APP_NAME=landing' }
        - name: Composer install ke landing
          composer:
            command: install
            working_dir: /var/www/html/landing
          environment:
            COMPOSER_NO_INTERACTION: "1"
        - name: generate php artisan
          args:
           chdir: /var/www/html/landing
          shell: php artisan key:generate
        - name: mengganti permission storage
          file:
           path: /var/www/html/landing/storage
           mode: 0777
           recurse: yes
     ```

     ![11](https://user-images.githubusercontent.com/78127403/144486458-0d312880-2fce-4441-b129-0a91875ed674.jpg)

     ![9](https://user-images.githubusercontent.com/78127403/144486464-4ed0d0d1-abf0-4bf9-911c-d663b06d3a94.jpg)

     ![10](https://user-images.githubusercontent.com/78127403/144486466-66d55228-905b-4dca-a3f2-db15a64f2b32.jpg)


   - instalasi
     ```
     ansible-playbook -y hosts installcomposer.yml -k
     ```

     

   - mengatur lxc_landing.dev
     ```
     server {
             listen 80;
             root /var/www/html/landing/public;
             index index.php index.html index.htm;
             server_name lxc_landing.dev;
         
             error_log /var/log/nginx/landing_error.log;
             access_log /var/log/nginx/landing_access.log;
         
             client_max_body_size 100M;
             location / {
                     try_files $uri $uri/ /index.php$args;
             }
             location ~\.php$ {
                     include snippets/fastcgi-php.conf;
                     fastcgi_pass unix:run/php/php7.4-fpm.sock;
                     fastcgi_param SCRIPTFILENAME $document_root$fastcgi_script_name;
             }
     }
     ```
     ![16](https://user-images.githubusercontent.com/78127403/144481504-d6cae3bf-e06c-4f2e-b8cc-3635f54aee90.jpg)

     ![17](https://user-images.githubusercontent.com/78127403/144481510-31f0c225-53cf-4f7f-aeab-c8ae606de357.jpg)


     

   - buat folder config.yml
     ```
     ---
     - hosts: all
       become : yes
       vars:
         domain: 'lxc_landing.dev'
       tasks:
        - name: stop apache2
          service:
           name: apache2
           state: stopped
           enabled: no
        - name: Write {{ domain }} to /etc/hosts
          lineinfile:
           dest: /etc/hosts
           regexp: '.*{{ domain }}$'
           line: "127.0.0.1 {{ domain }}"
           state: present
        - name: ensure nginx is at the latest version
          apt: name=nginx state=latest
        - name: start nginx
          service:
           name: nginx
           state: started
        - name: copy the nginx config file 
          copy:
           src: ~/ansible/laravel/lxc_landing.dev
           dest: /etc/nginx/sites-available/lxc_landing.dev
        - name: Symlink lxc_landing.dev
          command: ln -sfn /etc/nginx/sites-available/lxc_landing.dev /etc/nginx/sites-enabled/lxc_landing.dev
          args:
           warn: false
        - name: restart nginx
          service:
           name: nginx
           state: restarted
        - name: restart php7
          service:
           name: php7.4-fpm
           state: restarted
        - name: curl web
          command: curl -i http://lxc_landing.dev
          args:
           warn: false
      ```
     ![18](https://user-images.githubusercontent.com/78127403/144481206-b86a8bc1-d75f-44c5-9996-b336ab32a411.jpg)

     ![19](https://user-images.githubusercontent.com/78127403/144481213-1bc40046-4f4b-4ed5-822a-9c72544c9a27.jpg)


     

   - instalasi
     ```
     ansible-playbook -y hosts config.yml -k
     ```
     ![20](https://user-images.githubusercontent.com/78127403/144480694-1c8f079d-9c69-4e1a-b794-38c2dc8b6590.jpg)


     ![21](https://user-images.githubusercontent.com/78127403/144480688-f9edcf7f-cb18-4caa-98b2-1d5167d9fcbf.jpg)


   - buka vm.local untuk mengecek keberhasilan instalasi
   
     ![22](https://user-images.githubusercontent.com/78127403/144480541-49d4fbb9-4ee6-4d61-960c-8cce913b3e71.jpg)




4. **vm.local/blog**

   - masuk ansible untuk install wordpress
     ```
     cd ~/ansible/
     mkdir wordoress/
     cd wordpress/
     ```
     ![1](https://user-images.githubusercontent.com/92453574/144302776-0da1206a-124c-48cd-a4e9-37780b5c53e0.PNG)

     

   - membuat hosts untuk lxc
     ```
     nano hosts
     ubuntu_php7.4 ansible_host=lxc_php7.dev ansible_ssh_user=root ansible_become_pass=1234
     ```
     ![2](https://user-images.githubusercontent.com/92453574/144302781-2381b6ea-d4b9-4984-bcbb-6960732ad7f7.PNG)

     

   - buat direktori untuk tasks
     ```
     ---
     - hosts: all
       vars:
         domain: 'lxc_php7.dev'
       tasks:
        - name: delete apt chache
          become: yes
          become_user: root
          become_method: su
          command: rm -vf /var/lib/apt/lists/*
        - name: install requirement
          become: yes
          become_user: root
          become_method: su
          apt: name={{ item }} state=latest update_cache=true
          with_items:
           - nginx
           - nginx-extras
           - curl
           - wget
           - php7.4
           - php7.4-fpm
           - php7.4-curl
           - php7.4-xml
           - php7.4-gd
           - php7.4-opcache
           - php7.4-mbstring
           - php7.4-zip
           - php7.4-json
           - php7.4-cli
           - php7.4-mysqlnd
           - php7.4-xmlrpc
           - php7.4-curl
        - name: wget wordpress
          shell: wget -c http://wordpress.org/latest.tar.gz
        - name: tar latest.tar.gz
          shell: tar -xvzf latest.tar.gz
        - name: copy folder wordpress
          shell: cp -R wordpress /var/www/html/blog
        - name: chmod
          become: yes
          become_user: root
          become_method: su
          command: chmod 775 -R /var/www/html/blog/
        - name: copy .wp-config.conf
          copy:
           src=~/ansible/wordpress/wp.conf
           dest=/var/www/html/blog/wp-config.php
        - name: copy wordpress.conf
          copy:
           src=~/ansible/wordpress/wordpress.conf
           dest=/etc/nginx/sites-available/{{ domain }}
          vars:
           servername: '{{ domain }}'
        - name: Symlink wordpress.conf
          command: ln -sfn /etc/nginx/sites-available/{{ domain }} /etc/nginx/sites-enabled/{{ domain }}
       
        - name: restart nginx
          become: yes
          become_user: root
          become_method: su
          action: service name=nginx state=restarted
        - name: Write {{ domain }} to /etc/hosts
          lineinfile:
           dest: /etc/hosts
           regexp: '.*{{ domain }}$'
           line: "127.0.0.1 {{ domain }}"
           state: present
        - name: enable module php mbstring
          command: phpenmod mbstring
        - name: restart php
          become: yes
          become_user: root
          become_method: su
          action: service name=php7.4-fpm state=restarted
        - name: restart nginx
          become: yes
          become_user: root
          become_method: su
          action: service name=nginx state=restarted
     ```
     ![3](https://user-images.githubusercontent.com/92453574/144302782-bac5773b-ccd6-44e1-91a5-6991188695f7.PNG)

     

   - masuk pada template wp.conf
     ```
     <?php
     /**
      * The base configuration for WordPress
      *
      * The wp-config.php creation script uses this file during the installation.
      * You don't have to use the web site, you can copy this file to "wp-config.php"
      * and fill in the values.
      *
      * This file contains the following configurations:
      *
      * * MySQL settings
      * * Secret keys
      * * Database table prefix
      * * ABSPATH
      *
      * @link https://wordpress.org/support/article/editing-wp-config-php/
      *
      * @package WordPress
      */
     define( 'WP_HOME', 'http://vm.local/blog' );
     define( 'WP_SITEURL', 'http://vm.local/blog' );
     // ** MySQL settings - You can get this info from your web host ** //
     /** The name of the database for WordPress */
     define( 'DB_NAME', 'blog' );
     /** MySQL database username */
     define( 'DB_USER', 'admin' );
     /** MySQL database password */
     define( 'DB_PASSWORD', 'SysAdminSas0102' );
     /** MySQL hostname */
     define( 'DB_HOST', '10.0.3.200:3306' );
     /** Database charset to use in creating database tables. */
     define( 'DB_CHARSET', 'utf8' );
     /** The database collate type. Don't change this if in doubt. */
     define( 'DB_COLLATE', '' );
     /**#@+
      * Authentication unique keys and salts.
      *
      * Change these to different unique phrases! You can generate these using
      * the {@link https://api.wordpress.org/secret-key/1.1/salt/ WordPress.org secret-key service}.
      *
      * You can change these at any point in time to invalidate all existing cookies.
      * This will force all users to have to log in again.
      *
      * @since 2.6.0
      */
      define( 'AUTH_KEY',         'put your unique phrase here' );
      define( 'SECURE_AUTH_KEY',  'put your unique phrase here' );
      define( 'LOGGED_IN_KEY',    'put your unique phrase here' );
      define( 'NONCE_KEY',        'put your unique phrase here' );
      define( 'AUTH_SALT',        'put your unique phrase here' );
      define( 'SECURE_AUTH_SALT', 'put your unique phrase here' );
      define( 'LOGGED_IN_SALT',   'put your unique phrase here' );
      define( 'NONCE_SALT',       'put your unique phrase here' );
     /**#@-*/
     /**
      * WordPress database table prefix.
      *
      * You can have multiple installations in one database if you give each
      * a unique prefix. Only numbers, letters, and underscores please!
      */
      $table_prefix = 'wp_';
     /**
      * For developers: WordPress debugging mode.
      *
      * Change this to true to enable the display of notices during development.
      * It is strongly recommended that plugin and theme developers use WP_DEBUG
      * in their development environments.
      *
      * For information on other constants that can be used for debugging,
      * visit the documentation.
      *
      * @link https://wordpress.org/support/article/debugging-in-wordpress/
      */
      define( 'WP_DEBUG', false );
     /* Add any custom values between this line and the "stop editing" line. */
     
     /* That's all, stop editing! Happy publishing. */
     /** Absolute path to the WordPress directory. */
     if ( ! defined( 'ABSPATH' ) ) {
             define( 'ABSPATH', __DIR__ . '/' );
     }
     /** Sets up WordPress vars and included files. */
     require_once ABSPATH . 'wp-settings.php';
     ```
     ![4](https://user-images.githubusercontent.com/92453574/144302790-1a92a4a7-bf22-48de-b775-d58d7fe6304e.PNG)

     

   - masuk template wordpress.conf
     ```
     server {
          listen 80;
          listen [::]:80;
          # Log files for Debugging
          access_log /var/log/nginx/wordpress-access.log;
          error_log /var/log/nginx/wordpress-error.log;
         
          # Webroot Directory for Laravel project
          root /var/www/html/blog;
          index index.php index.html index.htm;
         
          # Your  Name
          server_name lxc_php7.dev;
         
          location / {
                  try_files $uri $uri/ /index.php?$query_string;
          }
         
          # PHP-FPM Configuration Nginx
          location ~ \.php$ {
                  try_files $uri =404;
                  fastcgi_split_path_info ^(.+\.php)(/.+)$;
                  fastcgi_pass unix:/run/php/php7.4-fpm.sock;
                  fastcgi_index index.php;
                  fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                  include fastcgi_params;
          }
     }
     ```
     ![5](https://user-images.githubusercontent.com/92453574/144302793-a10d3f6c-0ba2-4105-ba12-67d2086d0326.PNG)
     

   - jalankan ansible untuk menginstall
     ```
     ansible-playbook -i hosts installwordpress.yml -k
     ```
     ![6](https://user-images.githubusercontent.com/92453574/144302797-885ea735-5f9a-4efe-9fe3-3e4ed19d2478.PNG)

     

   - buka vm.local/blog/ untuk mengecek

     ![7](https://user-images.githubusercontent.com/92453574/144302800-50e62624-f122-40c3-b467-5ccdb56093f2.png)

     ![8](https://user-images.githubusercontent.com/92453574/144302805-0e638f38-7092-4150-8b18-2999506fcd61.png)

     ![9](https://user-images.githubusercontent.com/92453574/144302809-985b5359-bb0f-4af6-8f95-779170e4f242.png)

     ![10](https://user-images.githubusercontent.com/92453574/144302813-9d02b716-06e9-4925-bc6e-cada22795d76.png)

     berhasil


---
4. - 

---

**Soal Tambahan**

1. Laravel

   - masuk pada konfigurasi file lxc_landing

     ```
     cd ~/ansible/laravel/
     nano lxc_landing.dev
     ```

    ![1](https://user-images.githubusercontent.com/92453574/144454260-0268c6ab-f8f9-4176-89b1-57637a28e1b3.PNG)

   

   - akan tertampil seperti dibawah ini

     ![2 sebelum](https://user-images.githubusercontent.com/92453574/144454266-874e97d1-d13c-49f7-9367-8e61778d9f3a.PNG)

     

   - kemudian rubah menjadi seperti gambar di bawah

     ![2 sesudah](https://user-images.githubusercontent.com/92453574/144454268-ca06dc8c-579c-44aa-801a-e698c6d010df.PNG)

     

   - buat ansible dengan nama "tambahan.yml"

     ```
     nano tambahan.yml
     ```

     ![3](https://user-images.githubusercontent.com/92453574/144454272-f729e4a7-a77a-45ac-86ae-bd98ba089344.PNG)

     

   - jalankan ansible "tambahan.yml"

     ```
     ---
     - hosts: all
       become : yes
       tasks:
        - name: mengganti php sock
          lineinfile:
           path: /etc/php/7.4/fpm/pool.d/www.conf
           regexp: '^(.*)listen =(.*)$'
           line: 'listen = 127.0.0.1:9001'
           backrefs: yes
        - name: copy the nginx config file 
          copy:
           src: ~/ansible/laravel/lxc_landing.dev
           dest: /etc/nginx/sites-available/lxc_landing.dev
        - name: Symlink lxc_landing.dev
          command: ln -sfn /etc/nginx/sites-available/lxc_landing.dev /etc/nginx/sites-enabled/lxc_landing.dev
          args:
           warn: false
        - name: restart nginx
          service:
           name: nginx
           state: restarted
        - name: restart php7
          service:
           name: php7.4-fpm
           state: restarted
        - name: curl web
          command: curl -i http://lxc_landing.dev
          args:
           warn: false
     ```

     ![4](https://user-images.githubusercontent.com/92453574/144454275-8616d722-c706-4fad-bd27-15c44609cd8e.PNG)
     ![5](https://user-images.githubusercontent.com/92453574/144454276-051793f1-3ba1-4322-bdca-9d98dbc61b11.PNG)

     

   - buka vm.local untuk mengecek apakah berhasil.

     ![6](https://user-images.githubusercontent.com/92453574/144454280-1c81ba96-6679-49be-a7a7-12ed236446f3.PNG)

   - berhasil

     

   

2. Wordpress

   - masuk pada konfigurasi file wordpress.conf

     ```
     cd wordpress/
     nano wordpress.conf
     ```

     ![1](https://user-images.githubusercontent.com/92453574/144454321-5a9855df-ca3f-4fe2-b0b8-e8ae7f056832.PNG)

     

   - akan tertampil seperti dibawah ini

     ![2 sebelum](https://user-images.githubusercontent.com/92453574/144454324-5234b081-781a-47aa-9ce5-30c6cc711bbf.PNG)

     

   - kemudian ubah menjadi seperti di bawah ini

     ![2 sesudah](https://user-images.githubusercontent.com/92453574/144454326-574058ef-2528-4226-bdd3-651c3a95a1d8.PNG)

     

   - buat ansible dengan nama "tambahan.yml"

     ```
     nano tambahan.yml
     ```

     ![3](https://user-images.githubusercontent.com/92453574/144454329-829ab04f-284e-47f8-8005-72e9201bcaa9.PNG)

     

   - jalankan ansible "tambahan.yml"

     ```
     ---
     - hosts: all
       become : yes
       tasks:
        - name: mengganti php sock
          lineinfile:
           path: /etc/php/7.4/fpm/pool.d/www.conf
           regexp: '^(.*)listen =(.*)$'
           line: 'listen = 127.0.0.1:9001'
           backrefs: yes
        - name: copy the nginx config file 
          copy:
           src: ~/ansible/wordpress/wordpress.conf
           dest: /etc/nginx/sites-available/lxc_php7.dev
        - name: Symlink lxc_php7.dev
          command: ln -sfn /etc/nginx/sites-available/lxc_php7.dev /etc/nginx/sites-enabled/lxc_php7.dev
          args:
           warn: false
        - name: restart nginx
          service:
           name: nginx
           state: restarted
        - name: restart php7
          service:
           name: php7.4-fpm
           state: restarted
        - name: curl web
          command: curl -i http://lxc_php7.dev
          args:
           warn: false
     ```

     ![4](https://user-images.githubusercontent.com/92453574/144454333-f5781906-e330-4880-a0b8-b85cee85da88.PNG)

     ![5](https://user-images.githubusercontent.com/92453574/144454337-8c341508-c2a4-4052-afba-250a0a616143.PNG)

     

   - buka vm.local/blog untuk mengecek apakah berhasil

     ![6](https://user-images.githubusercontent.com/92453574/144454342-e37f8022-b5b6-48f4-a60f-08cbba37f956.PNG)

   - berhasil




















