# Ansible Role: PHP56

An Ansible role that installs and configure PHP 5-5.6 on Debian/Ubuntu servers.

Current PHP5-5.6 version: **5.6.18**

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    php_ppa: "ppa:ondrej/php5-5.6"    
    php_packages:
      - php5-common
      - php5-cli
      - php5-intl
      - php5-curl
      - php5-cgi
      - php5.6-fpm
      - php5-mysql
      - php5-gd
      - php5-mcrypt
    php_upload_max_filesize: "20M"
    php_post_max_size: "20M"
    php_memory_limit: "1024M"

## Dependencies

None.

## Example Playbook

    - hosts: webservers
      roles:
        - { role: itcraftsmanpl.php5 }

## License

MIT