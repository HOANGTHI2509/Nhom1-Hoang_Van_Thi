# 🔐 Hệ thống nộp bài tập lớn an toàn

## 📋 Mô tả dự án

**Hệ thống nộp bài tập lớn an toàn** là một ứng dụng web được phát triển theo **Đề tài 6** - cho phép giảng viên gửi file assignment.txt đến hệ thống chấm điểm một cách bảo mật. File được chia thành 3 phần để tối ưu băng thông và được mã hóa, ký số để đảm bảo an toàn.

## 🎯 Yêu cầu đề tài

### Thuật toán bảo mật:
- **Mã hóa:** DES (CBC mode, PKCS#7 padding)
- **Trao khóa & ký số:** RSA 1024-bit (PKCS#1 v1.5 + SHA-512)
- **Kiểm tra tính toàn vẹn:** SHA-512 (HEX format)

### Giao thức 4 bước:
1. **Handshake:** "Hello!" → "Ready!"
2. **Xác thực & Trao khóa:** Ký metadata + Mã hóa SessionKey
3. **Mã hóa & Kiểm tra toàn vẹn:** Chia file → DES → SHA-512 hash → RSA signature
4. **Phía Người nhận:** Verify → Decrypt → Reconstruct → ACK/NACK

## 🏗️ Cây thư mục dự án

```
hethong-nop-baitap-antoan/
├── 📄 app.py                     # Flask application chính với Socket Server
├── 📄 requirements.txt           # Dependencies: Flask, pycryptodome
├── 📄 README.md                  # Tài liệu hướng dẫn
│
├── 📁 crypto/                    # Package các thuật toán mã hóa
│   ├── __init__.py              # Package initialization
│   ├── rsa_handler.py           # RSA 1024-bit: key gen, encrypt, sign, verify
│   ├── des_handler.py           # DES/CBC: encrypt, decrypt, PKCS#7 padding
│   ├── hash_handler.py          # SHA-512: hash calculation và verification
│   └── file_processor.py        # File processing: split, encrypt, reconstruct
│
├── 📁 templates/                 # Giao diện web Bootstrap 5 + FontAwesome
│   ├── base.html                # Template cơ sở với navbar và alerts
│   ├── index.html               # Trang chủ với giới thiệu hệ thống
│   ├── login.html               # Đăng nhập giảng viên
│   ├── register.html            # Đăng ký tài khoản mới
│   ├── dashboard.html           # Dashboard giảng viên với 3 chức năng chính
│   ├── upload.html              # Gửi file với progress tracking 9 bước
│   ├── keys.html                # Quản lý khóa RSA (tạo mới/upload)
│   ├── history.html             # Lịch sử gửi file với thống kê
│   ├── admin_login.html         # Đăng nhập admin (admin/admin123)
│   ├── admin_dashboard.html     # Dashboard admin với thống kê tổng quan
│   ├── admin_transactions.html  # Quản lý giao dịch với search/filter
│   └── admin_users.html         # Quản lý người dùng với thống kê chi tiết
│
├── 📁 uploads/                   # File tạm thời khi upload
│   └── (auto-created)
│
├── 📁 encrypted_files/           # File đã mã hóa (JSON format cho giảng viên)
│   └── (auto-created)
│
├── 📁 decrypted_files/           # File đã giải mã (cho admin download)
│   └── (auto-created)
│
├── 📁 keys/                      # Khóa hệ thống RSA
│   ├── system_private.pem       # Khóa riêng hệ thống (auto-generated)
│   └── system_public.pem        # Khóa công khai hệ thống (auto-generated)
│
├── 📁 data/                      # Database JSON
│   ├── users.json               # Thông tin người dùng và public keys
│   ├── transactions.json        # Lịch sử giao dịch với metadata
│   └── admin.json               # Cấu hình admin (admin/admin123)
│
```
## 🚀 Cài đặt và chạy

### 1. Yêu cầu hệ thống:
```bash
Python 3.8+
Flask 2.3.3
pycryptodome 3.19.0
Werkzeug 2.3.7
```

### 2. Cài đặt dependencies:
```bash
pip install -r requirements.txt
```

### 3. Chạy ứng dụng:
```bash
python app.py
```

Hệ thống sẽ tự động:
- Khởi tạo dữ liệu mặc định
- Tạo khóa RSA hệ thống
- Khởi động Socket Server (port 9999)
- Khởi động Flask App (port 5000)

### 4. Truy cập:
- **Giảng viên:** http://localhost:5000
- **Admin:** http://localhost:5000/admin/login

## 👥 Tài khoản mặc định

### Admin:
- **Username:** admin
- **Password:** admin123

### Giảng viên:
- Tự đăng ký tại `/register`

## 🔧 Tính năng chính

### Dành cho Giảng viên:
- ✅ **Đăng ký/Đăng nhập:** SHA-256 password hashing
- ✅ **Quản lý khóa RSA:** Tạo mới hoặc upload khóa công khai
- ✅ **Gửi file assignment.txt:** Max 500KB, chỉ file .txt
- ✅ **Progress tracking:** 9 bước chi tiết với animation
- ✅ **Lịch sử giao dịch:** Với thống kê và download file mã hóa
- ✅ **File validation:** Size check và format validation

### Dành cho Admin:
- ✅ **Dashboard:** Thống kê tổng quan với charts
- ✅ **Quản lý giao dịch:** Search, filter, debug details
- ✅ **Quản lý người dùng:** User stats API với real-time data
- ✅ **Download files:** File đã giải mã
- ✅ **Audit trail:** Chi tiết protocol và debug info
- ✅ **System monitoring:** Resource usage và performance

## 🔐 Giao Diện đăng nhập
![Giao diện demo](/giaodiendangnhap.jpg)
## 🔐 Giao Diện tổng quan của admin
![Giao diện demo](/tongquangiaodien.jpg)
## 🔐 Giao Diện quản lý giao dịch của admin
![Giao diện demo](/giaodienquanlygiaodich.jpg)
## 🔐 Giao Diện quản lý người dùng của admin
![Giao diện demo](/giaodienquanlynguoidung.jpg)
## 🔐 Giao Diện tổng quan  của giảng viên
![Giao diện demo](/giaodientruyenfile.jpg)
## 🔐 Giao Diện truyền file của giảng viên
![Giao diện demo](/truyenfilecuagiangvien.jpg)

