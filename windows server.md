
#### 🛠️ GIAI ĐOẠN 1: CÀI ĐẶT CÔNG CỤ REMMINA (TRÊN UBUNTU)
```
sudo apt update && sudo apt install remmina remmina-plugin-rdp -y
```
+ Ở ô chọn giao thức (bên trái ô nhập IP): Chọn RDP.

+ Ở ô nhập địa chỉ: Nhập IP của VPS: 221.132.21.141.

+ Nhấn Enter

#### 🛡️ GIAI ĐOẠN 2: THỰC HÀNH WINDOWS FIREWALL
nhấn phím win rõ wf.msc 

**Bước 2.1: Allow Port (Mở cổng 80 cho Web)**
Mục tiêu làm cho cả thới giớ vào xem web 
- chuột phải Inbound Rules 
- nhìn bên phải chọn New Rules 
- Chọn Port -> Next.
- Chọn TCP, ô Specific local ports nhập: 80 -> Next.
- Chọn Allow the connection -> Next -> Next.
- Tên: Allow_HTTP_80 -> Finish

<img width="1035" height="125" alt="image" src="https://github.com/user-attachments/assets/b5e33a4b-2c3a-4d35-b0cf-61b817e9ed23" />

**Bước 2.2 Block IP**
Mục tiêu cấm tiệt đối ip truy cập vào máy tính (ví dụ ip 1.2.3.4)
- New Rule... -> Chọn Custom -> Next.
- Nhấn Next 2 lần (để mặc định All Programs và Protocol) đến phần Scope.
- Ở ô phía dưới (Which remote IP addresses...), chọn These IP addresses -> Add.
- Nhập IP: 1.2.3.4 -> OK -> Next.
- Chọn Block the connection -> Next -> Next.
- Tên: Block_Specific_IP -> Finish.

<img width="1035" height="125" alt="image" src="https://github.com/user-attachments/assets/6a8dc30d-bd4b-46ec-b196-f353be6de37b" />

**Bước 2.3 giới hạn ip**
Mục tiêu: Mục tiêu: Chỉ IP của máy Ubuntu nhà mới được phép Remote Desktop (cổng 3389).

- Trong danh sách Inbound Rules, tìm cái dòng: Remote Desktop - User Mode (TCP-In).

- Click đúp vào nó để mở bảng cài đặt -> Chọn Tab Scope.

- Nhìn xuống ô Remote IP address, tích vào These IP addresses -> Nhấn Add.

- Nhập IP máy Ubuntu hiện tại của ông (Lấy bằng cách gõ curl ifconfig.me trên Terminal Ubuntu).

- Nhấn OK -> Apply.

<img width="478" height="642" alt="image" src="https://github.com/user-attachments/assets/370c62cf-32f5-449e-b9f0-d3f25f60d542" />

- Kiểm tra trên máy ubuntu 

<img width="935" height="95" alt="image" src="https://github.com/user-attachments/assets/f5b22ae4-55f3-4162-8505-eb3303055063" />


#### 🌐 GIAI ĐOẠN 3: CÀI ĐẶT IIS (WEB SERVER)
IIS trên windows là dùng đẻe chạy web ta sẽ thực hiện các bước sau 

Mở lại Server Manager -> Chọn Add Roles and Features.

Nhấn Next cho đến phần Server Roles.

- Tìm đến mục Web Server (IIS) -> Mở rộng nó ra -> Mở tiếp mục Web Server.

- Mở rộng mục Common HTTP Features (Các tính năng HTTP chung).

- TÍCH CHỌN vào các ô sau (phải có mấy cái này mới chạy được):

  - Static Content (Để hiện file .htm, .png).

   - Default Document (Để tự nhận diện trang chủ).

  -  HTTP Errors (Để báo lỗi chuẩn).

- Nhấn Next -> Install.

<img width="875" height="600" alt="image" src="https://github.com/user-attachments/assets/5e4ea05f-b4ae-494d-8a5b-9bc71c3ed927" />


Kiểm tra trên trình duyệt ta truy cập http://localhots 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/af2abb07-d423-4ee0-a585-64ad4a86b930" />


- **Tình trạng:** Sau khi cài đặt Role Web Server (IIS), truy cập http://localhost trả về mã lỗi HTTP 404 - Not Found. Trình duyệt Internet Explorer trên Windows Server đồng thời chặn hiển thị nội dung do các thiết lập bảo mật mặc định.

- **Nguyên nhân**: 1.  Dịch vụ IIS chưa được cài đặt thành phần Static Content (Nội dung tĩnh), dẫn đến việc máy chủ không thể xử lý các định dạng file .htm, .png.
2.  Tính năng IE Enhanced Security Configuration (IE ESC) đang ở trạng thái On, gây hạn chế quyền truy cập của trình duyệt ngay cả với các địa chỉ nội bộ.



- Giải pháp thực hiện:

  +  Truy cập Server Manager > Local Server: Chuyển trạng thái IE Enhanced Security Configuration sang Off để cho phép trình duyệt hiển thị nội dung cục bộ.

  +  Truy cập Add Roles and Features: Bổ sung Role Service Static Content và Default Document trong nhóm Common HTTP Features.

  + Thực thi lệnh iisreset qua Command Prompt để khởi động lại dịch vụ (rõ cmd nhập câu lệnh iisreset)

<img width="875" height="600" alt="image" src="https://github.com/user-attachments/assets/3317c338-1718-4c47-864e-d9b807663d42" />

#### GIAI ĐOẠN 4: SQL SERVER 2016
- Sau khi hoàn tất cấu hình web server IIS, tiếp hành triển khai SQL 2016 làm hệ quản trị cơ sở dữ liệu (database server). Việc cấu hình Mixed Mode Authentication là bắt buộc để hỗ trợ các ứng dụng web như wwordress kết nối và quản lý dữ liệu thông qua tài khoản SQL

trên vps và trình duyệt https://software.vietnix.tech/datastore/sources/SQL_Server/sql2016

trong danh sách iện ra tìm file có đuôi chấm iso và dowload về 
Sau khi tải xong, mở thư mục Downloads.

Chuột phải vào file ISO vừa tải -> Chọn Mount.

**Bước 4.1: Chạy file Setup**

Chạy file setup.exe và bắt đầu quá trình cài đặt như tui đã hướng dẫn chi tiết ở trên.
<img width="875" height="600" alt="image" src="https://github.com/user-attachments/assets/3ca6a5c3-1b35-4e03-a2f5-781436a7b8dc" />

**Bước 4.2: Cửa sổ Installation Center**
- Nhìn cột bên trái, chọn dòng Installation.

- Nhìn sang bên phải, chọn dòng đầu tiên: New SQL Server stand-alone installation or add features to an existing installation

**Bước 4.3: Đi xuyên qua các bảng phụ (Next liên tục)**
  <img width="875" height="600" alt="image" src="https://github.com/user-attachments/assets/901ec17d-6884-4c27-b39e-69e035eaa47f" />


**Bước 4.4: Feature Selection (Chọn tính năng - QUAN TRỌNG)**
Chỉ tích chọn duy nhất ô: Database Engine Services.
<img width="875" height="600" alt="image" src="https://github.com/user-attachments/assets/285774b0-bf82-4fd8-b6a3-474440381e5e" />

**Bước 4.5: Instance Configuration**

    Chọn Default Instance (để nó tự lấy tên là MSSQLSERVER).

    Nhấn Next.

    tại mục này Dòng SQL Server Browser: Hiện tại nó đang bị Disabled. hãy nhấp vào ô đó và chuyển nó sang Automatic.

   <img width="875" height="600" alt="image" src="https://github.com/user-attachments/assets/40a4262a-35c2-4e5a-bb8b-8d4dedbb73ab" />

 - Tại cái này giống như "người dẫn đường" . Khi ta dùng SSMS từ máy ubuntu kết nối vào, nó sẽ giúp tìm thây cái SQL Server này dễ dàng hơn.

 **Bước 4.6: Database Engine Configuration**
- Authentication Mode: Tích chọn Mixed Mode (SQL Server authentication and Windows authentication).

- Password: Nhập mật khẩu là Admin@123 (Nhớ ghi vào báo cáo là mật khẩu này dùng để kết nối WordPress).

- Specify SQL Server administrators: Nhấn vào nút Add Current User ở ngay phía dưới để nó nhận diện là quản trị viên.

- Nhấn Next -> Install.

<img width="875" height="600" alt="image" src="https://github.com/user-attachments/assets/bbedf672-edc5-4dd1-8d0a-bf4bb9afa8e2" />

**GIAI ĐOẠN 5. CÀI PHP, WORDRESS VÀ SSL**

Cấu hình Handler Mappings (Để IIS hiểu file .php)
<img width="866" height="616" alt="image" src="https://github.com/user-attachments/assets/e94c1974-f4f1-450c-a17d-321d9ff9f42a" />

Bước 1: Chạy lại bộ cài SSMS

    Vào lại thư mục Downloads.

    Tìm file SSMS-Setup-ENU.exe, chuột phải chọn Run as administrator.

    Lần này nhấn Install, nó sẽ không báo lỗi .NET nữa mà chạy vèo vèo. Đợi nó chạy xong (hiện chữ Setup Completed) là xong.

<img width="866" height="616" alt="image" src="https://github.com/user-attachments/assets/59a8dca1-cd1e-4851-bac8-341ace5bb617" />

🛠️ Bước 2: Đổi lại mật khẩu và Kích hoạt tài khoản sa

Ở cột bên trái, tìm thư mục Security -> Nhấn dấu cộng + để mở ra.

Mở tiếp thư mục Logins.

Tìm cái tên sa, chuột phải vào nó chọn Properties.

Tại tab General: gõ lại mật khẩu mới vào 2 ô Password (nhớ gõ chậm thôi: Admin123@)
Tại tab Status: * Chỗ Settings -> Login: Chọn Enabled (để chắc chắn nó không bị khóa).

Nhấn OK.
<img width="866" height="616" alt="image" src="https://github.com/user-attachments/assets/96c00005-ad67-4573-b7b6-a1344f8b9ffd" />
