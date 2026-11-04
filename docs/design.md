# TÀI LIỆU THIẾT KẾ CANTEEN API

## 1. Tổng quan

Canteen API là RESTful API quản lý thực đơn căng tin trường học, sử dụng Python và FastAPI.

Chức năng:
- Xem danh sách và chi tiết món ăn.
- Thêm món ăn.
- Cập nhật toàn bộ thông tin.
- Cập nhật một phần thông tin.
- Xóa món ăn.
- Lọc và sắp xếp thực đơn.

## 2. Kiến trúc hệ thống

Hệ thống gồm 4 thành phần chính:

### 2.1. Main - app/main.py
Khởi tạo ứng dụng FastAPI, đăng ký router thực đơn và cung cấp API /health.

### 2.2. Router - app/routers/menu.py
Tiếp nhận HTTP request, kiểm tra dữ liệu, gọi Repository và trả về response.

### 2.3. Schema - app/schemas.py
Sử dụng Pydantic để định nghĩa và kiểm tra dữ liệu.

Các schema:
- MenuItemCreate: Dữ liệu tạo món.
- MenuItemUpdate: Dữ liệu cập nhật.
- MenuItemOut: Dữ liệu trả về.

### 2.4. Repository - app/repository.py
Quản lý dữ liệu món ăn trong RAM.

Các phương thức:
- list(): Lấy danh sách món.
- get(): Tìm món theo ID.
- find_by_name(): Tìm món theo tên.
- add(): Thêm món.
- update(): Cập nhật món.
- delete(): Xóa món.

## 3. Thiết kế dữ liệu

### 3.1. MenuItem

| Thuộc tính | Kiểu dữ liệu | Ý nghĩa |
|---|---|---|
| id | Integer | Mã món |
| name | String | Tên món |
| category | String | Danh mục |
| price | Integer | Giá |
| available | Boolean | Trạng thái còn hàng |
| updated_at | DateTime | Thời gian cập nhật |

### 3.2. Danh mục

Các danh mục hợp lệ:
- com
- bun_pho
- do_uong
- an_vat

### 3.3. Quy tắc dữ liệu

- Tên món từ 2 đến 100 ký tự.
- Giá lớn hơn 0.
- Giá tối đa 200000.
- Giá phải chia hết cho 1000.
- Danh mục phải thuộc danh sách cho phép.
- available có kiểu Boolean.

## 4. Thiết kế API

| Phương thức | Endpoint | Chức năng |
|---|---|---|
| GET | /health | Kiểm tra hệ thống |
| GET | /menu-items | Lấy danh sách |
| GET | /menu-items/{item_id} | Lấy chi tiết |
| POST | /menu-items | Tạo món |
| PUT | /menu-items/{item_id} | Cập nhật toàn bộ |
| PATCH | /menu-items/{item_id} | Cập nhật một phần |
| DELETE | /menu-items/{item_id} | Xóa món |

### 4.1. GET danh sách
Hỗ trợ lọc theo category, available, max_price và sắp xếp theo id, price, name.

### 4.2. POST tạo món
Kiểm tra tên không trùng. Nếu hợp lệ, tạo món mới, trả về HTTP 201 và header Location.

### 4.3. PUT cập nhật
Thay thế toàn bộ thông tin món ăn. Trả về 404 nếu không tìm thấy ID, 409 nếu tên bị trùng.

### 4.4. PATCH cập nhật
Chỉ cập nhật các trường được gửi lên. Các trường còn lại giữ nguyên.

### 4.5. DELETE
Xóa món theo ID và trả về HTTP 204 khi thành công.

## 5. Xử lý lỗi

| HTTP Status | Ý nghĩa |
|---|---|
| 404 | Không tìm thấy món |
| 409 | Trùng tên món |
| 422 | Dữ liệu đầu vào không hợp lệ |

## 6. Kiểm thử

Sử dụng Postman và Newman với 5 nhóm:
- 01-Read: Kiểm thử đọc dữ liệu.
- 02-Create: Kiểm thử tạo món.
- 03-Update: Kiểm thử cập nhật.
- 04-Delete: Kiểm thử xóa.
- 05-Data-driven: Kiểm thử dữ liệu CSV.

File CSV gồm 6 trường hợp: thiếu tên, giá âm, giá không hợp lệ, giá vượt giới hạn, sai danh mục và trùng tên.

## 7. Giới hạn hệ thống

Phiên bản hiện tại lưu dữ liệu trong RAM, chưa sử dụng hệ quản trị cơ sở dữ liệu. Dữ liệu được khởi tạo lại khi ứng dụng khởi động.
