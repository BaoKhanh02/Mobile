### E. Triển khai (level test) ứng dụng
1. Chuyển vào trong thư mục ~/myapp
2. Gõ lệnh để docker compose chạy: sẽ run tất cả các service khai báo trong file docker-compose.yml
  > Lợi ích: Chỉ cần docker-compose up -d là toàn bộ hệ thống (Web + Node-RED + Tunnel) tự chạy,
3. Kiểm tra các container đang chạy trong docker, nếu có cái nào bị restart cần tìm lỗi rồi edit lại docker-compose.yml
4. Kiểm tra kiểm thử các service đang chạy độc lập thông qua ip và port của nó: ví dụ mở trình duyệt ip_ubuntu:1880 để check nodered đã chạy chưa
5. Sử dụng nodered: kéo nodered http_in , http_response, function : để tạo api get đơn giản (dùng cho /api proxy_pass của nginx)
6. Sửa file ./myweb/index.html : thêm code html+js để sử dụng được api đã khai báo proxy_pass (thực ra là sử dụng nodered http_in hoặc sử dụng service myapi)
> 
### F. Gỡ lỗi:
1. nếu có lỗi xẩy ra trong quá trình triển khai docker compose up -d
   > Kiểm tra nhanh: **docker compose ps** giúp biết container nào đang chạy
   > xem log, ví dụ:
   >  docker logs mynginx
   >  docker logs myapi
2. Thêm healthcheck cho myapi trong file docker-compose.yml
   >     healthcheck:
   >       test: ["CMD", "curl", "-f", "http://localhost:9630"]
3. giới hạn resource cho một service: (tránh việc 1 service chiếm quá nhiều ram)
   >     deploy:
   >       resources:
   >         limits:
   >           memory: 512M
   sử dụng lệnh: **docker compose stats** để quan sát lượng ram sử dụng bởi mỗi service

# Bài Làm

1. Chuyển vào trong thư mục ~/myapp và Gõ lệnh để docker compose chạy
<img width="1920" height="1029" alt="Screenshot 2026-06-09 105005" src="https://github.com/user-attachments/assets/4a2ff621-dab4-4c10-8731-71db9b140e04" />

2. Kiểm tra các container

3. Kiểm tra các service
<img width="1920" height="1029" alt="image" src="https://github.com/user-attachments/assets/6c0b5992-30cd-43cb-88be-93fecd5f5a16" />

<img width="1920" height="1029" alt="image" src="https://github.com/user-attachments/assets/1769034f-5d67-4bf0-8d73-7a1209b63381" />

<img width="1920" height="1029" alt="image" src="https://github.com/user-attachments/assets/743cf122-413a-4ba6-94e5-baf849711f96" />

4. Sử dụng nodered
Sau khi Deploy
<img width="1920" height="1029" alt="image" src="https://github.com/user-attachments/assets/983031dd-7b92-4738-bedd-c32a58dfed0d" />

5. Cho Nginx proxy tới Node-RED
<img width="1920" height="1029" alt="Screenshot 2026-06-09 110339" src="https://github.com/user-attachments/assets/80704b09-eb82-48ce-8ffd-f333e855b72e" />

6. Sửa index.html gọi API
<img width="1920" height="1029" alt="image" src="https://github.com/user-attachments/assets/0078b2b0-9f6d-4f1e-97ab-2c5bd8c10856" />
