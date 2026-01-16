# Hệ thống Quản lý Cảm xúc Học sinh

Ứng dụng web quản lý và theo dõi cảm xúc học sinh hàng ngày, được xây dựng bằng Node.js, Express, PostgreSQL và EJS.

## Tính năng

### Dành cho Admin:
- Xem thống kê cảm xúc theo ngày, tháng
- Lọc dữ liệu theo họ tên, cảm xúc, khoảng thời gian
- Xem danh sách ngày học sinh chưa nhập cảm xúc
- Xuất báo cáo Excel (CSV)
- Quản lý người dùng (thêm, sửa, xóa)

### Dành cho Client (Học sinh):
- Ghi nhật ký cảm xúc hàng ngày
- Xem lại lịch sử cảm xúc
- Chỉnh sửa/xóa cảm xúc đã nhập
- Đổi mật khẩu

## Công nghệ sử dụng

- **Backend**: Node.js, Express.js
- **Database**: PostgreSQL (Supabase)
- **Template Engine**: EJS
- **Frontend**: HTML, CSS, JavaScript
- **UI Library**: SweetAlert2, Font Awesome

## Cài đặt

### 1. Cài đặt dependencies

```bash
cd Nodejs
npm install
```

### 2. Cấu hình Database

Cập nhật file `.env` với thông tin database PostgreSQL của bạn:

```env
PORT=3000
DB_HOST=db.fxbsvhdyilztynexwffq.supabase.co
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=your_password_here
DB_NAME=postgres
DB_SCHEMA=lopcolien
SESSION_SECRET=camxuchocsinh_secret_key_2025
```

### 3. Tạo Schema, Tables và Import dữ liệu

**Cách nhanh nhất:** Copy toàn bộ nội dung file `database/run_all.sql` và chạy trong Supabase SQL Editor.

File này sẽ:
- Tạo schema `lopcolien`
- Tạo tables `users` và `emotions`
- Import 48 users (1 admin + 47 học sinh)
- Import 20 bản ghi cảm xúc mẫu
- Reset sequences

```bash
# Hoặc dùng psql
psql -h db.fxbsvhdyilztynexwffq.supabase.co -U postgres -d postgres -f database/run_all.sql
```

Chi tiết xem file `database/README.md`

### 4. Chạy ứng dụng

```bash
# Development mode (với nodemon)
npm run dev

# Production mode
npm start
```

Ứng dụng sẽ chạy tại: `http://localhost:3000`

## Tài khoản mặc định

- **Username**: admin
- **Password**: admin123
- **Role**: admin

## Cấu trúc thư mục

```
Nodejs/
├── config/
│   └── database.js          # Cấu hình kết nối PostgreSQL
├── database/
│   └── schema.sql           # Schema PostgreSQL
├── middleware/
│   └── auth.js              # Middleware xác thực
├── public/
│   ├── css/                 # File CSS
│   ├── js/                  # File JavaScript
│   └── images/              # Hình ảnh
├── routes/
│   ├── admin.js             # Routes admin
│   ├── api.js               # API endpoints
│   ├── auth.js              # Routes đăng nhập/đăng xuất
│   ├── client.js            # Routes client
│   └── users.js             # Routes quản lý user
├── views/
│   ├── admin.ejs            # Trang admin
│   ├── client.ejs           # Trang client
│   ├── edit_user.ejs        # Trang sửa user
│   ├── index.ejs            # Trang đăng nhập
│   └── user_management.ejs  # Trang quản lý user
├── .env                     # Biến môi trường
├── .gitignore
├── package.json
├── README.md
└── server.js                # Entry point
```

## API Endpoints

### Authentication
- `POST /login` - Đăng nhập
- `POST /logout` - Đăng xuất

### Client
- `GET /client` - Trang client
- `POST /api/save-emotion?action=insert` - Thêm cảm xúc
- `POST /api/save-emotion?action=update` - Cập nhật cảm xúc
- `POST /api/save-emotion?action=delete` - Xóa cảm xúc
- `POST /api/change-password` - Đổi mật khẩu

### Admin
- `GET /admin` - Trang admin dashboard
- `GET /admin/export` - Xuất CSV
- `GET /admin/users` - Quản lý người dùng
- `POST /admin/users/add` - Thêm user
- `POST /admin/users/update/:id` - Cập nhật user
- `POST /admin/users/delete` - Xóa user

## Lưu ý

- Mật khẩu được lưu dạng plain text (nên mã hóa trong production)
- Session cookie có thời hạn 1 năm
- Database sử dụng soft delete (del = 1)
- Schema mặc định: `lopcolien`

## Chuyển đổi từ PHP

Ứng dụng này được chuyển đổi từ PHP/MySQL sang Node.js/PostgreSQL với:
- Giữ nguyên 100% giao diện và chức năng
- Chuyển từ MySQL sang PostgreSQL
- Sử dụng parameterized queries để tránh SQL injection
- Session management với express-session
- RESTful API design

## License

MIT
