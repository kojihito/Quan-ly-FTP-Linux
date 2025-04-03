# Quản lý FTP server trên Linux

## Nội dụng 
- **I.Hướng dẫn chuẩn bị**
  - **1. Chuẩn bị các DNS server hoàn chỉnh**
  - **2. Cài đặt thư viện VSFTPD**
  - **3. Cài thư viện FTP**
- **II. Bắt đầu quản lý FTP**
  - **1. Khởi chạy FTP cơ bản**
  - **2. Khởi chạy FTP với user**
  - **3. Dùng máy client dăng nhập vào FTP server**
  - **4. Cài đặt Filezilla trên máy Client**
  - **5. Thực hành Filezilla trên máy Client**
 
# I. Hướng dẫn chuẩn bị
## 1. Chuẩn bị các DNS Server hoàn chỉnh
  - Kiểm tra các DNS cách ping đến tên máy chủ DNS trong terminal
  ![image](https://github.com/user-attachments/assets/96d3e031-a743-4b21-917c-efa4eb364098)
  
  - Để có trang 2 trang web mang tên miền là sgu.edu.vn và vnlab.net, DNS của 2 domain này đều phải trỏ về IP của máy web server là 192.168.1.1

## 2. Cài đặt thư viện VSFTPD

  *- vsftpd (Very Secure FTP Daemon) là một dịch vụ FTP (File Transfer Protocol) trên CentOS và các hệ điều hành Linux khác. Nó được thiết kế để cung cấp một phương thức truyền tải tệp tin qua mạng một cách an toàn, nhanh chóng và dễ quản lý.*
  - Câu lệnh:  ```yum install vsftpd -y```
  - Kiểm tra cài đặt thành công hay chưa bằng câu lệnh:
  ``` rpm -qa | grep vsftpd ```
![image](https://github.com/user-attachments/assets/a23b79f3-9f4d-499e-ac95-a7225a79fbe3)
## 3. Cài thư viện FTP
*- Trên CentOS, gói ftp chủ yếu được sử dụng để cung cấp khả năng truyền tải tệp tin giữa máy khách và máy chủ thông qua giao thức FTP (File Transfer Protocol). FTP là một giao thức mạng tiêu chuẩn cho phép truyền tải tệp tin một cách dễ dàng và nhanh chóng trong hệ thống mạng, dù là mạng nội bộ hay qua Internet.*
- Bước 1: cấu hình lại file CentOS-Base.repo
  ```gedit /etc/yum.repos.d/CentOS.Base.repo```
- Bước 2: thêm ```#``` vào đầu các dòng bắt đầu bằng ```mirrorlist=```
- Bước 3: Edit các dòng bắt đầu bằng ```baseurl=``` và cập nhật lại như sau:
  ```baseurl=http://vault.centos.org/7.9.2009/os/$basearch/``` và save file.
- Bước 4: Mở terminal gõ ```yum install ftp -y```
- Bước 5: Kiểm tra bằng lệnh ```rpm -qa | grep ftp```
![image](https://github.com/user-attachments/assets/0f1f0837-5622-4ff2-8194-77f5cb5a41ca)
# II. Bắt đầu quản lý FTP
## 1. Khởi chạy FTP cơ bản
- Bước 1: ta cần vào thư mục vsftpd.conf để kiểm tra xem đã thiết lập IPv4 và IPv6 chưa
  ``` gedit /etc/vsftpd/vsftpd.conf ```
![image](https://github.com/user-attachments/assets/599405ca-10f7-4d59-8bcd-25683c014f04)
- Cấu hình giá trị ngay chỗ “listen = Yes” và “listen_ipv6 = No”.


- file vsftpd.conf mặc định
```
anonymous_enable=YES
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
xferlog_enable=YES
connect_from_port_20=YES
xferlog_std_format=YES
listen=NO
listen_ipv6=YES
pam_service_name=vsftpd
userlist_enable=YES
tcp_wrappers=YES
```

- Bước 2: Bắt đầu khởi chạy ftp bằng lệnh:
```
systemctl start vsftpd
systemctl enable vsftpd
```

- Bước 3: Kiểm tra trạng thái:
``` systemctl status vsftpd ```
![image](https://github.com/user-attachments/assets/f4adbbdc-c302-437e-a123-1efdcdda3d13)

- Bước 4: Thử kết nối đến miền đã cấu hình bên phần cấu hình DNS:
``` ftp sgu.edu.vn ```
![image](https://github.com/user-attachments/assets/8ff2e47b-ad40-48e1-9cd2-d786298ae73e)

- Tại đây, bạn có thể đăng nhập với name mặc định là anonymous và mật khẩu là mật khẩu thiết lập lúc đăng nhập vào centOS trên máy.
![image](https://github.com/user-attachments/assets/a1505a78-0723-4bb8-925c-dbdf6524af85)

- Kiểm tra thư mục tồn tại bằng  lệnh ```ls```
![image](https://github.com/user-attachments/assets/df0ddb43-06ec-436a-9e25-a9986acb8d9b)
*Như bạn thấy, nó thông báo đang có một thư mục gốc tên là pub trong ftp.*

- Hoặc bạn có thể truy cập trình duyệt Mozilla FireFox trên CentOS 7 và nhập đường dẫn ```ftp://sgu.edu.vn```
![image](https://github.com/user-attachments/assets/3689d943-6795-4e56-afb4-35cb0f81adaf)
*Nếu thực hiện được kết quả ở bước 3 có nghĩa là khởi động thành công ftp.*
*Còn nếu không hãy kiểm tra lại phần cấu hình DNS xem đã thành công hay chưa. (xem lại phần kết quả cuối cùng cấu hình DNS khi thực hiện lệnh ```nslookup```)*

- Bước 5: Ban đầu thư mục pub là thư mục rỗng. Tiến hành tạo một file trong thư mục pub trên máy và kiểm tra lại.
- *Di chuyển vào thư mục pub của ftp:*
  `cd /var/ftp/pub`
  ![image](https://github.com/user-attachments/assets/89523cfe-3650-4360-aa38-6ee01d771b60)
- *Tiến hành tạo một file hello.txt với nội dung tùy chỉnh:*
  `gedit hello.txt`
  ![image](https://github.com/user-attachments/assets/df758fc4-6e3d-467d-9073-640f5d50ea36)

- *Giờ ta vào Mozilla FireFox để kiểm tra lại bằng cách nhập đường dẫn “ftp://sgu.edu.vn” và click vào thư mục pub. Ta sẽ thấy file hello.txt mà ta vừa tạo trên máy.*
![image](https://github.com/user-attachments/assets/fb457b50-23c3-4549-804c-189bb41c8593)

*Ấn vào file hello.txt, ta có thể thấy thông điệp có trong file:*
![image](https://github.com/user-attachments/assets/f0fec188-c3b7-4532-972d-3e1535484639)

*Và ta có thể tải file đó ngược lại về máy bằng các click chuột phải chọn “Save Page As” và chọn mục cần lưu file tải xuống:*
![image](https://github.com/user-attachments/assets/98a8c67d-d9db-4cbd-8474-75b8fc7b2d37)

*Kiểm tra tệp trong thư mục Downloads*
![image](https://github.com/user-attachments/assets/3a52d231-1e52-4f41-9ac3-be8ba49a382f)

## 2. Khởi chạy FTP với user
*Do mặc định ban đầu, ftp khởi chạy với user anonymous. Giờ ta muốn ftp ngắt user anonymous và sử dụng các user do ta thiết lập thì thực hiện các bước như sau:*
- Bước 1: Thiết lập cấu hình trong file vsftpd.conf
` gedit /etc/vsftpd/vsftpd.conf `
![image](https://github.com/user-attachments/assets/e62cc7b3-7800-4aec-8c34-ecfc43b04204)
![image](https://github.com/user-attachments/assets/1705f880-cd4d-4e2d-9cb0-7dd12a991838)
![image](https://github.com/user-attachments/assets/8eb5333b-961a-49f2-990a-29884a1d116e)
![image](https://github.com/user-attachments/assets/23c08f39-a59c-468a-b81a-03771efe3b33)
![image](https://github.com/user-attachments/assets/f193024c-7892-4b86-adb1-c3a00da00ed4)

*Thêm các nội dung bên dưới ở cuối file:*
![image](https://github.com/user-attachments/assets/51f96aab-971a-4241-b58b-df848315f131)

*Hoặc copy nội dung bên dưới vào file vsftpd.conf*
```
anonymous_enable=NO
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
xferlog_enable=YES
connect_from_port_20=YES
xferlog_std_format=YES

chroot_local_user=YES
chroot_list_enable=YES
allow_writeable_chroot=YES
chroot_list_file=/etc/vsftpd/chroot_list
listen=YES
listen_ipv6=NO

pam_service_name=vsftpd
userlist_enable=YES
tcp_wrappers=YES

userlist_deny=NO
userlist_file=/etc/vsftpd/user_list
local_root=/var/ftp/
```

Với:
- `anonymous_enable=NO` : không cho phép người dùng ẩn danh (anonymous) đăng nhập vào máy chủ FTP. Điều này làm tăng tính bảo mật bằng cách yêu cầu tất cả người dùng phải có tài khoản hợp lệ.
- `local_enable=YES` : cho phép người dùng địa phương (local users) đăng nhập vào máy chủ FTP. Những người dùng này phải có tài khoản trên hệ thống.
- `write_enable=YES` : cho phép người dùng thực hiện các thao tác ghi, như tải lên, xoá và thay đổi tệp tin trên máy chủ FTP. Nếu NO, tất cả các thao tác ghi sẽ bị chặn.
- `local_umask=022` : đặt umask mặc định cho người dùng địa phương khi họ tải lên tệp. 022 sẽ thiết lập quyền mặc định cho tệp là 755, tức là chủ sở hữu có tất cả quyền, trong khi nhóm và người khác chỉ có quyền đọc và thực thi.
- `dirmessage_enable=YES` : hiển thị thông báo thư mục khi người dùng vào một thư mục cụ thể. Thông báo này có thể được thiết lập trong tệp .message trong thư mục đó.
- `xferlog_enable=YES` : kích hoạt ghi lại nhật ký các thao tác truyền tải tệp (như tải lên và tải xuống). Tất cả hoạt động sẽ được ghi vào tệp xferlog.
- `connect_from_port_20=YES` : đảm bảo rằng kết nối dữ liệu (data connection) sẽ sử dụng cổng 20, một yêu cầu tiêu chuẩn của FTP.
- `xferlog_std_format=YES` : định dạng nhật ký theo chuẩn xferlog để dễ dàng phân tích và tương thích với các công cụ xử lý nhật ký FTP.
- `chroot_local_user=YES` : chroot người dùng vào thư mục cá nhân của họ, ngăn không cho họ truy cập vào các thư mục hệ thống bên ngoài. Điều này tạo một lớp bảo mật bổ sung.
- `chroot_list_enable=YES` : kích hoạt danh sách chroot_list_file, nơi bạn có thể chỉ định người dùng ngoại lệ không bị giới hạn bởi chroot.
- `allow_writeable_chroot=YES` : cho phép các thư mục gốc (chroot) có quyền ghi, điều này sẽ tránh lỗi khi người dùng có quyền ghi vào thư mục của họ nhưng chroot_local_user=YES được kích hoạt.
- `chroot_list_file=/etc/vsftpd/chroot_list` : chỉ định đường dẫn tệp chứa danh sách các người dùng ngoại lệ không bị chroot.
- `listen=YES` : kích hoạt máy chủ FTP chạy ở chế độ độc lập, lắng nghe kết nối trên các socket IPv4.
- `listen_ipv6=NO` : vô hiệu hóa việc lắng nghe kết nối trên các socket IPv6.
- `pam_service_name=vsftpd` : chỉ định tên dịch vụ PAM (Pluggable Authentication Modules) cho VSFTPD, sử dụng các thiết lập bảo mật xác thực trong /etc/pam.d/vsftpd.
- `userlist_enable=YES` : kích hoạt tệp user_list để kiểm soát truy cập người dùng, cho phép chỉ những người dùng trong danh sách được phép truy cập.
- `tcp_wrappers=YES` : kích hoạt hỗ trợ TCP Wrappers để kiểm soát truy cập mạng thông qua các tệp /etc/hosts.allow và /etc/hosts.deny.
- `userlist_deny=NO` : khi userlist_enable=YES và userlist_deny=NO, chỉ những người dùng trong user_list mới được phép đăng nhập.
- `userlist_file=/etc/vsftpd/user_list` : chỉ định đường dẫn tệp chứa danh sách người dùng được phép truy cập FTP. Kết hợp với các tùy chọn ở trên, chỉ người dùng trong tệp này được phép truy cập.
- `local_root=/var/ftp/` : chỉ định thư mục gốc cho tất cả người dùng FTP trên hệ thống.

- Bước 2: Tiến hành khởi chạy lại vsftpd bằng lệnh:
- `systemctl restart vsftpd`

- Bước 3: Tạo các user trong ftp:
- Tạo các user trong ftp bằng lệnh
```
useradd -d /var/ftp/user1  user1
useradd -d /var/ftp/user2  user2
```
- Tạo mật khẩu:
```
passwd user1
passwd user2
```
![image](https://github.com/user-attachments/assets/ddaa7465-d766-494e-b8fb-ce50417524dc)

- *Thông tin user1 và user2 trong ví dụ này lần lượt là:*
```
Name: user1 - Pass: 0823072871
Name: user2 - Pass: 123456
```

- Bước 4: Tạo nhóm quyền và phân quyền cho 2 user vừa tạo

*Tiến hành tạo 2 nhóm quyền bằng lệnh sau*
`groupadd ftp_basic` 	(nhóm quyền này sẽ có 2 quyền cơ bản đọc và ghi)
`groupadd ftp_onlyread` (nhóm quyền này sẽ chỉ có quyền đọc)

- Gán user1 vào nhóm quyền ftp_basic
`usermod -g ftp_basic user1`  (user1 sẽ có các quyền của nhóm ftp_basic)

- Gán user2 vào nhóm quyền ftp_onlyread
`usermod -g ftp_onlyread user2` (user2 sẽ chỉ có quyền đọc)
![image](https://github.com/user-attachments/assets/56e00558-7e79-4642-b725-b0f456afe5da)

- Bước 5: Thêm 2 user vào danh sách các user muốn truy cập vào ftp

*bằng cách truy cập vào file user_list:*
`edit /etc/vsftpd/user_list`

*Khi đã vào được file danh sách user_list của ftp, tiến hành thêm 2 user1 và user2 vào cuối danh sách:*
![image](https://github.com/user-attachments/assets/5e6b8ac6-d06b-4c99-affc-ac799e5d792a)

- Bước 6: Thêm 2 user vào file chroot.
*File `/etc/vsftpd/chroot_list` được sử dụng để quản lý danh sách các user FTP và việc chroot của họ. Chroot là cơ chế giới hạn user trong một thư mục cụ thể, không cho phép họ truy cập ra ngoài thư mục đó.*

*Do ta đã cấu hình trong file vsftpd như bên dưới:*
![image](https://github.com/user-attachments/assets/210bc4c8-d935-4033-b925-716ba7e65101)
*Nên tất cả user sẽ bị chroot, trừ những user có trong file /etc/vsftpd/chroot_list*

*Nên giờ ta sẽ thêm user1 và user2 vào file /etc/vsftpd/chroot_list để 2 user có toàn quyền truy cập.*

`gedit /etc/vsftpd/chroot_list`
*Thêm 2 user1 và user2 vào:*
![image](https://github.com/user-attachments/assets/d0edacae-8e7f-49c4-b390-d3f834e38828)

Bước 7: Thay đổi quyền sở hữu của 2 thư mục var/ftp/user1 và var/ftp/user2.
`chown -R user1:ftp_basic /var/ftp/user1`
- user1:ftp_basic: Thay đổi chủ sở hữu của thư mục /var/ftp/user1 thành người dùng user1 và nhóm ftp_basic.
- gán chủ sở hữu của thư mục user1 là tài khoản user1
- -R: Thực hiện đệ quy, tức là áp dụng thay đổi cho tất cả các tệp và thư mục con bên trong /var/ftp/user1.

`chown -R user2:ftp_onlyread /var/ftp/user2`
- user2:ftp_onlyread: Thay đổi chủ sở hữu của thư mục /var/ftp/user2 thành người dùng user2 và nhóm ftp_onlyread.
- -R: Áp dụng đệ quy cho tất cả các tệp và thư mục con bên trong /var/ftp/user2.
![image](https://github.com/user-attachments/assets/3e82c81b-66d9-4fbb-8122-a45a966f62b4)

- Bước 8: Thay đổi quyền truy cập của thư mục /var/ftp/user1 và /var/ftp/user2
*Lệnh chmod được sử dụng để thay đổi quyền truy cập của tệp hoặc thư mục. Trong lệnh này, các quyền được biểu diễn dưới dạng số*
`chmod 705 /var/ftp/user1`  (Cho phép chủ sở hữu có toàn quyền, người khác chỉ có thể đọc và thực thi)
`chmod 550 /var/ftp/user2`  (Chủ sở hữu và nhóm có quyền đọc và thực thi, người khác không có quyền truy cập.)
![image](https://github.com/user-attachments/assets/cbe847c7-e89c-4cbd-bec6-27ce4276eeac)

- Bước 9: Khởi động lại vsftpd
`systemctl restart vsftpd`

- Bước 10: Tiến hành kiểm tra kết quả
*Vào Mozilla FireFox, truy cập đường dẫn `ftp://sgu.edu.vn/` Nó sẽ bắt ta nhập username và password.*
![image](https://github.com/user-attachments/assets/a3b1429d-1d10-4745-a42c-70046c825659)

Tại đây ta tiến hành nhập thông tin user1 như ta đã thiết lập bên trên
```
User Name: user1
Password: 0823072871
```

*Nếu truy cập thành công, giao diện sẽ có dạng như bên dưới:*
![image](https://github.com/user-attachments/assets/098384e1-3cab-4e99-bcf6-990b8cbdbb71)

*Ta có thể truy cập các thư mục trong ftp bằng lệnh `cd /var/ftp`*

- Truy cập thư mục user1 và user2, ta sẽ thấy nó đang rỗng. Lần lượt tạo 2 file user1_hello.txt đặt trong thư mục user1 và hello_user2.txt nằm trong thư mục user2

- * Tạo file user1_hello.txt trong thư mục user1*
![image](https://github.com/user-attachments/assets/6a5346fd-9862-4003-babb-10aeb523acb6)

- *Tạo file user2_hello.txt trong thư mục user2*
![image](https://github.com/user-attachments/assets/1fa5b1e3-d289-45ea-8271-55e3cd8f736e)

- Truy cập lại “ftp://sgu.edu.vn/” và đăng nhập user2 vào để kiểm tra.
![image](https://github.com/user-attachments/assets/61cfec59-161c-4b8f-951a-8ba40ff2930c)
![image](https://github.com/user-attachments/assets/8da9f1e4-b5e9-4eb2-9a84-5ffdbbfb926f)

## 3. Dùng máy client dăng nhập vào FTP server
### 3.1. Dùng máy client để tải file 
- Bước 1: Cấu hình IP tĩnh bên CentOS Client. Ta tiến hành thiết lập IP trên máy CentOS Client để máy có thể phân giải được tên miền “sgu.edu.vn”:
![image](https://github.com/user-attachments/assets/5451536c-2c28-4d4a-9e3f-9de55b7057d9)

*Lưu ý: Phần DNS thiết lập trỏ đến IP của máy Server.*
Kiểm tra lại trong thư mục resolv.conf bằng lệnh `gedit /etc/resolv.conf`
![image](https://github.com/user-attachments/assets/3885d650-9906-4396-885b-b958ebd4bdde)


- Sau khi thiết lập xong, ta tiến hành dùng lệnh nslookup xem máy Client có phân giải được tên miền sgu.edu.vn chưa. Lưu ý: trên máy Client cũng phải cài gói BIND mới có thể dùng được lệnh nslookup CentOS Client. 

![image](https://github.com/user-attachments/assets/738d0679-b44b-426c-8884-9ae0c1a185b6)
*Nếu kết quả của máy Client được kết quả như trên thì đã thành công.*

- Bước 2: Trên máy Client, ta mở Mozilla FireFox và truy cập vào đường dẫn `ftp://sgu.edu.vn`, lúc này nó sẽ yêu cầu ta đăng nhập:
![image](https://github.com/user-attachments/assets/226b8be7-f417-47a3-b0eb-0623cfb0daa9)

Ta tiến hành đăng nhập bằng tài khoản user2 như đã thiết lập bên máy Server: 
```
User Name: user2
Passowrd: 123456
```
![image](https://github.com/user-attachments/assets/d5f3eccc-1b10-455d-a08f-547748c476d5)

*Ta có thể tải các file nằm trong thư mục user1 và user2 về máy của Client:*
![image](https://github.com/user-attachments/assets/26e08bb9-53ee-48a4-aa18-629023be6006)

### 3.2. Dùng máy client để thêm file bằng "lftp"
*`lftp` là một công cụ dòng lệnh mạnh mẽ dùng để truyền tải tệp qua mạng với các giao thức như FTP (File Transfer Protocol), FTPS (FTP Secure), SFTP (SSH File Transfer Protocol), HTTP, HTTPS, và một số giao thức khác. Nó có thể được sử dụng để kết nối đến các máy chủ từ xa và thực hiện các thao tác trên tệp như tải lên, tải xuống, duyệt thư mục, và đồng bộ thư mục.*

- *Để có thể tải file, cần thực hiện setup ljai máy con với các cấu hình chỉnh sửa file CentOS-Base.repo cũng như cài lại các gói Bind như đã làm với server.*

- Sau khi đã xong, tiến hành đăng nhập FTP server trên máy client bằng tài khoản `user` bằng lệnh sau:
`lftp -u user1 sgu.edu.vn` 
*Và tiến hành nhập password như đã thiết lập trên máy Server. Ta sẽ được kết quả như bên dưới:*
![image](https://github.com/user-attachments/assets/08097c72-618c-48ff-86c4-d26d1fcf0893)

*Và giờ ta sẽ tiến hành đẩy một file từ máy Client lên FTP Server.*
- Bước 1: Tạo một file client.txt trong máy Client. 
![image](https://github.com/user-attachments/assets/f7153cb7-ac16-461d-996b-a28e6df0fa07)
![image](https://github.com/user-attachments/assets/9df125c2-a1a2-4a61-b5b4-d55585254672)

- Bước 2: Giờ ta tiến hành truy cập FTP bằng lftp:
`lftp -u user1 sgu.edu.vn` và dùng lệnh `ls` để kiểm tra các file có trên FTP server
![image](https://github.com/user-attachments/assets/f9493a5a-d296-4450-b0aa-2549051f7bec)

Giờ ta dùng lệnh `!ls` để kiểm tra các file có trên máy Client
![image](https://github.com/user-attachments/assets/ffb56451-96e9-474c-801e-74ec49430e50)

- Ta sẽ thấy có 3 file, trong đó:
  - File “client.txt” ta vừa mới tạo bên trên.
  - File “user1_hello.txt” là file ta vừa mới tải từ FTP Server về. (trong phần “DÙNG MỘT MÁY CENTOS CLIENT ĐỂ ĐĂNG NHẬP VÀO FTP BÊN MÁY CENTOS SERVER VÀ TẢI FILE”)

- Bước 3 (quan trọng): tắt SELinux cả 2 phía Server và Client, vì nó có thể cản trở quyền put file của client.
*SELinux (Security-Enhanced Linux) là một mô-đun bảo mật được tích hợp vào nhân Linux, được phát triển bởi NSA (Cơ quan An ninh Quốc gia Hoa Kỳ). Nó cung cấp một lớp bảo mật bổ sung bên cạnh các cơ chế bảo mật truyền thống của Linux.*

  - Câu lệnh tắt SELinux `sudo setenforce 0`

-Bước 4: Đẩy file “client.txt” từ Client lên FTP Server.
Ta tiến hành đi đến thư mục user1 `cd user1`
![image](https://github.com/user-attachments/assets/bacb8fb3-336b-4433-b801-bdcb0ab830d6)

  - Như ta thấy bên trên, khi dùng lệnh ls để kiểm tra các file có trong thư mục user1 bao gồm file user1_hello.txt được tải lên từ phía Server.

- Giờ ta tiến hành đẩy file client.txt lên bằng lệnh `put -a client.txt`
![image](https://github.com/user-attachments/assets/25463ae6-a9bf-4b94-bdbf-072090cd9e8f)

- Sau khi put file lên, ta tiến hành kiểm tra lại bằng lệnh ls, lúc này trong thư mục user1 sẽ có thêm một file client.txt do phía client vừa đẩy lên.

- Không chỉ đẩy file lên, ta có thể tạo một folder trong thư mục user1 bằng lệnh `mkdir client_folder`
![image](https://github.com/user-attachments/assets/5e7dc910-8acf-4c30-b91f-35cd6c2a313d)
- Ngoài ra, ta cũng có thể xóa một file ra khỏi user1 bằng lệnh `rm <tên_file>`

- Bước 5: Kiểm tra trên máy Server xem có thấy thư mục và tệp mà Client đưa lên không.
*Mở Mozilla FireFox và truy cập đường dẫn “ftp://sgu.edu.vn/” và đăng nhập user1 vào.*

![image](https://github.com/user-attachments/assets/d445ecc2-1532-40f0-b4e1-e8229c7db017)
*Nếu kết quả như trên thì bạn đã thành công*

## 4. Cài đặt Filezilla trên máy client
*Ngoài lftp ra thì chúng ta có thể dùng filezilla với giao diện thân thiện hơn công cụ dòng lệnh lftp*

Các bước để cài đặt Filezilla
- Bước 1: Kiểm tra và cập nhật hệ thống
Trước tiên, hãy đảm bảo hệ thống của bạn được cập nhật bằng cách chạy `sudo yum update -y`

- Bước 2: Cài đặt EPEL Repository
Vì FileZilla không có sẵn trong kho chính của CentOS 7, bạn cần cài đặt EPEL Repository để truy cập nhiều gói phần mềm hơn bằng câu lệnh:
`sudo yum install epel-release -y`

Hoặc 
`yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm -y`

- Bước 3: Cài đặt Filezilla
`sudo yum install filezilla -y`

- Bước 4: Khởi động lại D-Bus
`sudo systemctl restart dbus`

- Bước 5: Mở filezilla
![image](https://github.com/user-attachments/assets/17685609-1099-4d36-8835-fee0d700354f)

## 5. Thực hành Filezilla trên máy client
Sau khi đã cài đặt thành công, ta tiến hành kết nối đến FTP Server
- Bước 1: Mở ứng dụng filezilla bằng dòng lệnh `filezilla` trong Terminal

 ![image](https://github.com/user-attachments/assets/57996e4e-531d-418a-9ed1-391ce6d028d0)

- Bước 2: Ta tiến hành nhập thông tin của phần host, Username và Password, giá trị phần port không điền để nó sử dụng mặc định
Thông tin cần điền:
host: `sgu.edu.vn`
Username: `user1`
Password: `0823072871`

*Sau khi điền xong thông tin, ta nhấn nút “Quickconnect” để bắt đầu kết nối.*

![image](https://github.com/user-attachments/assets/d80227aa-72c1-4f8e-a570-5bf8002244bb)
*Kết quả như hình là đã kết nối thành công. Có thể thực hiện upload và save file như bình thường.*


