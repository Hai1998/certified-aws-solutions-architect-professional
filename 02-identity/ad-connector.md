# AD Connector (Directory Services)

- AD Connector cung cấp 1 cặp directory endpoints trong 1 VPC
- nó đưa các ENIs vào trong subnets của 1 VPC
- Khi đã được chèn, AD Connector xuất hiện như một danh mục gốc đối với các instance AWS khác có khả năng sử dụng dịch vụ danh mục.Khi được tích hợp, AD Connector xuất hiện như một thư mục gốc (native directory) đối với các phiên bản AWS khác có khả năng sử dụng directory services
- Chuyển hướng các yêu cầu đến một máy chủ thư mục hiện có tại cơ sở (on-premise), có nghĩa là không có dữ liệu thư mục nào được lưu trữ trong AWS.
- AD connector allows us to use AWS services which do require an AD directory (such as Workspaces) and use this with an on-premises directory service => we don't need to deploy additional AD directory in AWS
- There are 2 sizes of directory services *small* and *large*, while there are no explicit user limits, the chosen size does impact the amount of compute allocated by AWS for the connector
- We can use multiple connector to distribute the load
- AD directory is placed in 2 subnets in a VPCs in different availability zones => resilient to AZ failure
- The connector should be configured to point to at least one on-premise directory service => we need to provide account information for the connector to be able to authenticate itself
- Requires a working network to on-premise service, otherwise wont work (private network via Direct Connect or VPN)

## AD Connector Architecture

![AD Connector](images/DirectoryServiceADConnector.png)

## Use cases for AD Connector

- Prof of concept projects, we don't want to move our Active Directory to AWS for it
- We have a small infrastructure in AWS and we don't want to move the Active Directory to AWS
- Legal/compliance reasons - we don't want to store user info in AWS
- For larger requirements use AWS Directory Service
