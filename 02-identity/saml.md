# SAML 2.0 Identity Federation

- Federation cho phép user bên ngoài AWS to assume 1 temporary role để access AWS resources
- These users assume identity provider access role
- SAML 2.0 - Security Assertion Markup Language
- SAML 2.0 cho phép **indirectly/gián tiếp** sử dụng on-premise identities với AWS (console and CLI)
- SAML 2.0 dựa trên identity federation được sử dụng khi có enterprise dựa trên identity provider khi tương thích vơí SAML 2.0
- SAML 2.0 dựa trên federation lí tưởng khi có sẵn 1 identity management team quản lý quyền truy cập vào các services khác bao gồm AWS
- nếu đang tìm cách maintain 1 source thực sự duy nhất and/or có nhiều hơn 5000 users, SAML 2.0 dựa trên federation được đề xuất sử dụng
- Federation sử dụng IAM Roles and AWS Temporary Credentials (12 hours validity)

## SAML 2.0 Identity Federation Authentication Process - API Access

![SAML 2.0 Federation API](images/SAML2.0FederationAPI.png)

## SAML 2.0 Identity Federation Authentication Process - AWS Console Access

![SAML 2.0 Federation Console](images/SAML2.0FederationConsole.png)

## SAML 2.0 Federation

- cần setup trust giữa AWS IAM và SAML (both ways)
- SAML 2.0 enabled web based, cross domain SSO
- Uses the STS API: `AssumeRoleWithSAML`
- đó là các cũ để làm federation, cách được AWS khuyến nghị là sử dụng **Amazon Single Sign On (SSO)**
