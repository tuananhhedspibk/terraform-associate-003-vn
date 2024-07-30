# State trong terraform

Đây là nhân tố cần thiết giúp terraform hoạt động được.

Bạn có thể xem chi tiết tại: <https://developer.hashicorp.com/terraform/language/state/purpose>

## Lợi ích của việc sử dụng state trong terraform

### Mapping với thế giới thực

State giúp mapping resources trong terraform config tới resources có trong thế giới thực.

Terraform kì vọng rằng mỗi một remote object sẽ chỉ tham chiếu tới duy nhất một resource instance tương ứng trong config mà thôi.

Sau khi tạo resourcé trên môi trường cloud, terraform sẽ ghi nhận "danh tính" của resource này trong state.

### Metadata

Terraform cũng theo dõi metadata của resources thông qua state.

Terraform sử dụng config để định nghĩa ra được thứ tự phụ thuộc giữa các dependencies. Điều này đặc biệt quan trọng khi chúng ta muốn huỷ các resources (resources nào sẽ được huỷ trước? Resources nào sẽ bị huỷ ngay sau đó?).

Cùng lấy một ví dụ với AWS provider.

```tf
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"

  enable_dns_hostnames = true
  enable_dns_support   = true

  tags = {
    Name = "vpc"
  }
}

resource "aws_internet_gateway" "main" {
  vpc_id = aws_vpc.main.id

  tags = {
    Name = "interget-gateway"
  }
}
```

Trong ví dụ trên, tôi muốn tạo `aws_vpc` và `aws_internet_gateway`, gateway này sẽ được gắn vào vpc. Để thực hiện điều đó, thuộc tính `vpc_id` của `internet_gateway` phải được gán với **id của vpc**.

Khi scan để tạo resources, Terraform sẽ hiểu rằng mình phải tạo internet_gateway **sau** khi tạo vpc.

Bạn có thể xem code chi tiết hơn tại: <https://github.com/tuananhhedspibk/NewAnigram-Infrastructure/blob/main/terraform/network/main.tf>

### Hiệu năng

Terraform state sẽ cache giá trị của các thuộc tính để cải thiện hiệu năng, đặc biệt là trong các hệ thống infrastructure với quy mô lớn, resource query sẽ gặp các vấn đề như sau:

- Chậm (do số lượng resource cần query rất nhiều).
- Một vài cloud providers không cung cấp API cho query nhiều resources cùng một lúc.
- Cloud provider API rate limiter.

Do đó, việc caching các thông tin về resources trong state sẽ giúp chúng ta cải thiện về mặt hiệu năng.
