### D. (Bonus - không bắt buộc)
1. tạo thư mục ./myapi
2. tạo file ./myapi/app.py sử dụng Python + Flask để làm gì đó funny
3. tạo file ./myapi/requirements.txt chứa các thư viện mà app.py sử dụng (theo như app.py ví dụ thì requirements.txt chỉ cần có nội dung: **flask**)
4. tạo file ./myapi/Dockerfile để khai báo sử dụng Python 3.9 slim
   ```
	# Sử dụng phiên bản Python nhẹ (alpine) để giảm dung lượng image
	FROM python:3.9-slim

	# Thiết lập thư mục làm việc bên trong container
	WORKDIR /app

	# Sao chép file requirements vào và cài đặt thư viện
	COPY requirements.txt .
	RUN pip install --no-cache-dir -r requirements.txt

	# Sao chép toàn bộ mã nguồn vào container
	COPY . .

	# Thông báo container sẽ chạy ở cổng 9630
	EXPOSE 9630

	# Lệnh khởi chạy ứng dụng
	CMD ["python", "app.py"]
5. Sửa đổi docker-compose để sử dụng myapp (xem phần tham khảo ở dưới)
6. Sửa đổi nginx/nginx.conf để /api trỏ tới service myapp cổng 9630

# Bài làm
1. Tạo thư mục và file
<img width="1920" height="1029" alt="image" src="https://github.com/user-attachments/assets/8fdf84ec-ab4c-4066-b9ab-c8d68b2cf2e6" />

2. File app.py
```
from flask import Flask
import random

app = Flask(__name__)

jokes = [
    "Docker không phải máy ảo 😄",
    "Code chạy trên máy tôi là được 🤣",
    "Bug không phải lỗi, đó là tính năng 🐛",
    "Flask nhẹ hơn việc debug lúc 2 giờ sáng ☕",
    "Node-RED và Flask đang làm việc cùng nhau 🚀"
]

@app.route("/")
def home():
    return {
        "message": "Hello from Flask API!",
        "joke": random.choice(jokes)
    }

@app.route("/hello")
def hello():
    return {
        "name": "Bảo Khánh",
        "message": "Xin chào từ Flask Docker Container!"
    }

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=9630)
```
3. File requirements.txt
```
flask
```
4. File Dockerfile
```
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 9630

CMD ["python", "app.py"]
```
5. Sửa docker-compose.yml
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
      - myapp
    restart: always

  myapp:
    build: ./myapi
    container_name: my-flask-app
    restart: always
    expose:
      - "9630"
```
6. Sửa nginx/nginx.conf
```
events {}

http {
    server {
        listen 80;
        server_name khanh.id.vn;

        location / {
            root /myweb;
            index index.html;
        }

        location /api {
            proxy_pass http://myapp:9630;
        }
    }
}
```
7. Build và chạy
<img width="1920" height="1029" alt="image" src="https://github.com/user-attachments/assets/1075ef75-1bfe-4cbb-a39c-ceb87ad9366d" />
