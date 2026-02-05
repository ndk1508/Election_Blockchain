# Hệ Thống Bỏ Phiếu Phi Tập Trung Sử Dụng Ethereum Blockchain

## Giới Thiệu
Hệ thống bỏ phiếu phi tập trung sử dụng công nghệ Ethereum Blockchain là một giải pháp an toàn và minh bạch cho việc tổ chức bầu cử. Hệ thống đảm bảo hồ sơ bỏ phiếu không thể bị can thiệp, cho phép người dùng bỏ phiếu từ xa trong khi vẫn giữ tính ẩn danh và ngăn chặn gian lận.

## Tính Năng
- Xác thực và ủy quyền cử tri an toàn bằng JWT
- Sử dụng Ethereum blockchain để lưu trữ phiếu bầu minh bạch, không thể can thiệp
- Loại bỏ trung gian, đảm bảo quy trình bỏ phiếu không cần tin cậy
- Bảng điều khiển quản trị để quản lý ứng cử viên, đặt ngày bầu cử và theo dõi kết quả
- Giao diện trực quan để cử tri bỏ phiếu và xem thông tin ứng cử viên

## Yêu Cầu Hệ Thống
- Node.js (phiên bản 18.14.0)
- Metamask
- Python (phiên bản 3.9)
- FastAPI
- MySQL Database (cổng 3306)
- Ganache

## Hướng Dẫn Cài Đặt

### Bước 1: Cài đặt các phần mềm cần thiết

1. **Cài đặt Node.js**
   - Tải Node.js phiên bản 18.14.0 từ [nodejs.org](https://nodejs.org/)
   - Kiểm tra cài đặt: `node --version`

2. **Cài đặt Python**
   - Tải Python 3.9 từ [python.org](https://www.python.org/)
   - Kiểm tra cài đặt: `python --version`

3. **Cài đặt Ganache**
   - Tải từ [trufflesuite.com/ganache](https://trufflesuite.com/ganache/)
   - Cài đặt và khởi động Ganache

4. **Cài đặt Metamask**
   - Thêm extension Metamask vào trình duyệt từ [metamask.io](https://metamask.io/download/)
   - Tạo ví nếu chưa có

5. **Cài đặt MySQL**
   - Cài đặt MySQL (KHÔNG dùng XAMPP)
   - Đảm bảo MySQL chạy trên cổng 3306

### Bước 2: Thiết lập Ganache

1. Mở Ganache
2. Tạo workspace mới tên là **development**
3. Trong phần "Truffle Projects", nhấn `ADD PROJECT` và chọn file `truffle-config.js` từ thư mục dự án

### Bước 3: Thiết lập Metamask

1. Mở Metamask và tạo ví (nếu chưa có)
2. Import tài khoản từ Ganache:
   - Copy private key từ Ganache
   - Vào Metamask → Import Account → Paste private key
3. Thêm mạng Localhost vào Metamask:
   - Network name: `Localhost 7575`
   - RPC URL: `http://localhost:7545`
   - Chain ID: `1337`
   - Currency symbol: `ETH`

### Bước 4: Thiết lập Cơ Sở Dữ Liệu

1. Mở MySQL và tạo database:
   ```sql
   CREATE DATABASE voter_db;
   ```

2. Sử dụng database và tạo bảng voters:
   ```sql
   USE voter_db;
   
   CREATE TABLE voters (
       voter_id VARCHAR(36) PRIMARY KEY NOT NULL,
       role ENUM('admin', 'user') NOT NULL,
       password VARCHAR(255) NOT NULL
   );
   ```

3. Thêm dữ liệu mẫu (ví dụ):
   ```sql
   INSERT INTO voters (voter_id, role, password) VALUES 
   ('admin001', 'admin', 'admin123'),
   ('user001', 'user', 'user123'),
   ('user002', 'user', 'user456');
   ```

### Bước 5: Cấu hình Database API

1. Tạo file `.env` trong thư mục `Database_API/`:
   ```
   DB_HOST=localhost
   DB_USER=root
   DB_PASSWORD=your_mysql_password
   DB_NAME=voter_db
   DB_PORT=3306
   JWT_SECRET=your_secret_key_here
   ```

2. Thay `your_mysql_password` bằng mật khẩu MySQL của bạn
3. Thay `your_secret_key_here` bằng một chuỗi bí mật bất kỳ

### Bước 6: Cài đặt Dependencies

1. Mở terminal trong thư mục dự án

2. Cài đặt Truffle globally:
   ```bash
   npm install -g truffle
   ```

3. Cài đặt Node.js dependencies:
   ```bash
   npm install
   ```

4. Cài đặt Python dependencies:
   ```bash
   pip install fastapi mysql-connector-python pydantic python-dotenv uvicorn PyJWT
   ```

### Bước 7: Biên dịch Smart Contracts

1. Mở terminal trong thư mục dự án
2. Khởi động Ganache và mở workspace "development"
3. Chạy lệnh:
   ```bash
   truffle console
   ```
4. Trong truffle console, chạy:
   ```
   compile
   ```
5. Thoát console bằng lệnh `.exit` hoặc Ctrl+C

### Bước 8: Bundle JavaScript

Chạy lệnh sau để bundle file app.js:
```bash
browserify ./src/js/app.js -o ./src/dist/app.bundle.js
```

## Cách Sử Dụng

### Khởi động hệ thống

1. **Khởi động Ganache**
   - Mở Ganache và workspace "development"

2. **Khởi động Database API** (Terminal 1)
   ```bash
   cd Database_API
   uvicorn main:app --reload --port 8000
   ```

3. **Khởi động Node Server** (Terminal 2)
   ```bash
   node index.js
   ```

4. **Truy cập ứng dụng**
   - Mở trình duyệt và truy cập: `http://localhost:3000/src/html/login.html`

### Sử dụng hệ thống

#### Đăng nhập
1. Mở trang login
2. Nhập Voter ID và Password (dựa trên dữ liệu đã thêm vào MySQL)
3. Nhấn "Đăng Nhập"
4. Kết nối với Metamask khi được yêu cầu

#### Dành cho Admin
Sau khi đăng nhập với tài khoản admin:
1. **Thêm ứng cử viên:**
   - Nhập tên và đảng phái
   - Nhấn "Thêm Ứng Cử Viên"
   - Xác nhận giao dịch trên Metamask

2. **Đặt thời gian bầu cử:**
   - Chọn ngày bắt đầu và kết thúc
   - Nhấn "Xác Định Ngày"
   - Xác nhận giao dịch trên Metamask

#### Dành cho Cử tri
Sau khi đăng nhập với tài khoản user:
1. Xem danh sách ứng cử viên
2. Chọn một ứng cử viên
3. Nhấn nút "Bỏ Phiếu"
4. Xác nhận giao dịch trên Metamask
5. Chờ giao dịch được xác nhận trên blockchain

## Xử Lý Lỗi Thường Gặp

### Lỗi kết nối Metamask
- Đảm bảo Metamask đã được kết nối với mạng Localhost 7575
- Kiểm tra lại cấu hình mạng trong Metamask

### Lỗi Database
- Kiểm tra MySQL đang chạy
- Xác nhận thông tin đăng nhập trong file `.env`
- Đảm bảo database `voter_db` và bảng `voters` đã được tạo

### Lỗi Ganache
- Đảm bảo Ganache đang chạy
- Port 7545 không bị chiếm dụng
- Workspace "development" đã được cấu hình đúng

### Lỗi biên dịch Smart Contract
- Chạy lại `truffle compile` trong truffle console
- Kiểm tra file Solidity không có lỗi cú pháp

### Lỗi Bundle JavaScript
- Đảm bảo đã cài đặt browserify: `npm install -g browserify`
- Chạy lại lệnh bundle

## Cấu Trúc Thư Mục

```
├── contracts/              # Smart contracts Solidity
├── Database_API/           # FastAPI backend
├── migrations/             # Truffle migration scripts
├── public/                 # Hình ảnh và tài nguyên
├── src/
│   ├── css/               # File CSS
│   ├── html/              # Trang HTML
│   ├── js/                # File JavaScript
│   └── dist/              # Bundled JavaScript
├── index.js               # Node.js server
├── package.json           # Node dependencies
└── truffle-config.js      # Cấu hình Truffle
```

## Lưu Ý Quan Trọng

- Dự án này dùng để học tập, không nên sử dụng trong môi trường production
- Luôn backup dữ liệu quan trọng
- Không chia sẻ private key của Metamask
- Mỗi cử tri chỉ được bỏ phiếu một lần
- Admin không thể bỏ phiếu
- Chỉ có thể bỏ phiếu trong khoảng thời gian đã định

## Hỗ Trợ

Nếu gặp vấn đề, hãy kiểm tra:
1. Tất cả services đang chạy (Ganache, MySQL, API server, Node server)
2. Metamask đã kết nối đúng mạng
3. Database đã được cấu hình đúng
4. Console của trình duyệt để xem lỗi JavaScript
