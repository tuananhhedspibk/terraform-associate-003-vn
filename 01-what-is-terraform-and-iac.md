# Terraform và Iac là gì ?

## Terraform

Theo như định nghĩa của Hashicorp

> Terraform là một công cụ "infrastructure as code" cho phép người dùng có thể xây dựng, thay đổi và version hoá các cloud resources một cách an toàn và hiệu quả

Nói một cách đơn giản, terraform là công cụ cho phép người dùng thực hiện code infrastructure mà không cần các thiết lập quá phức tạp.

## IaC

IaC là viết tắt của `Infrastructure as Code`.

Theo như AWS

> Infrastructure as Code (IaC) cho phép người dùng provision computing infrastructure sử dụng code thay vì các thiết lập thủ công.

Về lợi ích, IaC có thể đem đến cho người dùng những điều sau:

- Versioning cho infrastructure.
- Chia sẻ và tái sử dụng infrastructure code.

Bên cạnh đó IaC có thể đem đến những lợi ích khác như:

- Idempotent
- Consistent (Tính thống nhất)
- Repeatable (Khả năng có thể lặp lại)
- Predictable (Khả năng dự đoán)

Khi không sử dụng IaC, việc scaling up infrastructure sẽ yêu cầu một vài điều kiện khác như:

- Kết nối remote tới servers
- Phải provision và config server thông qua nhiều câu lệnh hoặc các scripts.

## Các vấn đề khi quản lí infrastructure bằng tay

1. Khó đảm bảo các components được config một cách thống nhất.
2. Khó đảm bảo sự thống nhất, tiêu chuẩn giữa các môi trường khác nhau.
3. Provisioning chậm.
4. Khó tránh được các lỗi liên quan đến con người như `missed config`, ...
5. Khó trong việc tạo document.
