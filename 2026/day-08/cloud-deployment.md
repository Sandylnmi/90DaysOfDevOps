## Cloud Server Setup: Docker, Nginx & Web Deployment - Local Linux Machine

Step 1 : `Connet to Local Linux Machine using Gitbash.`

Step 2 : `Connect to instance using ssh.`

Command : ssh username@ipaddress

Step 3 : `Install Nginx`

Command : sudo apt-get update && sudo apt install nginx

Step 4 :  `Check logs of nginx service.`

Command : journalctl -u nginx

Step 5 : `Save logs to file`

Commad : scp sandy@192.168.29.92:/var/log/nginx/access.log .

<img width="1350" height="699" alt="image" src="https://github.com/user-attachments/assets/ad03c2f4-cfb3-4590-bd41-d6214818d99c" />
<img width="1348" height="106" alt="image" src="https://github.com/user-attachments/assets/e3f4cb41-8cf3-4a04-aa59-90965d2aadcc" />
<img width="848" height="239" alt="image" src="https://github.com/user-attachments/assets/94747f39-af27-4887-8bf1-b54bb60eff02" />
<img width="1351" height="597" alt="image" src="https://github.com/user-attachments/assets/7a009df2-707b-434e-86bb-44083ed9bebc" />


