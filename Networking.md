## What is VPC?
- Viết tắt của **Virtual private cloud**
- Cho phép người dùng tạo 1 mạng ảo và control toàn bộ network in/out của mạng đó
- VPC tương đối giống với network ở data center truyền thống tuy nhiên các khái niệm đã được AWS đơn giản hoá

## Các thành phần cơ bản của VPC
- **VPC**: Một mạng ảo được tạo ra ở cấp độ region
- **Subnet**: Một dải IP được định nghĩa nằm trong VPC. Mỗi subnet phải được quyết định AZ tại thời điểm tạo ra
- **IP Address**: IP v4 hoặc V6 được cấp phát. Có 2 loại là Public IP và Private IP
- **Routing**: xác định traffic sẽ được điều hướng đi đâu trong mạng
- **Elastic IP**: IP được cấp phát riêng, có thể access từ internet (public), không bị thu hồi khi instance start -> stop
- **Security Group**: Đóng vai trò như một firewall ở cấp độ instance, định nghĩa traffic được đi vào /đi ra
- **Network Access Controll List (ACL)**: được apply ở cấp độ subnet, tương tự như security group nhưng có rule Deny và các rule được đánh độ ưu tiên
- **VPC Flow Log**: capture các thông tin di chuyển của traffic trong network
- **VPN Connection**: kết nối VPC trên AWS với hệ thống dưới On-premise
- **Elastic Network Interface**: đóng vai trò như 1 card mạng ảo
- **Internet Gateway**: Kết nối VPC với Internet, là cổng vào từ internet tới các thành phần trong VPC
- **NAT Gateway**: dịch vụ NAT của AWS cho phép các thành phần bên trong kết nối tới internet nhưng không cho bên ngoài kết nối tới
- **VPC Endpoint**: kênh kết nối private giúp kết nối tới các services khác của AWS mà không thông qua internet
- **Peering connection**: kênh kết nối giữa 2 VPC

## Làm sao để định nghĩa 1 VPC?
- VPC được định nghĩa bằng 1 dải CIDR
- AWS recommend chọn 1 trong 3 dải CIDR sau (theo chuẩn RFC-1918)
    - 192.168.0.0 – 192.168.255.255
    - 10.0.0.0 – 10.255.255.255
    - 172.16.0.0 – 172.31.255.255
- Việc định nghĩa CIDR của IP cần tuân thủ một số tiêu chí sau:
    - Cover được số lượng IP private cần cấp phát trong tương lai
    - Tránh overlap với các hệ thống sẵn có (kể cả on-premise) nếu không sẽ không thể peering

## Phân chia subnet ntn?
- Subnet được coi như một thành phần con của VPC
- Một VPC có thể chứa nhiều subnet không overlap nhau
- Khi tạo subnet phải chọn Availability Zone
- Chọn CIDR cho subnet cần lưu ý:
    - Số lượng IP cho các resource cần cấp phát
    - Số lượng subnet dự tính sẽ tạo trong tương lai

## Lab 1: Thiết kế VPC đơn giản
### Thiết kế VPC:
- VPC CIDR: 10.0.0.0/16
- Có 2 loại subnet public và private, mỗi subnet chứa ít nhất 1000 IP
- Mỗi loại suubnet nằm ở ít nhất 2 AZ
- Có 1 Internet Gateway, cấu hình route table tới Internet Gateway
- Có 1 NAT Gateway, cấu hình route table tới NAT Gateway
- Thiết kế VPC Endpoint cho S3 service
    <img src="/images/a1.png" alt="" width="800" height="400">
### Thiết kế security group cho 4 nhóm đối tượng:
- ALB: port 443 HTTPS
- App server cho phết port: 80 từ ALB. 22 từ Bastion server
- Database Server sử dụng MySQL port: 3306
- Bastion Server: SSH port 22 từ IP công ty
    <img src="/images/a2.png" alt="" width="800" height="500">

## Lab 2: ...










