# Amazon Cognito

- Provides authentication, authorization and user management for mobile/web applications
- Provides 2 main pieces of functionality:
    - **User Pools**: được sủ dụng để sign-in, cung cấp 1 JSON Web Token (JWT) sau đó authentication
        - Cho phép bạn tạo và quản lý danh sách người dùng cho ứng dụng của bạn. Người dùng có thể đăng ký, đăng nhập, và quản lý hồ sơ cá nhân thông qua các giao diện do Cognito cung cấp.
        - **User Pools không grant access cho AWS services!**
        - User Pools cung cấp user directory management và user profiles, sign-up and sing-in với web UI tuỳ chỉnh, MFA và tính năng security khác
        - Chúng cho phép social sign-in cung cấp bởi Google, Apple, Facebook cũng như sử dung sign-in với loại identity như nhà cung cấp SAML identity
    - **Identity Pools**: mục đích của identity pool là trao đổi identity bên ngoài để lấy AWS credentials tạm thời, cái mà có thể access vào AWS resources
        - Unauthenticated Identities: identity pools có thể cung cấp access cho AWS services cho guest users (unauthenticated users)
        - Federated Identities: authorize users để tạm thời access AWS resources sau đó chúng được authenticate với Google, Facebook, Twitter, SAML 2.0 và event Cognito User Pools. Cho phép người dùng đăng nhập vào ứng dụng của bạn bằng thông tin từ các nhà cung cấp danh tính bên ngoài như Facebook, Google, Apple, hoặc bằng các tài khoản doanh nghiệp qua SAML hoặc OIDC

## Cognito concepts

![Cognito Concepts](images/CognitoConcepts.png)

