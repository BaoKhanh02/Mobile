# Cài đặt Ubuntu + Docker
# YÊU CẦU: 
### 1. Cài đặt hệ điều hành Ubuntu 24.04.4 LTS
- Sử dụng một trong các công cụ để giả lập: HyperV (có sẵn của windows), VirutualBox (Miễn phí), VM_Ware (Bản quyền).
- Download file iso để cài đặt.
- Cấu hình mạng trong Ubuntu (và công cụ để giả lập) để cho phép truy cập SSH vào Ubuntu từ cmd của windows.
### 2. Tìm hiểu các lệnh cơ bản của Ubuntu
Các lệnh cần tìm hiểu: 
- Liệt kê các file trong thư mục: ls
- Tạo thư mục: mkdir nameFolder
- Chuyển thư mục làm việc: cd path
- Copy file: cp file_nguồn path/file_đích
- Thay đổi quyền thao tác file: sudo chmod xxx filename
- Edit file: sudo nano tenfile: CTRL+o - lưu nội dung file sau khi edit; CTRL+x - thoát edit file
- Xem ip của máy Ubuntu: ip -4addr
### 3. Cài đặt docker cho Ubuntu
### 4. Kiểm tra phiên bản docker vừa cài đặt, kiểm tra phiên bản docker compose
### 5. Cấu hình để docker chạy được mà không cần tiền tố sudo
### 6. Tìm hiểu tập lệnh của docker và docker compose
### 7. Đảm bảo tường lửa trên Ubuntu đã cho phép cổng 80, 1880, 9630 (Lệnh: sudo ufw allow ...)
-------------------
# BÀI LÀM
## I. Cài đặt hệ điều hành Unbuntu 24.04.4 LTS
### 1. Truy cập vào: (https://ubuntu.com/download/desktop)
Chọn Download để tải file iso Ubuntu 24.04.4 LTS về máy 
<img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/f52d29dc-6c09-4c98-9c34-ccc1779b26b9" />

### 2. Sử dụng VMWare để giả lập
#### Tạo máy ảo

### 3. Cấu hình mạng trong Ubuntu cho phép SSH truy cập được từ CMD của windows

#### Gõ lệnh sau trên cửa sổ ubuntu trên máy ảo VM_Ware để lấy ip máy
```
ip a
```

```
IP là 172.20.10.2
```
#### Sau khi lấy được IP, mở cmd hoặc PowerShell trên windows và gõ lệnh dưới đây để truy cập SSH vào Ubuntu
```
ssh thuha@172.20.10.2
```
#### Tiếp theo nhập pasword, nếu đăng nhập thành công thì sẽ chuyển cửa sổ vào terminal của Ubuntu

---------------------
## II. Tìm hiểu các lệnh cơ bản của Ubuntu


-----------------------
## III. cài đặt docker và docker compose cho Ubuntu
### Sử dụng lệnh: sudo apt update để cập nhật danh sách phần mềm
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/839e41ba-c454-4d88-8901-dec70dcaa063" />

### Cài đặt Docker và Docker compose
- Sử dụng lệnh ```sudo apt install docker.io docker-compose -y ``` để cài đặt docker và docker compose
- Sau khi cài đặt xong, sử dụng lệnh hai lệnh dưới đây để kiểm tra phiên bản docker và docker compose vừa cài đặt
```
docker --version
docker-compose --version 
```
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/6ce8aaa4-d245-4f1d-a855-0b651a1f56c6" />

-----------------------
## IV. Cấu hình để docker để chạy mà không cần tiền tố sudo
### Bước 1. Thêm user vào group docker
- Sử dụng lệnh ```sudo usermod -aG docker $USER``` để thêm user vào group docker (thay $USER bằng tên người dùng của mình, ví dụ: thuha)
- Sau khi đã thêm user vào group, ta cần đăng xuất ra và đăng nhập lại để quyền có hiệu lực. Hoặc có thể dùng lệnh ```newgrp docker``` để không cần thoát SSH ra để đăng nhập lại mà áp dụng quyền được luôn.
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/95965913-9293-40b1-a0aa-0d3d6a312476" />

=> Kết quả sau khi cấu hình để docker chạy mà không cần tiền tố sudo

-----------------
## V. Tìm hiểu tập lệnh của docker và docker compose
### 🐳 Các lệnh của Docker (Single Container):

| Lệnh | Tác dụng | Ví dụ |
| :--- | :--- | :--- |
| `docker pull` | Tải Image từ Docker Hub về máy. | `docker pull nginx:alpine` |
| `docker images` | Xem danh sách các Image đã tải. | `docker images` |
| `docker run` | Tạo và chạy một Container mới từ một image. | `docker run -d -p 80:80 --name web nginx` |
| `docker ps -a` | Xem tất cả Container (đang chạy và đã dừng). | `docker ps -a` |
| `docker exec -it` | Truy cập vào bên trong Container đang chạy. | `docker exec -it my_container bash` |
| `docker logs -f` | Xem log của Container theo thời gian thực. | `docker logs -f my_container` |
| `docker stop` | Dừng một Container. | `docker stop my_container` |
| `docker rm -f` | Xóa bỏ hoàn toàn một Container. | `docker rm -f my_container` |
| `docker rmi` | Xóa một Image khỏi máy. | `docker rmi nginx:alpine` |

### 🏗️ Các lệnh Docker Compose (Multi-Container)
| Lệnh | Tác dụng |
| :--- | :--- |
| `docker-compose up -d` | Khởi tạo và chạy toàn bộ dịch vụ ở chế độ ngầm. |
| `docker-compose down` | Dừng và xóa toàn bộ containers, networks đã tạo. |
| `docker-compose ps` | Kiểm tra trạng thái các dịch vụ trong file compose. |
| `docker-compose logs -f` | Xem log của tất cả các dịch vụ cùng lúc. |
| `docker-compose restart` | Khởi động lại các dịch vụ. |
| `docker-compose exec [service] [cmd]` | Chạy lệnh trong một service cụ thể (vd: `docker-compose exec db bash`). |
| `docker-compose build` | Build lại các Image nếu có sự thay đổi trong Dockerfile. |

---------------------
## VI. Cấu hình tường lửa trên Ubuntu cho phép các cổng 80, 1880, 9630 (Lệnh: sudo ufw allow ...)
- Chạy các lệnh sau trong cửa sổ powrshell của ubuntu để cho phép các cổng 80, 1880, 9630 hoạt động
```
sudo ufw allow 80/tcp
sudo ufw allow 1880/tcp
sudo ufw allow 9630/tcp
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/47a7d792-cc10-4358-9b50-db89cc293349" />

- Sau khi chạy xong 3 lệnh trên, chạy tiếp lệnh dưới đây để kích hoạt tường lửa:
```
sudo ufw enable
```
<img width="1919" height="1048" alt="image" src="https://github.com/user-attachments/assets/85a71155-8463-425f-aba4-beb13bbef5e3" />



