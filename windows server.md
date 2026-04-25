Thực hiện allow port, allow ip trên window fw
- Thực hiện block port, block ip trên window fw
- Thực hiện giới hạn port, giới hạn ip trên window fw chỉ cho phép ip chỉ định truy cập
- Thực hành cài đặt 
- Webserver IIS, trên Webserver IIS
  + Cài đặt website wordpress mặc định
  + Cài đặt SSL
- SQL Server: 2016 

Link download: https://software.vietnix.tech/datastore/sources/SQL_Server/sql2016/


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


