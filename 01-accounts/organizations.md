# AWS Organizations

- Standard AWS account: là account không thuộc AWS Organization
- có thể tạo AWS Organization từ một standard AWS account
- organization không được tạo trong account này, chỉ sử dụng để tạo organization. Standard account sao đó trở thành **Management Account** (được gọi là *Master Account*)
- Sử dụng Management Account có thể mời accounts khác vào organization
- khi một standard account joins vào 1 organization, nó thay đổi thành **Member Account** của organization đó
- Organizations có 1 Management Account và 0 or nhiều Member Accounts
- có thể tạo cấu trúc của AWS accounts trong 1 organization. có thể group accountsbởi những thứ như business units, function or development stage, etc.
- Cấu trúc này có tính phân cấp, nó là inverted tree
- At the top of this tree is the root container of the organization (just a container within the organization, NOT to be confused with the root user)
- This root container can contain other containers, this containers are known as **Organizational Units (OU)**
- OUs có thể chứa accounts (Management/Member accounts) or OUs khác

## Consolidated Billing

- là một tính năng quan trong của AWS Organizations
- Phương thức thanh toán riêng của từng account từ organization bị xóa, member accounts chuyển tthanh toán qua Management Account (**Payer Account**)
- sử dụng consolidated billing(thanh toán tổng hợp) nhận 1 bill hàng tháng. Điều này bao gồm Management Account và tất cả Member Accounts của Organization
- khi sử dụng cả organization reservation benefits and discounts được gộp lại, organization có thể được hưởng lợi tổng thể từ việc chi tiêu của từng AWS account bên trong org

## Switch Role
- "Switch Role" (chuyển vai trò) là một tính năng cho phép chuyển đổi giữa các vai trò (roles) khác nhau trong cùng một tài khoản AWS hoặc giữa các tài khoản AWS khác nhau.
- Điều này rất hữu ích khi cần thực hiện các tác vụ khác nhau hoặc truy cập vào các tài nguyên khác nhau mà yêu cầu quyền truy cập khác nhau.
  
## Best Practices

- có 1 account duy nhất mà users có thể log into and assume IAM roles để truy cập vào các accounts khác từ org
- account có tất cả identities như là Management Account or nó có thể Member Account (*Login Account*)

## `OrganizationAccountAccessRole`

- là một IAM role được sử dụng để added/created account vào trong 1 organization
- role này sẽ được tạo tự động nếu tạo account từ organization có sẵn
- role được tạo thủ công trong member account nếu account được mời vào trong organization

# Service Control Policies (SCP)

- là 1 tính năng của AWS Organizations được sử dụng để restrict AWS accounts
- là JSON documents
- có thể được attach cho root của organization, từ 1 hoặc nhiều OUs or cho chính AWS accounts
- SCPs đc ké thừa thông qua organization tree
- Management Account đặc biệt: even if nếu có SCPs attached (trực tiếp or thông qua 1 OU) nó không bị ảnh hưởng bởi SCP
- SCPs là ranh giới cho phép account:
    - giới hạn account (including the root user of the account) có thể làm
    - có thể không bao giờ restrict 1 root user từ 1 account, nhưng có thể restrict chính account đó, dó đó những restrictions cũng sẽ apply cho root user
- **SCPs don't grant any permissions!** nó chỉ là ranh giới(boundary) để giới hạn những gì không được phép trong 1 account
- SCPs có thể đc sừ dụng theo 2 cách:
    - Deny list (default): allow by default and block access vào 1 số services nhất định
        - `FullAWSAccess`: policy đc apply theo default cho org và tất cả OUs khi enable SCPs. policy có nghĩa là theo default không có gì bị restricted
        - SCPs không grant permissions, nhưng khi enabled, sẽ có default deny cho mọi thứ. This is why the `FullAWSAccess` policy is needed
        - SCP priority rules:
            1. Explicit Deny
            2. Allow
            3. Default (implicit) deny
        - Benefits of deny lists is that as AWS mở rộng list các service cung cấp, services mới sẽ available cho accounts (low admin overhead)
    - Allow list: block by default and allow cho 1 số services nhất định
        - To implement allow lists:
            1. Remove the `FullAWSAccess` policy
            2. Add một vài services được allowed vào trong 1 new policy
        - Allow lists are more secure, but chúng require nhiều chi phí quản trị hơn 
