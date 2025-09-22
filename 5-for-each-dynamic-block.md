# for-each và dynamic block

Lí do chính khiến tôi đưa hai khái niệm này vào cùng một phần đó là chúng ta có thể sử dụng `for_each` và `dynamic block` để tạo ra:

- Repeatable resources
- Looping

## for-each

Ví dụ với `for_each`

```tf
variable public_subnets {
  default = {
    "public_subnet_1" = 1
    "public_subnet_2" = 2
    "public_subnet_3" = 3
  }
}

resource "aws_subnet" "public_subnets" {
  for_each                = var.public_subnets // iterable set
  map_public_ip_on_launch = true
}
```

Cùng phân tích ví dụ trên:

Với phần **định nghĩa biến**, chúng sẽ map với 3 cặp key/value như sau:

- public_subnet_1 / 1
- public_subnet_2 / 2
- public_subnet_3 / 3

Với phần **định nghĩa resource** `for_each = var.public_subnets` sẽ nói với terraform rằng: "Tôi muốn bạn tạo ra 3 subnets tương ứng với public_subnets config ('public_subnet_1', 'public_subnet_2' and 'public_subnet_3')" và kết quả đạt được đó là chúng ta sẽ có `3 subnets`:

- public_subnet_1
- public_subnet_2
- public_subnet_3

Nhân tiện đây, tôi cũng muốn nói thêm về `count`.

Cả `for_each` và `count` được sử dụng để tạo ra nhiều instances cùng một lúc nhưng:

- `for_each` có thể làm việc với `set` hoặc `map,` bên cạnh đó `for_each` cung cấp một cách để tham chiếu đến các instances đó là thông qua biến `key`.
- `count` chỉ có thể hoạt động với `list` mà thôi.

Cùng xét một ví dụ như sau:

Đầu tiên là với `for_each`

```tf
variable "security_groups" {
  type = map(object({
    name        = string
    description = string
  }))

  default = {
    sg1 = {
      name        = "allow_ssh"
      description = "Allow SSH traffic"
    }
    sg2 = {
      name        = "allow_http"
      description = "Allow HTTP traffic"
    }
  }
}

resource "aws_security_group" "example" {
  for_each    = var.security_groups

  name        = each.value.name
  description = each.value.description
  vpc_id      = aws_vpc.main.id

  ingress {
    from_port  = 22
    to_port    = 22
    protocol   = "tcp"
    cidr_block = ["0.0.0.0/0"]
  }

  egress {
    from_port  = 0
    to_port    = 0
    protocol   = "-1"
    cidr_block = ["0.0.0.0/0"]
  }
}
```

Cùng phân tích ví dụ:

Với phần **định nghĩa biến**, tôi định nghĩa biến `security_groups` là một map của các objects. Mỗi object sẽ bao gồm 2 thuộc tính là `name` và `description`.

Với phần **định nghĩa resource**, sử dụng `for_each` để duyệt qua `security_groups` map. Với mỗi security_group, nó tự động gán giá trị cho `name` và `description` dựa theo `each.value`.

Và giờ là `count`

```tf
variable "instances" {
  type = list(object({
    ami           = string
    instance_type = string
    name          = string
  }))

  default = [
    {
      ami           = "ami-0c55b159cbfafe1f0"
      instance_type = "t2.micro"
      name          = "example-instance-1"
    },
    {
      ami           = "ami-0c55b159cbfafe1f1"
      instance_type = "t2.small"
      name          = "example-instance-3"
    },
    {
      ami           = "ami-0c55b159cbfafe1f2"
      instance_type = "t2.medium"
      name          = "example-instance-3"
    }
  ]
}

resource "aws_instance" "example" {
  count         = length(var.instances)
  ami           = var.instances[count.index].ami
  instance_type = var.instances[count.index].instance_type

  tags = {
    Name = var.instances[count.index].name
  }
}
```

Cùng phân tích ví dụ:

Với phần **định nghĩa biến**, định nghĩa `instances` là một list của các objects. Mỗi một object sẽ bao gồm các thuộc tính `ami`, `instance_type`, `name`.

Với phần **định nghĩa resource**, thiết lập giá trị cho tham số `count` là độ dài của list `instances`. Với mỗi instance, nó tự động gán giá trị cho các thuộc tính `ami`, `instance_type`, `name` dựa theo giá trị của `count.index` hiện thời.

## dynamic block

Chúng ta cùng nhau chuyển qua dynamic block, chúng ta sử dụng dynamic block để tạo ra các "block" (đôi khi là các nested block).

Một dynamic block sẽ có các thông tin như sau:

- `content`: iterable content.
- `for_each`: collection sẽ được duyệt qua.
- `iterator`: optional, chúng ta sử dụng nó để chỉ ra tên của biến được sử dụng trong `content` block.

Cùng lấy một ví dụ

```tf
variable "ingress_rules" {
  type = list(object({
    from_port   = number
    to_port     = number
    protocol    = string
    cidr_blocks = list(string)
  }))

  default = [
    {
      from_port   = 80
      to_port     = 80
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    },
    {
      from_port   = 22
      to_port     = 22
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    }
  ]
}

resource "aws_security_group" "example" {
  name   = "example"
  vpc_id = aws_vpc.main.id

  dynamic "ingress" {
    for_each = var.ingress_rules

    content {
      from_port   = ingress.value.from_port
      to_port     = ingress.value.to_port
      protocol    = ingress.value.protocol
      cidr_blocks = ingress.value.cidr_blocks
    }
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
```

Phân tích ví dụ:

Với phần **định nghĩa biến**, định nghĩa `ingress_rules` là một list của các objects bao gồm các thuộc tính như `from_port`, `to_port`, `protocol`, `cidr_blocks`.

Với phần **định nghĩa resource**, từ khoá **dynamic** sẽ giúp chúng ta tạo ra các blocks với số lượng bằng với số lượng các phần tử có trong `ingress_rules` list và gán giá trị cho các thuộc tính `from_port`, `to_port`, `protocol`, `cidr_blocks` dựa theo `ingress.value`.
