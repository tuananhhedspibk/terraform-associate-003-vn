# Terraform workspace

## Workspace là gì?

Nói một cách đơn giản thì workspace trong terraform giống như khái niệm môi trường - environment như `develop`, `staging`, `production`.

Các workspaces này sẽ chia sẻ cùng một code base nhưng states thì khác nhau hoàn toàn.

Sau khi khởi tạo working directory, `default` worspace sẽ được tạo một cách tự động. `default` workspace này tồn tại vĩnh viễn và **KHÔNG THỂ BỊ XOÁ**.

## Các câu lện với workspace

### terraform workspace new [name]

Tạo workspace mới

### terraform workspace list

Liệt kê tất cả các workspaces hiện có trong local.

### terraform workspace delete [name]

Xoá workspace, nhưng hãy nhớ rằng **chúng ta không thể xoá default workspace**

## Sử dụng workspace trong code

```tf
provider "aws" {
  region = var.region
}

resource "aws_instance" "example" {
  count         = terraform.workspace == "production" ? 5 : 1
  ami           = "ami-12345678"
  instance_type = "t2.micro"

  tags = {
    Name = "example-${terraform.workspace}"
  }
}
```

Phân tích một chút về ví dụ trên, chúng ta có thể sử dụng workspace trong code thông qua `terraform.workspace`, với cách làm này chúng ta có thể kiểm tra workspace hiện thời khi thực thi terraform planning hoặc terraform apply.
