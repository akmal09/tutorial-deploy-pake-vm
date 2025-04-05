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
```syntax
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


Okay now please guide me to create backend repository using node js (using express.js) with CRUD feature. The database for this repo are using postgre who installed in my azure vm and also please guide me to install the database and connect it to backend repository.


Nama DB: mydb
username: myuser
password: lalabisa212123##

docker run --name mydb-pg -e POSTGRES_USER=myuser -e POSTGRES_PASSWORD=lalabisa212123## -e POSTGRES_DB=mydb -p 5432:5432 -d postgres:13.1-alpine

CREATE TABLE users (id SERIAL PRIMARY KEY, name VARCHAR(100), email VARCHAR(100) UNIQUE);


Please give again about the solution about the solution. I want the backend repository is pushed to github and then the repository is pulled in azure vm and configure it so the backend repository can be deployed.

Anyway, how to make sure in vm the backend repository is can connected to database in VM. In thi case i have backend express.js env like this:
PORT=8572
DB_USER=myuser
DB_PASSWORD='pwdb'
DB_NAME=mydb
DB_HOST=localhost
DB_PORT=5432
I want to make sure that my backend express.js is can be connected to postgresql which has been installed in my vm.

CREATE USER myuser WITH ENCRYPTED PASSWORD 'lalabisa212123##';
GRANT ALL PRIVILEGES ON DATABASE mydb TO myuser;

i run the query inside postgre db like this:
"CREATE TABLE public.users (id SERIAL PRIMARY KEY, name VARCHAR(100), email VA
RCHAR(100) UNIQUE);"
But got error like this "ERROR:  permission denied for schema public
LINE 1: CREATE TABLE public.users (id SERIAL PRIMARY KEY, name VARCH..."

CREATE TABLE public.users (   id SERIAL PRIMARY KEY,   name VARCHAR(100),   email VARCHAR(100) UNIQUE);


i mean i access the postgre db with this command first in my vm "psql -U myuser -d mydb -h localhost -W" and the i try this command "CREATE TABLE public.users (id SERIAL PRIMARY KEY, name VARCHAR(100), email VARCHAR(100) UNIQUE);" but i got error like this "ERROR:  permission denied for schema public
LINE 1: CREATE TABLE public.users (id SERIAL PRIMARY KEY, name VARCH...". Do you know how treat this so i can create table in mydb?

Now, can you guide to test my api, i have endpoint with this list:
1. method GET, endpoint "/"
2. method POST, endpoint "/users"
3. method GET, endpoint "/users"
4. method GET, endpoint "/users/:id"
5. method UPDATE, endpoint "/users/:id"
6. method DELETE, endpoint "/users/:id"

to test that please use curl so i can test locally in my vm.


Okay now i want my backend repository who have been connected with postgresql inside vm are deployed and public or external computer can access my backend repository. Please guide me to deploy it using nginx for the gateway and the systemd is the worker runner. My backend repository are run by express.js and run in port 8572, the database who connected with my backend repository is in port 5432

anyway in my vm there another service which based on my latest default nginx script:
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

So now how can i keep the existing service are keep run while my backend repository service can be run

nano /etc/systemd/system/experiment-db.service

```syntax
[Unit]
Description=Experiment DB for VM experiment
After=network.target

[Service]
ExecStart=/usr/bin/node /project-experiment-akmal/beginner-cloud/express-js-crud-master
WorkingDirectory=/project-experiment-akmal/beginner-cloud/express-js-crud-master
Restart=always
User=azureuser
Group=azureuser
Environment=PATH=/project-experiment-akmal/beginner-cloud/express-js-crud-master
Environment=NODE_ENV=development

[Install]
WantedBy=multi-user.target
```


wait anyway when i use node js(express.js), spring boot, laravel. I was run a command like node starter.js or mvn run:repo so the framework are run an program which give log and cli terminal who can access the system in localhost:3000. But its different to run php backend which i dont type a command to run the php backend but only just access the index.php. So what is the difference mean of running backend framework like node js(express.js), spring boot java, laravel php and running backend framework from programming language native like php, go lang?

So when i run program in terminal for every framework to type like "php artisan serve" in laravel, "mvn run :reponame" in spring boot, "node server.js" in node js (express.js), python3 app.py in flask python and all of that are outputing for accessing the system in "localhost:port" which mean the framework are run by a web server?

okay but how about if framework who haev their own web server / embedded web server like spring boot java or express js (node js) are run by external web server like nginx or apache?

Because in my current experience when i run node js backend framework i must run command "node server.js" so the backend framework are can be accessed from port 3000. After that i configure the nginx to run reverse proxy from incoming request from listening port 80 and the forward it to port 3000 to So the request is successfully accessed my backend service (node.js). But does the step is difference when i only want to use external web server so i dont need to type command "node server.js" and do reverse proy in nginx?

wait so although i use systemd for run my backend repository backgroundly, the web server who run my backend service is still from node js web server? Because i want to ask is it possible to run an external web server so the node js does need to run their own web server? just like php who use nginx for run ther web server

if the node js must be use their own web server then does laravel also can be work like node js? I mean just using systemd for the worker to run php artisan serve and then do reverse proxy from nginx to php artisan web server. But in most case why laravel is same treated like php native? which is use external web werver?


ssh

Lets talk about deployment of the database. So i want the database can be accessed from ip address with username and password from the azure VM that i have. Do you know how to do that?


okay, the database has been connected and deployed. But does the tutorial that you give is safe and best practice? If no, do you have suggestion to give tutorial for deploying the database to be more safe for accessing?