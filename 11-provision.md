# Terraform provision

Mặc định `terraform apply` có thể provision lên đến **10 resources** (quá trình provision được thực thi một cách song song) - đồng nghĩa với việc terraform có thể **cùng một lúc** thêm mới, cập nhật, xoá 10 resources.

Con số này được thiết lập dựa theo `-parallelism` flag.

Thường điều chỉnh con số `parallelism` theo các trường hợp như sau:

- Deploy hệ thống lớn
- CI/CD pipeline (do yêu cầu thời gian nghiêm ngặt)
- API Rate limit (do API limit từ phía provider)

## remote-exec & local-exec

Là các provisioner được sử dụng để chạy các scripts hoặc command trên môi trường lần lượt là `remote` và `local`.

Thường dùng trong quá trình boostrapping, config resource.

Ví dụ với `remote-exec`

```tf
resource "aws_instace" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"

  provisioner "remote-exec" {
    connection {
      type = "ssh"
      user = "ec2-user"
      private_key = file("~/.ssh/my-key.pem")
    }

    inline = [
      "sudo apt-get update",
      "sudo apt-get install -y nginx",
      "sudo service nginx start"
    ]
  }
}
```

Ví dụ với `local-exec`

```tf
resource "aws_s3_bucket" "example" {
  bucket = "my-bucket"

  provisioner "local-exec" {
    command = "echo 'S3 bucket created!' > /tmp/s3_notification.txt"
  }
}
```
