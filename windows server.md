<img width="817" height="595" alt="image" src="https://github.com/user-attachments/assets/7a7d328b-3b78-4761-bf14-082eff03ad93" />
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


**GIAI ĐOẠN 4. - Webserver IIS, trên Webserver IIS**
Bước 1 cài đặt php phiên bản 8.2 
- tải về và tiến hahf giải nén vào thư mục C:\inetpub\wwwroot
<img width="833" height="506" alt="image" src="https://github.com/user-attachments/assets/3c4e0940-39b4-495e-8450-e034d18d9666" />


**Bước 2 cấu hình cho php**
ta phải mở khóa các tinh ăng để PHP có thể chạy được wordpress.
truy cạp thoe đường dẫn **c:\php** và tìm file có tên **php.ini**
- tìm vào xóa dấu ; ở các dòng sau

```
extension_dir = "ext" (Dòng này cực quan trọng để nó tìm thấy thư mục ext).

extension=curl

extension=gd

extension=mbstring

extension=openssl

extension=pdo_mysql (Nếu  có ý định dùng MySQL sau này)
```
  
- save

- mở IIS Manager -> chọn server ở cột bên trái -> nhấn resart ở cột bên phải

**Bước 3. triển khai wordpress % phân quyền**
- vào c:\inetpub\wwwroot, xóa sạch file cũ
- copy toàn bộ file bên trong thư mục wordpress (đã được giải nén) và dán vào c:inetpub\wwwroot.
- cấp quyền cho thư mục
  + chuột phải vào wwwroot -> Properties -> security -> edit
  + nhấn Add -> rõ IIS_IUSRS -> ok
  + tích vào full control -> oke -> oke
    
  <img width="369" height="505" alt="image" src="https://github.com/user-attachments/assets/26f0e05f-b8cf-4f3c-a698-6cc4b456c506" />

- cáu hình IIS uw tiên chạy file wordpress
để màn hình xanh biến mất và hiện trạng thái cài đặt wordpress:
+ mở IIS manager.
+ chọn Default Web Site ở cột bên trái
+ nhấp vào Default Document ở giữa
+ nhìn sang cột bên phải nhấn Add
+ gõ index.php rồi oke
+ chọn index.php nhấn Move up ở cột bên phải cho nó lên trên cùng   

**Tiến hành cài Visual Studio**
- đa số các phần mềm chạy trên windows (như PHP, MySQL) điều được viết bằng ngôn ngữ lập trình trình c++ và biên dịch bằng công cụ Visual Studio.
- khi lập trình viên tạo ra phần mềm, họ sử dụng những đoạn code có sẵn từ Microsoft. Để những phần mềm đó chạy được trên máy tính, máy chúng ta phải có bộ thư viện đó thì nó mới hiểu được các câu lệnh từ C++ đó là gì.

tải file và chạy nhấn install 
<img width="476" height="319" alt="image" src="https://github.com/user-attachments/assets/f60c68b5-7da0-45aa-b6ba-02af196c4f89" />


**Tiến hành cài đặt MySQL 5.7**
Vào đường link : https://dev.mysql.com/downloads/installer/

<img width="910" height="983" alt="image" src="https://github.com/user-attachments/assets/e9b9228a-9a81-4b75-b02d-5694bbd39fc1" />
Tiến Hành Tạo Database Và Tạo Website WordPress.
- mở cmd với quyền admin gõ lệnh sau đó nó sẽ hỏi hỏi mật khẩu lúc tạo MySQL

"C:\Program Files\MySQL\MySQL Server 5.7\bin\mysql.exe" -u root -p

```

mysql> CREATE DATABASE wordpress_db;
Query OK, 1 row affected (0.00 sec)

mysql> CREATE USER 'sa'@'localhost' IDENTIFIED BY 'Admin@123';
Query OK, 0 rows affected (0.00 sec)

mysql> GRANT ALL PRIVILEGES ON wordpress_db.* TO 'sa'@'localhost';
Query OK, 0 rows affected (0.00 sec)

mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec)
```
**Cài môi trường PHP.**
Cài PHP Manager: https://www.iis.net/downloads/community/2018/05/php-manager-150-for-iis-10
cài bản 1.5.0
<img width="602" height="305" alt="image" src="https://github.com/user-attachments/assets/7abb62da-a9de-4f2b-a18c-408306c47960" />

**config môi trường PHP**
ta vào trong phần IIS để config 
ta truy cập Server manager -> tools -> IIS
- chọn PHP Manager
<img width="602" height="305" alt="image" src="https://github.com/user-attachments/assets/38986931-3900-497d-9839-c24d702695fd" />

- Sau đó ta chọn Register new PHP viersion để thêm thư mục php ta đã giải nén ở trên 
- Sau khi xong ta sẽ được như thế này
<img width="602" height="305" alt="image" src="https://github.com/user-attachments/assets/d41565ad-bd89-423b-9395-859411d55413" />

tiến hiền lên trình duyệt truy cập http://localhost 
<img width="910" height="983" alt="image" src="https://github.com/user-attachments/assets/287eb2e9-540a-495b-af44-e95c367d78af" />

Điền thông tin như sau 
<img width="910" height="983" alt="image" src="https://github.com/user-attachments/assets/8740efaf-ce06-4985-bf5f-12764b2a02e5" />

- login 
<img width="1809" height="902" alt="image" src="https://github.com/user-attachments/assets/6a1a34c6-6db3-4284-abb8-177b260a67c5" />

##### Cài đặt SSL
- Vào IIL Manager
- tìm mục Server Certificates
- ở cột Actions chọn: Create Self-Signed Certificate...
<img width="202" height="207" alt="image" src="https://github.com/user-attachments/assets/686278e5-1e67-4ccb-91b4-177a25df3e91" />

- đặt tên
<img width="677" height="538" alt="image" src="https://github.com/user-attachments/assets/33010aaa-424f-4299-86d4-4ca00e79b1b1" />

**Gán SSL vào web wordpress**
- ở cột bên trái tìm đến mục: Default Web Site
- nhấn Bindings...
- Nhấn Add
- type: Chọn https
- SSL certificate: chọn tên vừa mới đặt ở trên

<img width="677" height="538" alt="image" src="https://github.com/user-attachments/assets/1806d717-b781-4e9a-9c4e-e98e6b2560a0" />


#### Cài SQL server 2016 trên Windows Server 2016
SQl Server 2016 là một phiên bản của hệ quản trị cơ sở dữ liệu (DBMS) do Microsoft phát triển và phát hành. Nó là một bản cập nhật lớn cho dòng sản phẩm Microsoft SQL Server và được phát hành vào tháng 6 năm 2016. Dưới đây là một số tính năng của SQL Server 2016

- Always Encrypted: Cung cấp khả năng mã hóa dữ liệu trong cơ sở dữ liệu và ghi giải mã nó khi cần thiết, giwux cho dữ liệu luôn an toàn trong quá trình truyền tải và lưu trữ.
- Strech Database: Cho phép di chuyển một phần bảng dữ liệu và Azure để giảm chi phí lưu trữ địa phương và tối ưu hóa hiệu suất truy vấn.
- Polybase: Kết hợp dữ liệu từ cơ sỡ dữ liệu quan hệ với dữ liệu không quan hệ, giúp người dùng truy vấn và kếthợp dữ liệu từ nhiều nguồn khác nhau.
- JSON Support: Hỗ trợ xử lý và truy vấn dữ liệu JSON natively, làm cho việc làm việc với dữ liệu dạng JSON trở nên dễ dàng hơn
- Query Store: Lưu trữ lịch sử của các kế hoạch truy vấn và thống kê thay đổi, giúp quản trị viên và nhà phân tích hiểu rõ hiệu suất của các truy vấn.
- Row-level Security (RLS): Cho phép xác định quyền truy cập dữ liệu dựa trên các điều kiện được xác định, giúp tăng cường bảo mật.
- Dynamic Data Masking: Ẩn dữ liệu nhạy cảm từ người dùng không có quyền truy cập, giảm rủi ro bảo mật.

trên vps và trình duyệt https://software.vietnix.tech/datastore/sources/SQL_Server/sql2016

trong danh sách iện ra tìm file có đuôi chấm iso và dowload về 
Sau khi tải xong, mở thư mục Downloads.

Chuột phải vào file ISO vừa tải -> Chọn Mount.

**Chạy file Setup**
- Chọn Install
- Cung cấp Product Key nếu muôn sử dụng bản Enterpise đầy đủ
<img width="825" height="632" alt="image" src="https://github.com/user-attachments/assets/2f12c385-d257-4ea3-8b9a-8436dff4f1b1" />

- chấp nhận chính sách của Microsoft. Sau đó Next
<img width="825" height="632" alt="image" src="https://github.com/user-attachments/assets/f83d5e0f-caf9-40b3-945f-c9393be306d3" />

- Chọn các tính năng mà muốn cài đặt. Chắc chắn rằng "Database Engine Services" được cài đặt máy chủ SQL. Ở đây ta sẽ chọn tất cả và bỏ chọn "PolyBase Query Serice for External Data".
- sau đó Next 
<img width="825" height="632" alt="image" src="https://github.com/user-attachments/assets/e2b64f5d-aa29-41a3-8ba6-561dc30c0cca" />

Next
<img width="825" height="632" alt="image" src="https://github.com/user-attachments/assets/3ad2dc2d-d0df-4871-978c-2513a50fc2bd" />

Tiếp tục để nguyên vậy và bấm Next.
<img width="817" height="595" alt="image" src="https://github.com/user-attachments/assets/ecdd98f6-4b48-4df6-b1b1-1822c54ce79f" />

Bấm Add Current User để thêm user quản lý sau đó bấm next.
<img width="817" height="595" alt="image" src="https://github.com/user-attachments/assets/1cb8dc53-4c16-46d6-93bd-0d8ff4babbcf" />

- ở đây cũng vậy
<img width="817" height="595" alt="image" src="https://github.com/user-attachments/assets/21578b05-f8d3-43f0-80f9-b9c50ce607bd" />

- ở phần này chúng ta để mặc định sau đó bấm next
  <img width="817" height="595" alt="image" src="https://github.com/user-attachments/assets/c36d81f8-8cec-4b3b-9570-e98738a44070" />

Bấm vào Add Current User để thêm user quản lý, sau đó bấm  Next  để tiếp tục.

- Bấm Accept trước rồi Next
<img width="817" height="595" alt="image" src="https://github.com/user-attachments/assets/57b81ceb-27ce-4e81-a693-8761cb0dbe33" />

Tới đây ta cần cài bổ sung các thư viện này cho Microsoft bằng cá truy cập và 2 link mà SQL server 2016 hiện lên, sau khi cài ta sẽ chọn đường dẫn tới thư mục tải file .cab đó về.
```
https://go.microsoft.com/fwlink/?LinkId=761266&lcid=1033 
 https://go.microsoft.com/fwlink/?LinkId=735051&lcid=1033 
```
<img width="384" height="46" alt="image" src="https://github.com/user-attachments/assets/b641675b-9d95-4317-8004-7c90b9596a28" />

- kết quả sẽ như thế này
<img width="814" height="604" alt="image" src="https://github.com/user-attachments/assets/096a972c-57de-4950-b665-b967b0a05e76" />

- Bấm Instal để tiến trình cài đặt
<img width="814" height="604" alt="image" src="https://github.com/user-attachments/assets/8a6b8571-06a7-4116-b95d-037a6bb920ba" />

- Hoàn tất qaus trình cài đặt
<img width="814" height="604" alt="image" src="https://github.com/user-attachments/assets/7be3ed26-eed4-458a-9a68-aae4d3d44ba5" />


##### Cài đặt công cụ quản lý SQL Server Management Studio 
SQL Server Management Tools là công cụ dành cho quản lý và tương tác với Microsoft SQL Server.
Những công cụ này cung cấp giao diện người dùng đồ họa để thực hiện nhiều thao tác vụ quản lý cơ sở dữ liệu, từ tạo cơ sở dữ liệu mới đến quản lý người dùng và sao lưu dữ liệu.

- quay lại file setup.ext và chạy nó lần nữa 
-  sau đó chọn Instal SQL Server Management Tools.4
-  Nó sẽ dẫn bạn tới 1 trang download SQL Server Management Studio.
<img width="814" height="604" alt="image" src="https://github.com/user-attachments/assets/39e2ed54-e0ce-4298-b3c0-6cf22f8b7dde" />

- bấm vào instal và đợi hệ thóng tự cài đặt SQL Server Management Studio
<img width="812" height="627" alt="image" src="https://github.com/user-attachments/assets/4f64e81c-b86f-48b2-a5a3-5d48ee6a833f" />

- Kiểm tra kết nối SQL Server
  Để kết nối tới Microsoft SQL Serve 2016, mở công cụ SQL Server Managament Studio
- Nhấn Connect 
  <img width="763" height="542" alt="image" src="https://github.com/user-attachments/assets/369a1d71-5286-47f7-af93-9f0867008ecd" />


<img width="882" height="402" alt="image" src="https://github.com/user-attachments/assets/4447b400-055f-4bb6-9dee-260080f10041" />


