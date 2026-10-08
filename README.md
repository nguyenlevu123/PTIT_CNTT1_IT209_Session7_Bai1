Bài 1: Khảo sát FHS và Phân quyền File/Folder nâng cao
Mục tiêu
Nắm vững cấu trúc thư mục tiêu chuẩn theo FHS (Filesystem Hierarchy Standard) trên Linux.
Thực hành phân quyền tập tin và thư mục với chmod (phân quyền OCTAL) và chown nâng cao.
1. Khảo sát Cấu trúc Thư mục FHS (Filesystem Hierarchy Standard)
Thư mục	Mục đích sử dụng
/etc	Chứa các tệp tin cấu hình hệ thống (Nginx, SSH, Sysctl, v.v.).
/var/log	Chứa các tệp tin nhật ký hoạt động (System logs, App logs, Web logs).
/bin & /usr/bin	Chứa các lệnh thực thi nhị phân cơ bản cho người dùng hệ thống.
/opt	Chứa các phần mềm và ứng dụng của bên thứ 3 (Third-party applications).
/home	Thư mục cá nhân của người dùng thông thường trong hệ thống.
2. Các Bước Thực hiện Phân quyền Nâng cao
Bước 1: Khởi tạo thư mục và tệp tin dự án
sudo mkdir -p /opt/secure-app/
sudo touch /opt/secure-app/config.env
sudo touch /opt/secure-app/app.py
Bước 2: Thiết lập quyền sở hữu và phân quyền truy cập
# Gán quyền sở hữu cho user sysadmin và group devops
sudo chown -R sysadmin:devops /opt/secure-app

# Cấu hình phân quyền chmod (OCTAL)
# 750 cho thư mục /opt/secure-app (rwxr-x---)
sudo chmod 750 /opt/secure-app

# 640 cho config.env (rw-r----) - Chỉ owner đọc/ghi, group chỉ đọc, người khác bị cấm
sudo chmod 640 /opt/secure-app/config.env

# 755 cho app.py (rwxr-xr-x) - Cho phép thực thi
sudo chmod 755 /opt/secure-app/app.py
3. Kiểm tra Trạng thái (ls -la /opt/secure-app)
$ ls -la /opt/secure-app
total 12
drwxr-x--- 2 sysadmin devops 4096 Oct  7 11:15 .
drwxr-xr-x 4 root     root   4096 Oct  7 11:14 ..
-rwxr-xr-x 1 sysadmin devops    0 Oct  7 11:15 app.py
-rw-r----- 1 sysadmin devops    0 Oct  7 11:15 config.env
4. Kết luận
Việc phân quyền theo chuẩn 750/640 giúp cô lập file cấu hình nhạy cảm config.env, ngăn chặn các user không thuộc nhóm devops đọc dữ liệu bí mật.
