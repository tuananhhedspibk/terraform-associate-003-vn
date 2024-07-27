# Alias cho terraform provider

Sử dụng `alias` để có thể sử dụng **multiple provider blocks với cùng kiểu**

```tf
provider "aws" {
  region = "us-west-1"
  alias  = "us_west_1"
}

provider "aws" {
  region = "us-east-1"
  alias  = "us_east_1"
}

resource "aws_instance" "web_us_west_1" {
  provider = aws.us_west_1
  ami           = "ami-12345678"
  instance_type = "t2.micro"

  tags = {
    Name = "web-us-west-1"
  }
}

resource "aws_instance" "web_us_east_1" {
  provider = aws.us_east_1
  ami           = "ami-87654321"
  instance_type = "t2.micro"

  tags = {
    Name = "web-us-east-1"
  }
}
```

Cùng phân tích ví dụ trên:

Tôi sử dụng 2 `aws` providers với regions khác nhau và được alias với 2 cái tên lần lượt là `us_west_1` & `us_east_1` tương ứng với từng region.

Với mỗi provider, tôi sẽ sử dụng `alias name` như ví dụ ở trên sẽ lần lượt là `aws.us_east_1` & `aws.us_west_1`
