1. 'sudo nano /etc/postgresql/${db+version}/main/postgresql.conf'
=====================================
1. 'sudo cat /var/lib/postgresql/instance_4001/postgresql.conf'

2. Find this line:
```md
#listen_addresses = 'localhost'
```
Change it to:
```md
listen_addresses = '*'
```

3. 'sudo nano /etc/postgresql/${db+version}/main/pg_hba.conf'
=====================================
3. 'sudo nano /var/lib/postgresql/instance_4001/pg_hba.conf'
Add this line at the bottom to allow external IP access:
```md
host    all             all             0.0.0.0/0               md5
```

4. 'sudo systemctl restart postgresql' 
=====================================
4. 'sudo -u postgres /usr/lib/postgresql/16/bin/pg_ctl -D /var/lib/postgresql/instance_4001 restart'

5. Allowing Postgresql port in firewall
PostgreSQL runs on port 5432 by default. But the port number can be adjusted, so first is allowing port via cli:
'sudo ufw allow {dpPortYouWant}/tcp'

6. Or in Azure (if UFW is inactive):
Go to the Azure Portal.
a. Find your VM → Networking → Inbound Port Rules
b . Add a new rule:
Port: {dpPortYouWant}
Protocol: TCP
Source: Your IP or Any (if you’re just testing)
Action: Allow

7. test DB connection in DBeaver/DBMS connector