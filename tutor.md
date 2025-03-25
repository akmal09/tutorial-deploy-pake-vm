Verify installation:

sh
Copy
Edit
node -v
npm -v

If you see version numbers, it's installed correctly.



mkdir node-akmal && cd node-akmal

## Using nginx
nano /etc/nginx/sites-available/default
isi default
```
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
- Configure systemd for run node js with background service
nano /etc/systemd/system/nodeapp.service
```service
[Unit]
Description=Node.js Hello World App
After=network.target

[Service]
ExecStart=/usr/bin/node /node-akmal/server.js
WorkingDirectory=/node-akmal
Restart=always
User=azureuser
Group=azureuser
Environment=PATH=/usr/bin
Environment=NODE_ENV=development

[Install]
WantedBy=multi-user.target
```
systemctl daemon-reload
systemctl enable nodeapp
systemctl restart nodeapp

3️⃣ Manage Your Node.js Service
Command	Description:
sudo systemctl start nodeapp            | Start the service manually
sudo systemctl stop nodeapp             | Stop the service
sudo systemctl restart nodeapp          | Restart the service
sudo systemctl status nodeapp           | Check service status
sudo systemctl disable nodeapp          | Disable auto-start on boot
sudo journalctl -u nodeapp --no-pager   | View logs