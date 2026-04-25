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

Bước 2.1: Allow Port (Mở cổng 80 cho Web)
Mục tiêu làm cho cả thới giớ vào xem web 
- chuột phải Inbound Rules 
- nhìn bên phải chọn New Rules 
- Chọn Port -> Next.
- Chọn TCP, ô Specific local ports nhập: 80 -> Next.
- Chọn Allow the connection -> Next -> Next.
- Tên: Allow_HTTP_80 -> Finish

<img width="1035" height="125" alt="image" src="https://github.com/user-attachments/assets/b5e33a4b-2c3a-4d35-b0cf-61b817e9ed23" />
