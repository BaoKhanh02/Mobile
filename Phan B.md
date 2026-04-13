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
<img width="1920" height="1029" alt="image" src="https://github.com/user-attachments/assets/fc5d5614-93d4-4a5d-955d-99f939d940f6" />

### 3. Cấu hình mạng trong Ubuntu cho phép SSH truy cập được từ CMD của windows

#### Gõ lệnh sau trên cửa sổ ubuntu trên máy ảo VM_Ware để lấy ip máy
```
ip a
```
<img width="1920" height="1029" alt="image" src="https://github.com/user-attachments/assets/918932da-2650-4514-b2ed-425c4bcb8e8b" />

```
IP là 192.168.15.128
```
#### Sau khi lấy được IP, mở cmd hoặc PowerShell trên windows và gõ lệnh dưới đây để truy cập SSH vào Ubuntu
```
thực hiện lệnh: ssh baokhanh@192.168.15.128
```
#### Tiếp theo nhập pasword, nếu đăng nhập thành công thì sẽ chuyển cửa sổ vào terminal của Ubuntu
<img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/2d33fd67-f067-4306-b3ac-cfeb88b2cc47" />

---------------------
## II. Tìm hiểu các lệnh cơ bản của Ubuntu
Tài liệu này trình bày các lệnh cơ bản khi làm việc với Ubuntu.

---

## 1. Liệt kê file trong thư mục
ls
<img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/492e79c8-3aad-4a23-bbc8-bd7d66f1e726" />

## 2. Tạo thư mục
mkdir nameFolder
<img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/0fe4d02f-cb2b-4190-8e94-e6dfcc2d9482" />

## 3. Chuyển thư mục làm việc
cd path
<img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/bf81995b-6e3e-4110-943a-f763552988bb" />

## 4. Sao chép file
cp file_nguồn path/file_đích
<img width="1920" height="1029" alt="image" src="https://github.com/user-attachments/assets/3aa08aa1-70ee-453c-8376-f32e42e34b85" />

cp aaa.txt ~/Downloads/
<img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/24b8522d-cd77-4dd9-a811-791316de7451" />

Kết quả
<img width="1920" height="1029" alt="image" src="https://github.com/user-attachments/assets/88458d51-2670-46d3-a71e-c71f9b99988e" />

## 5. Thay đổi quyền file
sudo chmod xxx filename
<img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/99106ccb-0bdf-45ee-b8d3-6bb606eff0aa" />

Cho phép toàn quyền truy cập vào file aaa.txt
## 6. Chỉnh sửa file
sudo nano tenfile
<img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/29dfdea2-ed2b-4ecd-8df4-27b9f84e09a0" />

-----------------------
## III. cài đặt docker và docker compose cho Ubuntu
### Cài đặt Docker và Docker compose
- Sử dụng lệnh ```sudo apt install docker.io docker-compose -y ``` để cài đặt docker và docker compose
<img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/14ea9671-1be2-4b0f-9a4d-be9a91877a7d" />

- Sau khi cài đặt xong, sử dụng lệnh hai lệnh dưới đây để kiểm tra phiên bản docker và docker compose vừa cài đặt
```
docker --version
docker-compose --version 
```
<img width="970" height="95" alt="image" src="https://github.com/user-attachments/assets/2c9be381-1dff-456f-93cc-1f51a0086590" />

-----------------------
## IV. Cấu hình để docker để chạy mà không cần tiền tố sudo
### Bước 1. Thêm user vào group docker
- Sử dụng lệnh ```sudo usermod -aG docker $USER``` để thêm user vào group docker (thay $USER bằng tên người dùng của mình, ví dụ: thuha)
- Sau khi đã thêm user vào group, ta cần đăng xuất ra và đăng nhập lại để quyền có hiệu lực. Hoặc có thể dùng lệnh ```newgrp docker``` để không cần thoát SSH ra để đăng nhập lại mà áp dụng quyền được luôn.
<img width="1483" height="762" alt="image" src="https://github.com/user-attachments/assets/4789cc42-6e64-4f80-821a-6dc124a6b130" />

=> Kết quả sau khi cấu hình để docker chạy mà không cần tiền tố sudo

-----------------
## V. Tìm hiểu tập lệnh của docker và docker compose
### 🐳 Các lệnh Docker (Single Container)

| Lệnh | Tác dụng | Ví dụ |
| :--- | :--- | :--- |
| `docker pull` | Tải Image từ Docker Hub về máy | `docker pull nginx:alpine` |
| `docker images` | Xem danh sách Image đã tải | `docker images` |
| `docker run` | Tạo và chạy Container từ Image | `docker run -d -p 80:80 --name web nginx` |
| `docker ps` | Xem container đang chạy | `docker ps` |
| `docker ps -a` | Xem tất cả container | `docker ps -a` |
| `docker exec -it` | Truy cập vào container đang chạy | `docker exec -it web bash` |
| `docker logs -f` | Xem log realtime | `docker logs -f web` |
| `docker stop` | Dừng container | `docker stop web` |
| `docker rm -f` | Xóa container | `docker rm -f web` |
| `docker rmi` | Xóa image | `docker rmi nginx:alpine` |

---

### 🏗️ Các lệnh Docker Compose (Multi-Container)

⚠️ **Lưu ý quan trọng:**  
Trên Ubuntu 24, sử dụng:
👉 `docker compose` (KHÔNG dùng `docker-compose`)

---

| Lệnh | Tác dụng |
| :--- | :--- |
| `docker compose up -d` | Khởi tạo và chạy tất cả service ở chế độ nền |
| `docker compose down` | Dừng và xóa toàn bộ container |
| `docker compose ps` | Kiểm tra trạng thái service |
| `docker compose logs -f` | Xem log tất cả service |
| `docker compose restart` | Khởi động lại service |
| `docker compose exec [service] [cmd]` | Chạy lệnh trong container |
| `docker compose build` | Build lại image |

---------------------
## VI. Cấu hình tường lửa trên Ubuntu cho phép các cổng 80, 1880, 9630 (Lệnh: sudo ufw allow ...)
- Chạy các lệnh sau trong cửa sổ powrshell của ubuntu để cho phép các cổng 80, 1880, 9630 hoạt động
```
sudo ufw allow 80/tcp
sudo ufw allow 1880/tcp
sudo ufw allow 9630/tcp
sudo ufw enable
```
<img width="1483" height="762" alt="image" src="https://github.com/user-attachments/assets/574b1cc7-50e5-421b-bd02-217266710897" />
