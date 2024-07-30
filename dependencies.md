# Dependencies

## Explicit dependencies

```tf
resource "aws_instance" "example" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"

  depends_on = [
    aws_security_group.example
  ]

  tags = {
    Name = "example-instance"
  }
}
```

Cùng phân tích ví dụ trên, bằng việc sử dụng từ khoá `depends_on` chúng ta có thể thấy rằng `aws_instance` phụ thuộc một cách "tường minh" vào `aws_security_group` tức là trước khi tạo `aws_instance` thì `aws_security_group` cần phải được tạo trước đó đã.

## Implicit dependencies

Đây là cách Terraform tạo một dependency graph từ config file dựa theo tham chiếu giữa các resources.

```tf
resource "aws_security_group" "example" {
  name        = "example-sg"
  description = "Example security group"
  vpc_id      = var.vpc_id

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

resource "aws_instance" "example" {
  ami             = "ami-12345678"
  instance_type   = "t2.micro"
  security_groups = [
    aws_security_group.example.name
  ]

  tags = {
    Name = "example-instance"
  }
}
```

Phân tích ví dụ trên, trước khi tạo `aws_instance` chúng ta phải có `aws_security_group` nên do đó `security_group` sẽ được tạo trước, và `security_group` sẽ được gọi là `implicit dependency`.

Ưu điểm:

1. Đơn giản hoá config file khi ta không cần sử dụng keyword `depends_on`.
2. Đảm bảo rằng terraform sẽ tạo resources theo đúng thứ tự mong muốn.
