# STS

- Allows to assume roles across different accounts or same accounts
- tạo thông tin xác thực tạm thời (`sts:AssumeRole*`)
- Temporary credentials giống với access key. Chúng hết hạn và chúng không trực tiếp thuộc về identity khi assumes the role
- Temporary credentials thường cung cấp limited access
- Temporary credentials được request bởi identity khác (AWS or external - identity federation)
- Temporary credentials include the following:
    - `AccessKeyId`: unique ID of the credentials
    - `Expiration`: date and time of credential expiration
    - `SecretAccessKey`: used to sign the requests to AWS
    - `SessionToken`: unique token which must be passed with all the requests to AWS
- STS cho phép enable identity federation/liên kết

## Assume a Role with STS

1. xác định IAM role bên trong account or cross-account
2. xác định principals cso thể access vào IAM role
3. Use the AWS STS (Secure Token Service) để truy xuất vào IAM role mà có quyển access (`AssumeRole` API)
4. Temporary credentials can be valid between 15 minutes to 1 hour

## Revoke IAM Role Temporary Credentials

- **Trust policy**: specifies who can assume a role
- Roles can be assumed by many identities
- Everybody who assumes a role, gets the same set of permissions
- Temporary credentials không thể bị hủy, chung valid cho đến khi expire
- Temporary credentials can last for longer time
- trong trường hợp credential leak(rò rỉ) nếu thay đổi permissions cho policy, điều đó sẽ làm ảnh hưởng (legitimate: ảnh hưởng) tất cả users - không phải là good idea cho revoking access
- Solution: 
    - Revoke tất cảt sessions hiện có, bằng cách áp dụng `AWSRevokeOlderSessions` inline policy cho role. nó sẽ apply cho tất cả sessions hiện có, sessions đc tạo sau đó sẽ không bị ảnh hưởng
    - không thể revoke credentials một cách manual!
