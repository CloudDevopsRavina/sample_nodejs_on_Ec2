Nginx Configuration

File: /etc/nginx/conf.d/reverse-proxy.conf

server {
    listen 80;
    server_name app.example.com;

    location / {
        proxy_pass http://10.0.1.10:8080;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        proxy_connect_timeout 60;
        proxy_send_timeout 60;
        proxy_read_timeout 60;
    }
}
Example for Backend Running on Same EC2

If your application is running on port 8080 on the same server:

server {
    listen 80;
    server_name _;

    location / {
        proxy_pass http://localhost:8080;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
Validate Configuration
sudo nginx -t

Output should be:

nginx: configuration file /etc/nginx/nginx.conf test is successful
Restart Nginx
sudo systemctl restart nginx
sudo systemctl enable nginx

Check status:

sudo systemctl status nginx
Security Group

Allow:

Type	Port
HTTP	80
HTTPS	443 (Optional)
SSH	22
Verify
curl http://localhost

or

curl http://<EC2-Public-IP>
Architecture
User Browser
      |
      v
+------------------+
|  Nginx (Port 80) |
| Reverse Proxy    |
+------------------+
      |
      v
+------------------+
| Application      |
| Port 8080        |
+------------------+

This is the most commonly used reverse pro
