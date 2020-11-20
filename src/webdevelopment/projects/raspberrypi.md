## Json automation for surf

crontab -l

crontab -e

  

1.  Fetch Github parent repo
    
2.  Api Handler
    
3.  JSON Handler
    
4.  push dist repo to github
    

-   02:00
    
-   02:30
    
-   02:40
    
-   03:00
    

  

# m h dom mon dow command

0 2 * * * cd Surf-forecast-rennes && git pull -q origin master && cd

30 2 * * * cd Surf-forecast-rennes/py_scripts && python3 API_handler.py && cd

40 2 * * * cd Surf-forecast-rennes/py_scripts && python3 JSON_handler.py && cd

50 2 * * * cd Surf-forecast-rennes && git add . && cd

55 2 * * * cd Surf-forecast-rennes && git commit . -m “data_update” && cd

0 3 * * * cd Surf-forecast-rennes && git push -q origin master && cd

  
  
  

## access mariadb

  

/etc/mysql/my.cnf:

-   skip-networking=0
    
-   skip-bind-address
    

view privileges:

-   select user, host from mysql.user where host <> 'localhost'
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1MTk1ODU0NTddfQ==
-->