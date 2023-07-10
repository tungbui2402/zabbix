# Zabbix
## I. Khái niệm
- Zabbix là một phần mềm giám sát hệ thống mã nguồn mở. Nó được sử dụng để theo dõi và kiểm soát hiệu suất, sự cố và các thông số quan trọng khác của các hệ thống máy tính, mạng và các ứng dụng khác.
- Zabbix cung cấp các tính năng mạnh mẽ để giám sát và phân tích dữ liệu từ các nguồn khác nhau, bao gồm các máy chủ, máy tính cá nhân, thiết bị mạng, ứng dụng web và các dịch vụ cloud. Nó có khả năng thu thập dữ liệu từ các nguồn khác nhau và hiển thị thông tin trong giao diện người dùng trực quan.
- Phần mềm Zabbix có thể giám sát các thông số như tải CPU, bộ nhớ, lưu lượng mạng, tình trạng hệ thống và các thành phần khác của hệ thống. Nó cũng có thể gửi cảnh báo khi các thông số vượt quá ngưỡng được xác định trước, giúp quản trị viên phát hiện và giải quyết các sự cố kịp thời.
- Zabbix được cài đặt dưới dạng máy chủ và có khả năng kết nối với nhiều đại lý (agents) để thu thập dữ liệu từ các hệ thống được giám sát. Nó cũng cung cấp các tính năng bổ sung như báo cáo, đồ thị và khả năng mở rộng để tương thích với môi trường quản lý hệ thống phức tạp.
- Zabbix là một giải pháp mạnh mẽ và linh hoạt cho việc giám sát hệ thống và mạng, và nó được sử dụng rộng rãi trong các môi trường doanh nghiệp và tổ chức khác nhau trên toàn thế giới.

## II. Cài đặt
### 1. Cài với apache
Đăng nhập bằng root: `sudo -s`
- B1: Thêm zabbix vào kho lưu trữ:
```
wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.0-4+ubuntu20.04_all.deb
dpkg -i zabbix-release_6.0-4+ubuntu20.04_all.deb
apt update 
```
- B2: Cài đặt Zabbix server, frontend, agent: `apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent`
- B3: Tạo database zabbix:
```
mysql -uroot -p
create database zabbix character set utf8mb4 collate utf8mb4_bin;
create user zabbix@localhost identified by 'password';
grant all privileges on zabbix.* to zabbix@localhost;
set global log_bin_trust_function_creators = 1;
quit;
```
- B4: Trên máy chủ Zabbix, máy chủ nhập tài khoản zabbix và dữ liệu ban đầu. Nhập mật khẩu mới tạo của zabbix: `zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix`
- B5: Tắt tùy chọn log_bin_trust_function_creators sau khi nhập giản đồ cơ sở dữ liệu:
```
mysql -uroot -p
set global log_bin_trust_function_creators = 0;
quit;
```
- B6: Cấu hình cơ sở dữ liệu cho máy chủ Zabbix:
```
nano /etc/zabbix/zabbix_server.conf
DBPassword=password
```
- B7: Khởi động hệ thống zabbix:
```
systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2
```
Url mặc định của Zabbix là `ip/zabbix`
### 2. Cài với nginx
Đăng nhập bằng root: `sudo -s`
- B1: Thêm zabbix vào kho lưu trữ:
```
wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.0-4+ubuntu20.04_all.deb
dpkg -i zabbix-release_6.0-4+ubuntu20.04_all.deb
apt update
```

- B2: Cài đặt Zabbix server, frontend, agent: `apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent`

- B3: Tạo database zabbix:
```
mysql -uroot -p
create database zabbix character set utf8mb4 collate utf8mb4_bin;
create user zabbix@localhost identified by 'password';
grant all privileges on zabbix.* to zabbix@localhost;
set global log_bin_trust_function_creators = 1;
quit;
```

- B4: Trên máy chủ Zabbix, máy chủ nhập tài khoản zabbix và dữ liệu ban đầu. Nhập mật khẩu mới tạo của zabbix: `zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix`

- B5: Tắt tùy chọn log_bin_trust_function_creators sau khi nhập giản đồ cơ sở dữ liệu:
```
mysql -uroot -p
set global log_bin_trust_function_creators = 0;
quit;
```

- B6: Cấu hình cơ sở dữ liệu cho máy chủ Zabbix:
```
nano /etc/zabbix/zabbix_server.conf
DBPassword=password
```

- B7: Cấu hình nginx cho máy chủ Zabbix:
```
nano /etc/zabbix/nginx.conf
listen 80;
server_name your_ip;
```

- B8: Khởi động hệ thống zabbix:
```
systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2
```
Url mặc định của Zabbix là `ip`
## III. Cấu hình zabbix
- Hệ điều hành: Zabbix có thể chạy trên nhiều hệ điều hành như Linux, Windows, FreeBSD, và một số hệ điều hành khác. Hãy chọn một hệ điều hành phù hợp với môi trường của bạn.
- Cấu hình phần cứng: Zabbix có yêu cầu về tài nguyên phần cứng tương đối. Bạn nên đảm bảo rằng máy chủ Zabbix có đủ tài nguyên RAM, CPU và dung lượng đĩa cứng để xử lý dữ liệu giám sát. Số lượng tài nguyên cụ thể phụ thuộc vào quy mô hệ thống của bạn.
- Cơ sở dữ liệu: Zabbix yêu cầu một cơ sở dữ liệu để lưu trữ thông tin giám sát. Bạn có thể sử dụng MySQL, PostgreSQL, hoặc Oracle để làm cơ sở dữ liệu cho Zabbix.
- Cài đặt Zabbix Server: Bạn cần cài đặt Zabbix Server trên máy chủ chính của mình. Đây là nơi xử lý và lưu trữ dữ liệu giám sát. Bạn có thể tìm hiểu hướng dẫn cài đặt Zabbix trên trang web chính thức của Zabbix.
- Cài đặt Zabbix Agent: Zabbix Agent là một phần mềm cần được cài đặt trên các máy chủ mà bạn muốn giám sát. Nó thu thập thông tin từ các máy chủ đó và gửi cho Zabbix Server. Cài đặt Zabbix Agent trên các máy chủ mục tiêu của bạn.
- Cấu hình giao diện người dùng: Sau khi cài đặt Zabbix Server, bạn có thể truy cập vào giao diện người dùng của Zabbix để cấu hình và tùy chỉnh hệ thống giám sát. Giao diện người dùng cho phép bạn tạo các host, giám sát các thông số, tạo đồ thị và báo cáo, và cấu hình các cảnh báo.

## IV. Zabbix Monitor
Chuẩn bị 3 máy với 1 máy là zabbix server và 2 máy là zabbix agent
### B1: Cài đặt
Với máy zabbix server thì cài như trên

Với máy zabbix client thì:
- B1: Thêm zabbix vào kho lưu trữ:
```
wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.0-4+ubuntu20.04_all.deb
dpkg -i zabbix-release_6.0-4+ubuntu20.04_all.deb
apt update
```

- B2: Cài đặt Zabbix agent: `apt install zabbix-agent`

- B3: Chỉnh sửa file cấu hình /etc/zabbix/zabbix_agentd.conf và cung cấp địa chỉ IP hoặc tên miền của Zabbix Server.
```
Server=ip_zabbix_server
ServerActive=ip_zabbix_server
Hostname=ubuntu-agent-1
```

- B4: Khởi động lại Zabbix Agent để áp dụng các thay đổi:
```
systemctl restart zabbix-agent
systemctl enable zabbix-agent
```

- B5: Tại Zabbix Server, vào phần Configuration, vào Hosts, chọn Create host. Nhập Hostname=ubuntu-agent-1, Groups chọn Linux servers, Interfaces chọn add rồi thêm ip của zabbix_server. Sau khi xong thì chọn add để thêm và đợi để máy lên là xong.

![Screenshot (82)](https://github.com/tungbui2402/zabbix/assets/129025623/95f9c8b9-5763-484b-b69f-fddc5374aae5)


## V. Zabbix alert
Telegram là một kênh nhận thông báo vô cùng tốt, nhanh chóng và hỗ trợ trên nhiều nền tảng bởi việc tạo và kết nối riêng qua API/Token..vv




Đầu tiên chúng ta cần đăng nhập zabbix bằng tài khoản admin

```
user: Admin
Password: zabbix
```

Tiếp đến  vào Administration -> Media types -> Telegram và nhập token và group id. Để lấy token và ip chat của nhóm telegram thì làm theo video sau: https://www.youtube.com/watch?v=JNwEJ5HvLgM&pp=ygUbdG9rZW4gdsOgIGlwIG5ow7NtIHRlbGVncmFt

Tiếp đến vào Configuration -> Action -> Trigger actions, chọn Report problems to Zabbix administrators, chọn Operations, chọn Add và add mỗi phần như dưới: 

Operations:
```
1	Send message to user groups: Zabbix administrators via all media
1	Send message to users: Admin (Zabbix Administrator) via Telegram
```
Recovery operations
```
Notify all involved	
Send message to users: Admin (Zabbix Administrator) via Telegram
Send message to user groups: Zabbix administrators via Telegram
```
Update operations
```
Send message to user groups: Zabbix administrators via Telegram
Send message to users: Admin (Zabbix Administrator) via Telegram
```
Lưu ý: Mỗi phần trên đều chọn Telegram ở phần Send only to, trừ Notify all involved.

Sau khi xong thì chọn Update để lưu thay đổi là xong.

### Test:
Tắt 1 host và refresh web lại nhiều lần cho đến khi hiện problems, sau đó zabbix sẽ gửi thông báo về telegram
