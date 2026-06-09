### G. Triển khai ứng dụng đến End-user
1. Trong Cloudflare: Tạo tunnel (đường hầm), chọn loại triển khai cho docker
2. Convert lệnh docker run ... sang dạng docker compose
3. Khai báo kết quả convert vào trong file docker-compose.yml
4. Chạy lại docker compose
5. Public ứng dụng bằng cách thêm 1 router trỏ tới container đang chạy trong docker, dữ liệu sẽ đi qua tunnel, url dạng sub-domain
6. Kiểm tra url sub-domain đã hoạt động public cho mọi end-user

# Bài Làm

1. Trong Cloudflare: Tạo tunnel (đường hầm), chọn loại triển khai cho docker
<img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/7705a903-664d-4bdc-b9bf-0b44cb33044b" />

2. Convert lệnh docker run ... sang dạng docker compose
```
  cloudflared:
    image: cloudflare/cloudflared:latest
    container_name: cloudflared
    command: tunnel --no-autoupdate run --token eyJhIjoiYWYxOTJlZTk5YzA0NWViMmViOTQ1ZTU3ZDc3M2I5ODQiLCJ0IjoiZTE5ZWY0ZjEtMzJhZS00MWE0LWJkNTQtNWUwY2FjZTg0YThkIiwicyI6Ik1EYzVNamc0TURNdE9XVm1aQzAwTmpsakxUaGpZell0TURRMU0yTXpaREU0TnpGaCJ9
    restart: unless-stopped
```
3. Khai báo kết quả convert vào trong file docker-compose.yml
<img width="1920" height="1029" alt="image" src="https://github.com/user-attachments/assets/acc59088-14b0-4c49-8067-8d445c58fb75" />

4. Chạy lại docker compose
<img width="1920" height="1029" alt="image" src="https://github.com/user-attachments/assets/c4a1e9d7-b0a8-4569-a5a5-d57c51bd1027" />

5. Public ứng dụng bằng cách thêm 1 router trỏ tới container đang chạy trong docker, dữ liệu sẽ đi qua tunnel, url dạng sub-domain
<img width="1920" height="1029" alt="Screenshot 2026-06-09 112440" src="https://github.com/user-attachments/assets/e070fc69-0c5f-4db1-ab47-e9bada3ac058" />

6. Kiểm tra url sub-domain đã hoạt động public cho mọi end-user
<img width="1920" height="1029" alt="image" src="https://github.com/user-attachments/assets/b4b75646-30f7-4a9e-9d47-00c65c01fec3" />

## Trả lời câu hỏi
## F. Câu hỏi tìm hiểu

### 1. Tại sao phải dùng Nginx làm Reverse Proxy mà không trỏ thẳng Tunnel vào Node-RED?

Nginx đóng vai trò Reverse Proxy giúp điều hướng request tới nhiều dịch vụ khác nhau thông qua cùng một tên miền.

Ví dụ:

* `https://demo.baokhanh2004.id.vn/` → Website cá nhân
* `https://demo.baokhanh2004.id.vn/api/` → Flask API
* `https://demo.baokhanh2004.id.vn/red/` → Node-RED

Nếu Cloudflare Tunnel trỏ trực tiếp vào Node-RED thì chỉ có thể truy cập Node-RED mà không thể sử dụng chung domain cho các dịch vụ khác.

Lợi ích của Nginx Reverse Proxy:

* Sử dụng một domain cho nhiều service.
* Che giấu các cổng nội bộ.
* Tăng tính bảo mật.
* Dễ cấu hình SSL, Cache, Rate Limit.
* Dễ dàng thay đổi backend mà không ảnh hưởng người dùng.

---

### 2. Sự khác biệt giữa Mount file và Mount thư mục trong Docker là gì?

#### Mount file

Ví dụ:

```yaml
volumes:
  - ./nginx/nginx.conf:/etc/nginx/nginx.conf
```

Chỉ một file được chia sẻ giữa Host và Container.

Thường dùng cho:

* File cấu hình.
* Chứng chỉ SSL.
* File `.env`.

#### Mount thư mục

Ví dụ:

```yaml
volumes:
  - ./myweb:/myweb
```

Toàn bộ thư mục được chia sẻ giữa Host và Container.

Thường dùng cho:

* Website tĩnh.
* Source code.
* Dữ liệu Node-RED.

---

### 3. Nếu thay đổi file index.html ở máy Ubuntu, nội dung trên web có thay đổi ngay không? Tại sao?

Có.

Do thư mục `./myweb` được mount trực tiếp vào container Nginx:

```yaml
volumes:
  - ./myweb:/myweb
```

Khi chỉnh sửa file:

```bash
nano ~/myapp/myweb/index.html
```

và lưu lại, Nginx sẽ đọc nội dung mới ngay lập tức.

Chỉ cần nhấn F5 trên trình duyệt để xem kết quả mà không cần restart container.

---

### 4. docker-compose.yml khai báo restart: always hoặc restart: unless-stopped để làm gì?

#### restart: always

```yaml
restart: always
```

Container sẽ tự động khởi động lại khi:

* Docker restart.
* Ubuntu reboot.
* Container bị lỗi hoặc bị dừng bất thường.

#### restart: unless-stopped

```yaml
restart: unless-stopped
```

Hoạt động tương tự `always`, tuy nhiên nếu người quản trị chủ động dừng container bằng:

```bash
docker stop <container>
```

thì Docker sẽ không tự khởi động lại.

Thông thường nên sử dụng:

```yaml
restart: unless-stopped
```

---

### 5. Cách khai báo để tất cả các services đều dùng chung một network? Lợi ích là gì?

Khai báo network:

```yaml
networks:
  my-network:
    driver: bridge
```

Gắn vào các service:

```yaml
services:
  nginx:
    networks:
      - my-network

  nodered:
    networks:
      - my-network

  myapp:
    networks:
      - my-network

  cloudflared:
    networks:
      - my-network
```

Lợi ích:

* Các container có thể gọi nhau bằng tên service.
* Không cần sử dụng địa chỉ IP.
* Dễ quản lý và mở rộng hệ thống.
* Tăng khả năng cô lập và bảo mật.

Ví dụ:

```nginx
proxy_pass http://myapp:9630;
```

Docker sẽ tự tìm container có tên `myapp`.

---

### 6. Đưa Cloudflare Token vào file .env và thêm vào .gitignore. Tại sao điều này quan trọng?

Tạo file `.env`:

```env
TUNNEL_TOKEN=xxxxxxxxxxxxxxxxxxxxxxxx
```

Sử dụng trong docker-compose:

```yaml
environment:
  - TUNNEL_TOKEN=${TUNNEL_TOKEN}
```

Thêm vào `.gitignore`:

```gitignore
.env
```

Lý do:

Cloudflare Tunnel Token giống như mật khẩu truy cập hệ thống.

Nếu đẩy Token lên GitHub công khai, người khác có thể:

* Chiếm quyền điều khiển Tunnel.
* Truy cập hệ thống nội bộ.
* Thay đổi cấu hình dịch vụ.

Đây là nguyên tắc bảo mật quan trọng khi phát triển phần mềm.

---

### 7. Tại sao nên thêm hậu tố :ro khi mount file cấu hình Nginx?

Ví dụ:

```yaml
volumes:
  - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
```

Trong đó:

```text
ro = Read Only
```

Container chỉ được phép đọc file mà không được ghi đè hoặc chỉnh sửa.

Lợi ích:

* Tránh thay đổi cấu hình ngoài ý muốn.
* Tăng tính bảo mật.
* Bảo vệ file cấu hình trên Host.

---

### 8. Khi dùng Cloudflare Tunnel có cần mở cổng cho các service nữa không?

Không bắt buộc.

Cloudflare Tunnel hoạt động bằng cách tạo kết nối từ máy chủ Ubuntu ra Cloudflare.

Luồng dữ liệu:

```text
Internet
    ↓
Cloudflare
    ↓
Tunnel
    ↓
Ubuntu
    ↓
Nginx
```

Do đó không cần:

* Public IP.
* NAT Port Forwarding.
* Mở cổng dịch vụ ra Internet.

Tuy nhiên trong quá trình học tập và kiểm thử vẫn nên mở:

```bash
sudo ufw allow 80
sudo ufw allow 1880
sudo ufw allow 9630
```

để có thể kiểm tra trực tiếp bằng địa chỉ IP của Ubuntu và đáp ứng yêu cầu của bài thực hành.
