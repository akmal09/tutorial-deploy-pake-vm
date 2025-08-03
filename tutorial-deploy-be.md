## config repo, nginx,systemd (di dalam VM)
1. Masuk ke CLI VM.
2. clone repo github sesuai folder yang diinginkan di VM. Lalu masuk ke directory projectnya.
3. 'git config --global --add safe.directory /{projectdirectory}'
4. 'sudo chown -R $USER:$USER'
- contohya 'sudo chown -R azureuser:azureuser' azureuser itu contoh nama user untuk akses vm. User ini bakal jadi agent dari si github copilot nya.
5. 'nano /etc/nginx/sites-available/default'
```syntax
server {
    listen 80;
    server_name 20.247.180.27;

    location / {
        proxy_pass http://127.0.0.1:{portWebServer};
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

6. Configure systemd for run service with background service. (for the example im using node js to run serviec bacgroundly)
- 'nano /etc/systemd/system/{namaServiceSystemD}'
```syntax
[Unit]
Description=Bebas
After=network.target

[Service]
ExecStart=/usr/bin/{binUntukJalaninProgrammingLang} /projectdirectory/fileRunner
WorkingDirectory=/projectdirectory
Restart=always
User=azureuser
Group=azureuser
Environment=PATH=/projectdirectory
Environment=NODE_ENV=development

[Install]
WantedBy=multi-user.target
```

- 'sudo visudo'
tambahin linecode kayak gini di bawah:
```
your_user ALL=(ALL) NOPASSWD: /bin/systemctl restart nodeapp.service

## contoh: "azureuser ALL=(ALL) NOPASSWD: /bin/systemctl restart payload-forwarder.service"
```

- 'systemctl daemon-reload'
- 'systemctl enable applicationname'
- 'systemctl restart applicationname'


7. access service via ip address si vm nya beserta path dari nginx nya .

## Set CI/CD
1. Bikin folder ".github\workflows" di root project dan bikin file yml. Contoh file yml kayak gini:
```
name: Deploy Express master akmal App to Azure VM main

on:
  push:
    branches:
      - {namaBranch} ## Sesuaikan mau branch apa yang di deploy. Bisa main,development, dsb.
jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout Repository
      uses: actions/checkout@v4

## Untuk secrets nya tentuin di settings github bagian developer settings. config setiap ## variabel yang dibutihin kayak di bawah ini AZURE_VM_IP, AZURE_SSH_KEY, AZURE_VM_USER.
    - name: Deploy to Azure VM via SSH in Staging
      uses: appleboy/ssh-action@v1.0.3
      with:
        host: ${{ secrets.AZURE_VM_IP }}
        username: ${{ secrets.AZURE_VM_USER }}
        key: ${{ secrets.AZURE_SSH_KEY }}
        script: |
          cd /{pathFolderRepodiComputerAtauVm}
          git pull {namaRemote} {namaBranch}
          npm install --{namaBranch}
          sudo systemctl restart {namaServiceSystemD} ## step ini ada hubungan nya di step nomor 2 di bagian 
```
2. push dan pastikan file yml nya udah ke push di repo github.


## Test CI/CD
1. Coba push hasil commit an dari repo kita di local computer ke repo github.
2. Pantau ci/cd nya di section "Actions" di github nya.
3. Kalo misal gagal, coba debugging gimana error nya.

## NOTES:
1. variable untuk "binUntukJalaninProgrammingLang" bisa node,mvn,gradlew,javac,dsb.
2. Manage Your Node.js Service
Command	Description:
sudo systemctl start applicationname            | Start the service manually
sudo systemctl stop applicationname             | Stop the service
sudo systemctl restart applicationname          | Restart the service
sudo systemctl status applicationname           | Check service status
sudo systemctl disable applicationname          | Disable auto-start on boot
sudo journalctl -u applicationname --no-pager   | View logs
3. variable untuk "portWebServer" contohnya 3000,5000,4001,dsb.
4. variable namaRemote biasanya origin.
5. variable namaServiceSystemD contohnya "payload-forwarder.service".