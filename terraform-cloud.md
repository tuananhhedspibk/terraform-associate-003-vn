# Terraform Cloud

## Mối liên hệ với Terraform CLI

![Screenshot 2024-05-30 at 21 42 59](https://github.com/tuananhhedspibk/tuananhhedspibk.github.io/assets/15076665/e785b0b6-9890-458a-b5f5-34c0b1644308)

## Workspace

Workspace trong terrraform cloud sẽ là các working directories riêng biệt chứ không giống như workspace của `terraform workspace`

Naming convention `a-b-c`.

![Screenshot 2024-05-30 at 21 44 26](https://github.com/tuananhhedspibk/tuananhhedspibk.github.io/assets/15076665/1419d30c-2cb2-4d7f-acb1-fe0c90bbf1bf)

## Authentication

`terraform login` sẽ sử dụng `API Token` từ `app.terraform.io` để login vào terraform cloud.

## Secure variables

Có thể chuyển sang chế độ sensitive với các thông tin quan trọng như key, ...

## Version control

Mục đích chính là để nhiều người có thể làm việc trên cùng một terraform code.

Được tích hợp với các version control như `Github`, `Bitbucket`, ...

## Private registry

Lưu và versioning các terraform modules cho mục đích tái sử dụng.

Cũng có thể liên kết từ Github (VCS) repo sang. Các repo này phải có tên theo format `terraform-<PROVIDER>-<NAME>`

## Sentinel policy

Có thể hiểu như là Policy-as-code. Sentinel policy sẽ nằm ở giữa `terrform plan` và `terrform apply` stages.

Policy có 3 levels chính đó là:

- `Advisory`: policy được cho phép failed, sẽ đưa ra log ở dạng warning.
- `Soft Mandatory`: cho phép override còn không thì phải pass.
- `Hard Mandatory`: bắt buộc phải pass, không cho phép override.

Sentinal policy evaluation sẽ được thực thi `sau khi terrafom hoàn thành plan, cost estimation và trước khi apply`.

## Version control workflow

Dùng khi làm việc nhóm. Liên kết terraform workspace với VCS repo và trigger apply hoặc plan mỗi khi commit code.

Ngoài ra nhiều workspace (dev, stg, prod) có thể liên kết và sử dụng chung một code base từ VCS repo.

Với các workspace liên kết với VCS, yêu cầu VCS-driven workflow để đảm bảo `single source of truth` chứ không được phép chạy `terraform apply` bằng CLI hoặc phương thức khác (vẫn có thể chạy `terraform plan`).

## Agents

Thực thi terraform plan và apply changes tới infrastructre

Chức năng cơ bản:

- Nhận `terraform plan` từ Terraform cloud.
- Chạy plan dưới local.
- Apply những sự thay đổi đó.
