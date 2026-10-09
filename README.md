Bài 1: Quản lý người dùng giới hạn và Truyền tải dữ liệu qua SFTP trên Windows
1. Mục tiêu
Tạo tài khoản người dùng giới hạn phục vụ cho các tác vụ truyền nhận tệp tin từ xa.
Làm chủ quy trình cài đặt và kết nối SFTP bằng phần mềm client trên hệ điều hành Windows (Bitvise SSH Client, WinSCP hoặc FileZilla).
Thực hiện truyền tải tệp tin nhật ký (logs) an toàn từ máy chủ Linux về máy tính cá nhân.
2. Quá trình thực hiện trên VPS (Linux)
Bước 1: Khởi tạo tài khoản người dùng sftp-user

sudo adduser sftp-user
(Tiến hành nhập mật khẩu mạnh và điền các thông tin theo yêu cầu của hệ thống)

Bước 2: Tạo thư mục log giả lập và gán quyền

# Tạo thư mục
sudo mkdir -p /var/log/app-backup/

# Tạo file log giả lập và ghi nội dung
sudo touch /var/log/app-backup/backup-check.log
sudo bash -c 'echo "Backup status: SUCCESS at $(date)" > /var/log/app-backup/backup-check.log'

# Phân quyền sở hữu và quyền hạn
sudo chown -R root:sftp-user /var/log/app-backup
sudo chmod 750 /var/log/app-backup
sudo chmod 640 /var/log/app-backup/backup-check.log
Bước 3: Kiểm tra người dùng và quyền hạn tệp tin

id sftp-user
ls -l /var/log/app-backup/backup-check.log
Kết quả: User sftp-user không thuộc nhóm sudo (không có quyền chạy lệnh đặc quyền). Tệp tin có quyền đọc đối với group sftp-user.

3. Quá trình thực hiện trên Windows (Sử dụng SFTP Client)
Sử dụng phần mềm SFTP (ví dụ: Bitvise SSH Client) để kết nối:

Mở phần mềm Bitvise SSH Client trên Windows.
Điền IP của VPS vào ô Host.
Điền sftp-user vào ô Username.
Chọn Method là Password và nhập mật khẩu của sftp-user.
Nhấp Log in.
Khi kết nối thành công, chọn New SFTP Window, tìm đến thư mục /var/log/app-backup/ ở khung bên phải (Remote files).
Kéo thả tệp backup-check.log sang thư mục máy tính ở khung bên trái (Local files).
4. Bằng chứng kết quả (Ảnh chụp màn hình)
(Chèn ảnh chụp màn hình giao diện kết nối SFTP từ Windows bằng Bitvise/WinSCP đã kết nối thành công và tải được file log về máy tính cá nhân vào bên dưới)

Giao diện SFTP kết nối thành công và tải file

5. Kiểm tra nội dung file đã tải về trên Windows
Mở file backup-check.log đã được tải về trên máy tính Windows bằng trình soạn thảo văn bản (như Notepad) để xác nhận nội dung file khớp với file gốc trên VPS:

Backup status: SUCCESS at ...

Backup status: SUCCESS at ...
