# Environment variable

## Tổng quan

Environment variable (biến môi trường) thường được định nghĩa trong file với tên `terraform.tfvar`.

Ví dụ:

```tf
AWS_REGION = "ap-northeast-1"
AWS_ACCESS_KEY = "key"
```

File `terraform.tfvars` phải được đưa vào `.gitignore`

## TF_VAR

Đây là prefix string dùng khi thiết lập đầu vào thông qua biến môi trường.

```sh
# export environment variable
export TF_VAR_instructor_name = "Foo"

terraform apply
```
