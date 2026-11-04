# Canteen API

## 1. Giới thiệu
Canteen API là RESTful API quản lý thực đơn căng tin trường học, được xây dựng bằng Python và FastAPI.

Hệ thống hỗ trợ xem, thêm, sửa, xóa, lọc và sắp xếp món ăn.

## 2. Công nghệ
- Python
- FastAPI
- Pydantic
- Uvicorn
- Postman
- Newman

## 3. Cấu trúc dự án
- app/main.py: Khởi tạo ứng dụng FastAPI.
- app/schemas.py: Khai báo và kiểm tra dữ liệu.
- app/repository.py: Quản lý dữ liệu món ăn.
- app/routers/menu.py: Xử lý các API thực đơn.
- docs/design.md: Tài liệu thiết kế.
- postman/: Bộ kiểm thử API.

## 4. Cài đặt

Tạo môi trường ảo:

    python3 -m venv .venv
    source .venv/bin/activate

Cài đặt thư viện:

    pip install -r requirements.txt

## 5. Khởi chạy

    python -m uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload

Truy cập:
- API: http://localhost:8000
- Swagger: http://localhost:8000/docs
- Health check: http://localhost:8000/health

## 6. Danh sách API

| Phương thức | Endpoint | Chức năng |
|---|---|---|
| GET | /health | Kiểm tra hệ thống |
| GET | /menu-items | Danh sách món |
| GET | /menu-items/{item_id} | Chi tiết món |
| POST | /menu-items | Tạo món |
| PUT | /menu-items/{item_id} | Cập nhật toàn bộ |
| PATCH | /menu-items/{item_id} | Cập nhật một phần |
| DELETE | /menu-items/{item_id} | Xóa món |

## 7. Bộ lọc và sắp xếp

- category: Lọc theo danh mục.
- available: Lọc trạng thái còn hàng.
- max_price: Lọc theo giá tối đa.
- sort: Sắp xếp theo id, price hoặc name.
- order: asc hoặc desc.

Ví dụ:

    /menu-items?category=bun_pho&sort=price&order=desc
    /menu-items?available=false

## 8. Quy tắc dữ liệu

Danh mục hợp lệ: com, bun_pho, do_uong, an_vat.

Tên món từ 2 đến 100 ký tự.
Giá lớn hơn 0, tối đa 200000 và chia hết cho 1000.
Trạng thái available mặc định là true.

## 9. Mã trạng thái HTTP

- 200: Thành công.
- 201: Tạo mới thành công.
- 204: Xóa thành công.
- 404: Không tìm thấy món.
- 409: Trùng tên món.
- 422: Dữ liệu không hợp lệ.

## 10. Kiểm thử bằng Newman

Kiểm thử CRUD:

    newman run postman/Canteen-API.postman_collection.json -e postman/Canteen-Local.postman_environment.json --folder "01-Read" --folder "02-Create" --folder "03-Update" --folder "04-Delete"

Kiểm thử dữ liệu CSV:

    newman run postman/Canteen-API.postman_collection.json -e postman/Canteen-Local.postman_environment.json --folder "05-Data-driven" --iteration-data postman/negative.csv

## 11. Lưu ý

Dữ liệu hiện được lưu trong RAM. Khi khởi động lại ứng dụng, hệ thống khởi tạo lại 4 món mẫu.
