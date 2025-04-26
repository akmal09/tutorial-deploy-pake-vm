## Using nginx
1. 'nano /etc/nginx/sites-available/default'
```syntax
server {
    listen 80;
    server_name 20.247.180.27;

    location / {
        proxy_pass http://127.0.0.1:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```
- restart the nginx:
```
sudo systemctl restart nginx
```
- see nginx status:
```
sudo nginx -t
```


2. Configure systemd for run node js with background service
'nano /etc/systemd/system/servicename.service'
```syntax
[Unit]
Description=Bebas
After=network.target

[Service]
ExecStart=/usr/bin/node /projectdirectory/fileRunner
WorkingDirectory=/projectdirectory
Restart=always
User=azureuser
Group=azureuser
Environment=PATH=/projectdirectory
Environment=NODE_ENV=development

[Install]
WantedBy=multi-user.target
```
=======NOTE:=======
Jangan lupa ketik command 'sudo visudo'
===================
'systemctl daemon-reload'
'systemctl enable applicationname'
'systemctl restart applicationname'


3. access service via ip address of ssh.

NOTES:
3️⃣ Manage Your Node.js Service
Command	Description:
sudo systemctl start applicationname            | Start the service manually
sudo systemctl stop applicationname             | Stop the service
sudo systemctl restart applicationname          | Restart the service
sudo systemctl status applicationname           | Check service status
sudo systemctl disable applicationname          | Disable auto-start on boot
sudo journalctl -u applicationname --no-pager   | View logs