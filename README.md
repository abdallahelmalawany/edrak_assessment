## Task 1: Create Project Structure & README
<img width="821" height="333" alt="Screenshot 2025-10-01 015814" src="https://github.com/user-attachments/assets/70685592-67fc-4832-8974-f1b6b84b6e87" />

```bash
ubuntu@ip-172-31-16-147:~/edrak_assessment$mkdir -p project/src && mkdir project/docs && mkdir project/tests
ubuntu@ip-172-31-16-147:~/edrak_assessment$touch project/src/main.py && touch project/docs/README.md && touch project/tests/test_main.py   # Create directories
ubuntu@ip-172-31-16-147:~/edrak_assessment$ echo "This is a sample DevOps project for testing automation." >> project/docs/README.md        # create files
ubuntu@ip-172-31-16-147:~/edrak_assessment$ ls
project
ubuntu@ip-172-31-16-147:~/edrak_assessment$ tree
.
└── project
    ├── docs
    │   └── README.md
    ├── src
    │   └── main.py
    └── tests
        └── test_main.py

5 directories, 3 files
ubuntu@ip-172-31-16-147:~/edrak_assessment$ cat project/docs/README.md
This is a sample DevOps project for testing automation.
```

## Task2 :Configure the hostname of the Linux machine
```bash
ubuntu@ip-172-31-16-147:~/edrak_assessment$ sudo hostnamectl set-hostname devops-junior
ubuntu@ip-172-31-16-147:~/edrak_assessment$ hostname
devops-junior
```
<img width="822" height="107" alt="Screenshot 2025-10-01 015954" src="https://github.com/user-attachments/assets/cbb929d1-fbef-4c6f-81b9-053ef88a7463" />

## Task3:Run a basic web server container
```bash
#create web container using ngnix image and expose on port 8080
ubuntu@devops-junior:~$ sudo docker run -it -d -p 8080:80 --name edrak nginx
ed3488fca5b604bba7dae6e1430988911dbddc0b97854268ffac566183888297
# display the container
ubuntu@devops-junior:~$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                     NAMES
ed3488fca5b6   nginx     "/docker-entrypoint.…"   8 seconds ago   Up 8 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   edrak
# test the container
ubuntu@devops-junior:~$ curl localhost:8080
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```
<img width="1919" height="966" alt="Screenshot 2025-10-01 021001" src="https://github.com/user-attachments/assets/a818ec34-8cdb-4053-9fe4-d553397bf238" />
<img width="1222" height="725" alt="Screenshot 2025-10-01 030355" src="https://github.com/user-attachments/assets/c548f94b-9dfa-418e-b1ec-29b1322ed048" />

## Task4:Automate a simple task using a bash script

```bash
using vim editor to create and edit in file to write script
ubuntu@devops-junior:~$ vim backp.sh
#!/bin/bash
if [ "$#" -ne 2 ]; then
    echo "Usage: $0 Need to Two arguments <source_directory> <destination_directory>"
    exit 1
fi

SRC_DIR=$1
DEST_DIR=$2
DATE=$(date +"%Y-%m-%d_%H-%M-%S")
BACKUP_NAME="backup_${DATE}.tar.gz"

if [ ! -d "$SRC_DIR" ]; then
    echo "Error: Source directory '$SRC_DIR' does not exist."
    exit 2
fi

mkdir -p "$DEST_DIR"

tar -czf "$DEST_DIR/$BACKUP_NAME" -C "$(dirname "$SRC_DIR")" "$(basename "$SRC_DIR")"

if [ $? -eq 0 ]; then
    echo "Backup successful!"
    echo "Backup file: $DEST_DIR/$BACKUP_NAME"
else
    echo "Backup failed!"
    exit 3
fi
ubuntu@devops-junior:~$ chmod +x backp.sh    #make the executable file 
ubuntu@devops-junior:~$ ./backp.sh edrak_assessment/project/ edrak_assessment/   #run the script
Backup successful!
Backup file: edrak_assessment//backup_2025-09-30_23-23-57.tar.gz

ubuntu@devops-junior:~/edrak_assessment$ ls
backup_2025-09-30_23-23-57.tar.gz  project
```
<img width="1077" height="724" alt="Screenshot 2025-10-01 030646" src="https://github.com/user-attachments/assets/37018738-4dae-40e9-9117-bc4c17c21c30" />

## Task5:Set up a containerized database with security.
```bash
# create database container and expose on port 5432 and set password and bind to localhost
ubuntu@devops-junior:~/edrak_assessment$ docker run -d --name postgres_junior -e POSTGRES_PASSWORD=devops_pass -p 127.0.0.1:5432:5432 postgres
execute psql command on database container and go inside the container
ubuntu@devops-junior:~/edrak_assessment$ docker exec -it postgres_junior psql -U postgres
psql (18.0 (Debian 18.0-1.pgdg13+3))
Type "help" for help.
# create database with junior_db name
postgres=# CREATE DATABASE junior_db;
CREATE DATABASE
postgres=# exit
 #test connectvity from host to database to ensure that is working
ubuntu@devops-junior:~/edrak_assessment$ psql -h 127.0.0.1 -U postgres -d junior_db
Password for user postgres:
psql (16.10 (Ubuntu 16.10-0ubuntu0.24.04.1), server 18.0 (Debian 18.0-1.pgdg13+3))
WARNING: psql major version 16, server major version 18.
         Some psql features might not work.
Type "help" for help.

junior_db=# exit
```
<img width="854" height="210" alt="Screenshot 2025-10-01 023806" src="https://github.com/user-attachments/assets/a627b8fb-9be8-42f8-9449-a3427a628dfe" />
<img width="909" height="215" alt="Screenshot 2025-10-01 023752" src="https://github.com/user-attachments/assets/67ab7af1-230e-4b80-9ea8-0186dcdba01b" />
<img width="934" height="560" alt="Screenshot 2025-10-01 023725" src="https://github.com/user-attachments/assets/338b51cd-a8be-4983-a2d5-cd3c19731d0a" />

