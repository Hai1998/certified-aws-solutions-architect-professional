# AWS Resource Access Manager - RAM

- Cho phép sharing resources giữa các AWS accounts
- 1 số services có thể cho phép sharing giưã mọi AWS accounts, một vài sharing chỉ giữa accounts từ cùng 1 organization
- Services cần hỗ trợ RAM để có thể shared đc (not everything can be shared)
- Services can be shared with principals: accounts, OU's and ORG
- Shared resources can be accessed natively (nguyên bản)
- không mất chi phí khi sử dụng RAM, chỉ mất cost mà service được apply
- AWS RAM cho share resources trong 1 organization có thể đc enable với `enable-sharing-with-aws-organizations` CLI command. hoạt động này cần tạo 1 service-linked role đc gọi là `AWSServiceRoleForResourceAccessManager` để có policy do IAM quản lý có tên là `AWSResourceAccessManagerServiceRolePolicy` đc attach. role này cho phép RAM lấy thông tin về organization và structure của nó. điều này cho phép share resources với tất cả accounts

## Availability Zone IDs

- A region in AWS has multiple availability zones, example: `us-east-1a`, `us-east-1b`, etc.
- AWS rotates the name của AZs tuỳ theo AWS account, nghĩa là `us-east-1a` có thể không giống AZ nếu so sánh giữa 2 accounts
- nếu sảy ra lỗi ở mức hardware level, two accounts có thể thấy issue ở các AZ khác nhau, điều này có thể gây ra challenge trong troubleshooting
- AWS cung cấp AZ IDs để overcome challenge này. Example of IDs: `use1-az1`, `use1-az2`
- AZ IDs là duy nhất trên multiple accounts

## RAM Concepts

- **Owner account**: 
    - Owns the resource, creates a share, provides the name
    - giữ lại full permission đối với resource shared
    - chỉ định principal (AWS account, OU, entire AWS organization) with whom the share a specific resource
- **Principle**:
    - có thể là 1 AWS account, OU, toàn bộ AWS organization
    - Resources are shared with a principle(nguyên tắc)
- Nếu participant(ng tham gia) bên trong 1 ORG với sharing enabled, sharing automatically đc accept
- For non ORG accounts, or sharing with AWS Organizations is not enabled, phải accept 1 invite

## Shared Services VPC

- nó là 1 VPC được cung cấp infrastructure có thể đc sử dụng bởi services khác
- trong AWS điểu này được thiết kế theo architected truyền thống bằng cách sử dụng separate networks kết nối sử dụng VPC peering or Transit Gateways. Với AWS RAM và AWS Organizations we can create something which is more effective:
    ![Shared Services VPC](images/RAM.png)
- VPC owner có thể tạo và quản lý VPC and subnets được share với participants
- Participants có thể cung cấp services vào trong shared subnets, có thể read network objects được tham chiếu nhưng không thể modify or delete the subnets
- Resources đc tạo bởi 1 participant account sẽ không hiển thị cho participants khác or bởi VPC owner account
- Resources đc tạo bởi 1 participant account có thể đc truy cập từ resources khác đc tạo bởi participant accounts khác bởi vì chúng đang trên cùng 1 network
