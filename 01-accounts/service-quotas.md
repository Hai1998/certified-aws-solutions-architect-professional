# AWS Service Quotas

- Defines how much of a "thing" we can use inside of an AWS account
- Example:
    - Number of EC2 instances at a certain times per region
    - Number of IAM users per AWS accounts
- Services thường có 1 default per region quota
- Global services có thể có 1 per account quota instead per region
- hầu hêts services quotas có thể tăng nếu cần
- một vài service quotes không thể change, example: number of IAM users per account (5000)
- Service endpoint and quotas: [https://docs.aws.amazon.com/general/latest/gr/aws-service-information.html](https://docs.aws.amazon.com/general/latest/gr/aws-service-information.html)
- **Service Quotas**: 
    - From the console we can go to *Service Quotas* page, nơi có thể tạo dashboards cho quotas khi muốn monitor
    - có thể request quota changes từ service này cho một số services cụ thể
    - *Quote request template*: có thể xác định quota value request cho new accounts trong 1 AWS organization
    - có thể tạo CloudWatch Alarm dựa trên service quota cụ thể
- Legacy method to increase quotas: create a support ticket selecting service quota increase
- We can request service quota increase from the CLI as well. Reference API: [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/service-quotas/request-service-quota-increase.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/service-quotas/request-service-quota-increase.html)
