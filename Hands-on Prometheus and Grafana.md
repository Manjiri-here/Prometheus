# After running the script check if all the applications are downloaded:

Nginx- 8080
Mysql server- http://SERVER_IP:3306.  - But mysql is not accessible on http/https/browser, you can access it by- sudo mysql
Prometheus- http://SERVER_IP:9090
Node exporter- http://localhost:9100/metrics
Nginx prometheus exporter- 9113
Grafana - http://SERVER_IP:3000.    
Alert manager- http://SERVER_IP:9093

Now you need to set user name and password for sql:

Note: 1) Keep password of your own. 
2) as exporter and mysql are both installed on same server we can keep exporter and localhost as it is. But in case where exporter is hosted on anotherserver then we need to provide its IP address instead of localhost

exporter → MySQL username

localhost → host MySQL expects the connection from

| Scenario                                                                  | What to put instead of `localhost`                                                      |
| ------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| Prometheus/MySQL Exporter is on **another machine**                       | 'exporter'@'<exporter-server-ip>' (replace '<exporter-server-ip>` with the actual IP) |
| Prometheus/MySQL Exporter can connect from **any host** (not recommended) | 'exporter'@'%'` → allows any IP to connect                                             |
| You have multiple MySQL servers or special networking                     | 'exporter'@'<hostname-or-subnet>'`                                                     |


% sudo mysql
 Once you are inside sql run below command:
CREATE USER 'exporter'@'localhost' IDENTIFIED BY 'StrongPass123!';
GRANT PROCESS, REPLICATION CLIENT, SELECT ON *.* TO 'exporter'@'localhost';
FLUSH PRIVILEGES;
EXIT;


