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

wget https://github.com/prometheus/mysqld_exporter/releases/download/v0.15.1/mysqld_exporter-0.15.1.linux-amd64.tar.gz
    5  tar -xvf mysqld_exporter-0.15.1.linux-amd64.tar.gz
    6  sudo mv mysqld_exporter-0.15.1.linux-amd64/mysqld_exporter /usr/local/bin/
    7  sudo chmod +x /usr/local/bin/mysqld_exporter
    8  mysqld_exporter --version
    9  sudo vi /etc/.mysqld_exporter.cnf
   10  sudo chmod 600 /etc/.mysqld_exporter.cnf
   11  sudo nano /etc/systemd/system/mysqld_exporter.service
   12  sudo systemctl daemon-reload
   13  sudo systemctl start mysqld_exporter
   14  sudo systemctl enable mysqld_exporter
   15  sudo systemctl status mysqld_exporter --no-pager

Running above steps gave error in starting sql exporter, hence I followed below steps:
   
   16  sudo mysql                 # and check if password is right in  /etc/.mysqld_exporter.cnf
   17  vi /etc/.mysqld_exporter.cnf         
   18  sudo vi /etc/.mysqld_exporter.cnf          # save and close
   
#The .mysqld_exporter.cnf must be readable by the Prometheus user. If systemd runs exporter as prometheus, it cannot read the file as root-only or another user → fails. Hence running below 2 lines

   20  sudo chown prometheus:prometheus /etc/.mysqld_exporter.cnf
   21  sudo chmod 600 /etc/.mysqld_exporter.cnf
   
#Checking if it is accessible and now restarting it
   
   22  sudo -u prometheus /usr/local/bin/mysqld_exporter --config.my-cnf=/etc/.mysqld_exporter.cnf
   23  sudo systemctl daemon-reload
   24  sudo systemctl restart mysqld_exporter
   25  sudo systemctl status mysqld_exporter --no-pager
