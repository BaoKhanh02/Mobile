# C. Cấu hình docker compose:
1. Tạo thư mục: ~/myapp
2. Chuyển vào trong thư mục ~/myapp
3. Tạo thư mục: ./myweb
4. Tạo file ./myweb/index.html (với nội dung là thông tin cá nhân của em)
5. Tạo file **docker-compose.yml** để nó sẽ có các dịch vụ sau:
   > - Khai báo sử dụng nodered/node-red, cổng 1880, dữ liệu nằm tại thư mục ./nodered
   > - Khai báo sử dụng nginx, cổng 80, cấu hình trong file ./nginx/nginx.conf
   > - Mount thư mục ./myweb thành thư mục /myweb trong nginx
   > - Mount file **./nginx/nginx.conf** vào file **/etc/nginx/nginx.conf** trong nginx
6. Edit file **./nginx/nginx.conf** để: 
   > - Cấu hình web server cổng 80
   > - server_name là sub-domain (sub-domain tuỳ ý của em)
   > - location / trỏ tới root là thư mục /myweb
   > - location /api dùng proxy_pass trỏ tới 1 (hoặc nhiều) node http_in của nodered
7. Edit file **./nodered/settings.js** để nodered bắt buộc đăng nhập
   > Chạy docker-compose lần đầu để Node-RED tự sinh file cấu hình trong thư mục ./nodered, sau đó mới tiến hành sửa settings.js và restart lại container

# Bài Làm
## 1. Tạo thư mục: ~/myapp
```
mkdir -p ~/myapp
cd ~/myapp
mkdir myweb nginx nodred
```
<img width="1483" height="762" alt="image" src="https://github.com/user-attachments/assets/f4f708a2-7668-4088-b1e5-b1ac5b069a86" />

## 2. Tạo file web cá nhân
nano myweb/index.html
<img width="1483" height="762" alt="image" src="https://github.com/user-attachments/assets/4cb840dd-eea6-4605-a3b3-6840d0c6133d" />

### Nội dung ví dụ:
```
<h1>Xin chào, tôi là Bảo Khánh</h1>
<p>Đây là website cá nhân của tôi</p>
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2d6d6319-ca01-4a31-bcb9-20806db69bd6" />

## 3. Tạo file docker-compose.yml
nano docker-compose.yml
<img width="1483" height="762" alt="image" src="https://github.com/user-attachments/assets/e5e972cd-3412-4c33-8020-5fe7ef2228b1" />

### Nội dung:
```
version: "3.8"

services:
  nodered:
    image: nodered/node-red
    container_name: node-red
    ports:
      - "1880:1880"
    volumes:
      - ./nodered:/data
    restart: always

  nginx:
    image: nginx
    container_name: web-nginx
    ports:
      - "80:80"
    volumes:
      - ./myweb:/myweb
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf
    depends_on:
      - nodered
    restart: always
```
## 4. Tạo file nginx.conf
nano nginx/nginx.conf
<img width="1483" height="762" alt="image" src="https://github.com/user-attachments/assets/d14a2938-3672-4fbd-bfc1-83cde256500b" />

### Nội dung:
```
events {}

http {
    server {
        listen 80;
        server_name mysite.local;

        location / {
            root /myweb;
            index index.html;
        }

        location /api {
            proxy_pass http://nodered:1880;
        }
    }
}
```
## 5. Chạy lần đầu (để Node-RED tạo config)
docker compose up -d
<img width="1483" height="762" alt="image" src="https://github.com/user-attachments/assets/2947af66-e2b7-4b72-9c28-ee0c859e00cd" />

Sau đó sẽ thấy thư mục ./nodered có file cấu hình
<img width="1920" height="1029" alt="image" src="https://github.com/user-attachments/assets/3f55a9d0-7c30-4c3c-980f-136ef23f4ced" />
## 6. Cấu hình đăng nhập Node-RED
nano nodered/settings.js

### Tìm dòng:

//adminAuth: {

### Sửa thành:

adminAuth: {
    type: "credentials",
    users: [{
        username: "admin",
        password: "$2a$08$abcdefghijk1234567890", 
        permissions: "*"
    }]
},
## Tạo password hash

### Chạy:

docker run -it nodered/node-red-admin hash-pw

Nhập password → copy đoạn hash thay vào trên
<img width="1483" height="762" alt="image" src="https://github.com/user-attachments/assets/1b4a6b2e-f4ae-4eae-b457-b355312003a4" />

## 7. Restart lại
docker compose restart
## 8. Truy cập

Trên Windows:
Web cá nhân:
http://192.168.15.128
<img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/e8c0177c-233e-4191-abdc-ed8af1c91ab9" />

Node-RED:
http://192.168.15.128:1880
<img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/1a7e1540-718f-44cd-865a-f51b5dc9458a" />
