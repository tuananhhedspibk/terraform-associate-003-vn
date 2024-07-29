# Terraform settings

## Mục đích của terraform settings

Dùng để định nghĩa các thiết lập cũng như các behaviors cơ bản cho terraform như:

- Yêu cầu về minimum version của terraform.
- Yêu cầu về version của các providers

```tf
terraform {
  required_version ">= 0.12.0"

  backend "s3" {
    bucket = "my-terraform-state"
    key    = "terraform.tfstate"
    region = "ap-northeast-1"
  }

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "3.57.0"
    }
  }
}
```

`required_providers` được sử dụng để chỉ ra version cụ thể của provider mà chúng ta muốn sử dụng.
