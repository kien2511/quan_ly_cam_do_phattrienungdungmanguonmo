# quan_ly_cam_do_phattrienungdungmanguonmo
# Họ và tên : Nguyễn Trung Kiên 
# Mssv : K225480106032
# Lớp : K58KTp

SỬ DỤNG DJANGO ĐỂ TẠO WEB QUẢN LÝ TIỆM CẦM ĐỒ
deadline : 23h59 ngày 09 tháng 5 năm 2026.
Link gửi bài: Tại đây
TỔ CHỨC CSDL CHO HỆ THỐNG QUẢN LÝ TIỆM CẦM ĐỒ: viết tay ra giấy, lấy điện thoại chụp lại, upload ảnh lên github (đã nói về các nghiệp vụ trên lớp, ghi bảng)

SỬ DỤNG DOCKER TRÊN UBUNTU ĐỂ:

Mariadb : chứa csdl của hệ thống này
Phpmyadmin: để soi được csdl (chỉ để xem, ko cần tạo bảng từ đây, django sẽ làm hết)
Django: build 1 docker container (dùng Dockerfile): trên nền python, sử dụng django, nhớ mount thư mục để dễ edit, edit dùng: sudo nano ten_file
sau khi có 3 service này trong file docker-compose.yml :

run nó, cấu hình để Django nhận csdl mariadb (sửa file settings.py), cấu hình user login ban đầu, mô tả các bảng trong models.py, .... (đc phép sử dụng AI để làm) => KQ được trang admin, y/c đăng nhập, vào trang admin: cho phép thêm sửa xoá dữ liệu các bảng. các trường là khoá ngoại chỉ việc chọn text (mặc dù là csdl tại trường FK đó lưu ID của PK mà nó tham chiếu : sử dụng phpmyadmin để kiểm chứng)
chú ý kết hợp ssh để chạy lệnh tác động vào django và sudo nano để edit file.
sử dụng template (file html, sử dụng cú pháp jinja2), lấy context từ 1 view home_page, để tạo trang liệt kê các con nợ đến hạn mà chưa trả tiền!
sử dụng cloudflare tunnel để public kết quả lên 1 sub-domain => chụp kết quả
Hướng dẫn:

Tạo thư mục để chứa image tự buid cho django
Vào thư mục đó tạo file tên: Dockerfile (nội dung hỏi AI xem file này cần có nội dung gì, full comment cho từng dòng lệnh trong file này => mục tiêu kép: để hiểu và để hệ thống chạy được)
AI sẽ nói cần thêm file requirements.txt để cài các thư viện cho python (cài qua lệnh pip) => tạo file requirements.txt với nội dung tưng ứng, trong file này cũng comment được => comment xem thư viện nào dùng để làm gì
Sau mỗi lần sửa đỏi có thể phải chạy lệnh dạng : docker compose exec TÊN_SERVICE_DJANGO_CỦA_BẠN python manage.py migrate để tác động vào django (còn nhiều lệnh khác chứ ko luôn như này), để django thay đổi csdl hoặc thay đổi cấu hình.

# ======================= Bài Làm ==========================

tạo dockerfile 

<img width="1476" height="560" alt="image" src="https://github.com/user-attachments/assets/e86c4c7f-4989-4c16-986c-440bb7f67640" />

nội dung 

# Dùng image python chính thức
FROM python:3.12

# Không tạo file .pyc
ENV PYTHONDONTWRITEBYTECODE=1

# Hiển thị log realtime
ENV PYTHONUNBUFFERED=1

# Thư mục làm việc trong container
WORKDIR /app

# Copy file requirements
COPY requirements.txt .

# Cài thư viện python
RUN pip install --no-cache-dir -r requirements.txt

# Copy source code
COPY ./app /app

# Mở port django
EXPOSE 8000

# Chạy server django
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]

file docker compose.yml

<img width="1176" height="400" alt="image" src="https://github.com/user-attachments/assets/b57dd219-b305-445a-97aa-ef9b306a407e" />

cấu hình file docker compose.yml

<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/ff835c80-bdb5-4585-817f-50b5c9f3b1c4" />

trong camdo có 

<img width="1917" height="797" alt="image" src="https://github.com/user-attachments/assets/f605fdad-9027-4e43-a923-5e1d8d3396ef" />

trong thư mục app có 


<img width="1917" height="1077" alt="image" src="https://github.com/user-attachments/assets/3c23c0d0-729f-489f-b562-22565edd254d" />
<img width="1912" height="597" alt="image" src="https://github.com/user-attachments/assets/a10ae51f-64ee-4940-99ee-4e8458a70f20" />

cấu hình file setting.py


<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/432d18fc-863d-46f4-b6be-7490a088bb5e" />

cấu hình file admin.py

<img width="1915" height="1043" alt="image" src="https://github.com/user-attachments/assets/6806bf15-f809-4c9a-a776-19dbdc7e2fab" />

cấu hình file models.py


<img width="1918" height="852" alt="image" src="https://github.com/user-attachments/assets/5920344b-80d1-48e9-9b3e-087ce5c8fa27" />

cấu hình file views.py

<img width="1917" height="846" alt="image" src="https://github.com/user-attachments/assets/1e3e2c45-9b78-4638-97b1-22dff4b906a9" />

chạy docker 


<img width="1918" height="1020" alt="image" src="https://github.com/user-attachments/assets/ab59a7cb-5dd4-4ad7-ba97-6630597d606c" />


<img width="1918" height="1013" alt="image" src="https://github.com/user-attachments/assets/e9a60073-c721-40d3-bcc9-702159d0559a" />

kiểm tra xem các container vhajy chưa


<img width="1918" height="262" alt="image" src="https://github.com/user-attachments/assets/02ad167d-aeb9-4790-9d0a-93b5fdc69143" />

cài tài khoản mật khẩu admin 

<img width="1531" height="217" alt="image" src="https://github.com/user-attachments/assets/8dec8f51-88d8-4cb2-9cba-7f56d3a974d1" />

cấu hình django 

<img width="1918" height="947" alt="image" src="https://github.com/user-attachments/assets/7dfc5280-48cf-475f-b68e-7733a7df8504" />


<img width="1917" height="887" alt="image" src="https://github.com/user-attachments/assets/3376e897-ecd3-4166-a942-5861cdc69b3a" />

php nhận được dữ liệu 


<img width="1918" height="883" alt="image" src="https://github.com/user-attachments/assets/693e4e44-d63f-40b7-97ac-7a85956d93ea" />


kết quả 

<img width="1868" height="877" alt="image" src="https://github.com/user-attachments/assets/53079f99-ee44-4f50-81a1-728d8461bbc9" />











