# IAM Identity Center (Successor to AWS Single Sing-On - SSO)

- 1 cách để sử dụng enterprise identity lưu trữ với AWS
- cho phép quản lý tập trung truy cập SSO vào multiple AWS accounts và business applications bên ngoài
- Thay thế các trường hợp sử dụng lịch sử do SAML 2.0 cung cấp
- Flexible Identity store: where identities are stored. cho phép identities bên ngoài được swap với AWS credentials
- IAM Identity Center supports the following type of identity stores:
    - Built-in identity store
    - AWS Managed Microsoft AD
    - On-premise Microsoft AD (Two way trust or AD Connector)
    - External Identity Provider - SAML 2.0
- IAM Identity Center được AWS ưu tiên sử dụng cho bất kì "workforce" (enterprise) identity federation over the traditional SAML 2.0 based identity federation

## IAM Identity Center Architecture

![IAM Identity Center](images/AWSSSO.png)

- yêu cầu của IAM Identity Center là phải tạo một AWS Organization hợp lệ
