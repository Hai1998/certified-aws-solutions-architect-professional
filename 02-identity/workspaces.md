# Amazon Workspaces

- Managed desktop as a service product (DAAS) - dedicated virtual Windows/Linux desktop delivered as a managed service
- lý tưởng cho home working
- tương tự như Citrix/Remote Desktop - nhưng đc host bên trong AWS
- Nó cung cấp 1 desktop nhất quán có thể truy cập từ mọi nơi, có thể log off và log back từ những nơi khác nhau, applications duy trì trạng thái của chúng
- có thể có Windows and Linux workspaces ở nhiều size khác nhau
- Có thể sử dụng commercial applications bên trong workspaces
- Workspaces có thể đc tính phí theo monthly or hourly basis. Có một khoản chi phí infrastructure bổ sung hàng tháng, được áp dụng ngay cả trong trường hợp thanh toán theo giờ
- Workspaces sử dụng Directory Service (Simple, AD, AD Connector) cho authentication và quản lý user
- Mỗi workspace sử dụng 1 ENI (Elastic Network Interface) được đưa vào 1 VPC
- Workspaces được truy cập bằng cách sử dụng client software từ desktop/laptop, chi phí bandwidth được bao gồm free. Đối với mọi truy cập Internet khác từ workspaces sẽ sử dụng VPC infrastructure thông thường và tính phí tương ứng
- Windows workspaces có thể truy cập cào FSx and EC2 windows resources
- bất kì hybrid network nào hiện có thể truy cập vào on-premise resources
- Workspaces cung cấp 1 system volume và user volume, cả 2 đều có thể encrypted

## WorkSpaces Application Manager (WAM)

- Triển khai và Quản lý các ứng dụng dưới dạng virtualized application containers.
- Cung cấp sacle và giữ cho các ứng dụng luôn được cập nhật bằng cách sử dụng WAM.

## Windows Updates

- Theo mặc định, Amazon WorkSpaces được cấu hình để install software updates.
- Amazon WorkSpaces chạy hệ điều hành Windows sẽ có tính năng Windows Update được bật.
- Bạn có toàn quyền kiểm soát Windows Update frequency.

## Maintenance Windows

- Các bản cập nhật được cài đặt trong maintenance windows (do bạn xác định).
- Always On WorkSpaces: mặc định từ 00h00 đến 04h00 vào sáng Chủ nhật.
- AutoStop WorkSpaces: tự động khởi động một lần mỗi tháng để cài đặt các bản cập nhật.
- Manual maintenance: bạn xác định các cửa sổ bảo trì và thực hiện bảo trì.

## IP Access Control Groups

- tương tự security groups cho Amazon WorkSpaces
- List of IP addresses / CIDR address ranges that users are authorized to connect from
- Nếu users access WorkSpaces qua VPN or NAT, IP Access Control Group must authorize the public IP of these

## Workspaces Architecture

![Workspaces Architecture](images/AmazonWorkspaces.png)

- Workspaces đưa 1 ENI vào customer managed VPCs, chúng chạy bên trong AWS managed VPCs
- Directory services cũng đưa ENIs vào customer managed VPCs cho file access, etc.
- Workspaces không có tính highly available bởi design
- có thể phân phối workspaces trong AZs khác nhau, nhưng 1 workspace trong trường hợp đặc biệt có thể bị lỗi khi AZ failure
