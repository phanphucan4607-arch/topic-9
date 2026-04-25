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

- Nhập IP máy Ubuntu hiện tại

-Nhấn OK -> Apply.

<img width="452" height="592" alt="image" src="https://github.com/user-attachments/assets/37563c3b-ef5a-450f-b692-a7d8a22b592b" />

