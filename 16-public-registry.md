# Terraform registry

## Terraform registry là gì?

Theo như định nghĩa của hashicorp

> Terraform registry là một repository của tất cả các providers, modules, policies, run tasks. Nếu không có terraform registry, việc sử dụng các terraform config là không thể∂

## Yêu cầu để publish terraform public registry

1. Phải có github public repository tương ứng.
2. Repositorys name phải theo format `terraform-<PROVIDER>-<NAME>` - với `PROVIDER` có thể là aws hoặc google, ... `NAME` chỉ đơn thuần là một cái tên mô tả cho repository. Ví dụ: `terraform-aws-ec2-instance`
3. Cấu trúc các files trong module phải như sau

```text
├── main.tf
├── variables.tf
├── outputs.tf
└── README.md
```

4. Document phải có:

- Mô tả
- Hướng dẫn sử dụng
- Inputs
- Outputs

5. Semantic versioning: ví dụ `v1.0.0` or `v1.0.1`, bạn có thể tạo một tag version cho github repo bằng câu lệnh phía dưới

```sh
git tag v1.0.0
git push origin v1.0.0
```
