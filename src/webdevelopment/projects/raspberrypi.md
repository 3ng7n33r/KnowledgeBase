## Json automation for surf

### Automate update of Surf json file with cron
crontab -l
crontab -e


1.  Fetch Github parent repo -   02:00
    
2.  Api Handler -   02:30
    
3.  JSON Handler -   02:40
    
4.  push dist repo to github -   03:00

![enter image description here](https://raw.githubusercontent.com/3ng7n33r/KnowledgeBase/master/src/static/20201120_rasppi_cron.PNG)

  

## access mariadb
Settings to make raspberry pi MariaDB listen to connections
  

/etc/mysql/my.cnf:

-   skip-networking=0
    
-   skip-bind-address
    

view privileges:

-   select user, host from mysql.user where host <> 'localhost'
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzU4MDgzOTAsMTA5NjA2NDc5NV19
-->