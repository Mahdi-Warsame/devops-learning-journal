# Docker Learning

Docker Mini Project Log - Flask + MySQL

Overview:
Built a Flask app using Docker and connected it to a MySQL container using Docker networking.

Goal:
- Containerize Flask app
- Run MySQL in separate container
- Connect both using Docker network
- Learn debugging Docker issues

Final Working Setup:

1. Create network:
docker network create my-custom-network

2. Run MySQL container:
docker run -d --name mydb --network my-custom-network -e MYSQL_ROOT_PASSWORD=1234 -e MYSQL_DATABASE=mydb mysql:8

3. Run Flask app container:
docker run -d --name myapp --network my-custom-network -p 5003:5003 hello-flask-mysql

Issues Faced:

1. Missing build context:
docker build -t hello-flask-mysql
Fix: add "."

2. mysqlclient build error (pkg-config missing):
Fix: install system dependencies or use pymysql

3. Missing MySQLdb module:
Fix: use pymysql or fix mysqlclient install

4. Container not running:
Check: docker ps -a and docker logs

5. MySQL host error:
Unknown server host 'mydb'
Fix: correct container name and same network

6. MySQL container stuck in Created:
docker start mydb or recreate properly

7. Network typo:
my-cutom-network -> my-custom-network

8. Container name conflict:
docker rm -f myapp

Key Learnings:
- Containers communicate via names in same network
- localhost does not work between containers
- Created != Running
- Most issues are networking or config related

Result:
Successfully connected Flask container with MySQL container using Docker networking.

