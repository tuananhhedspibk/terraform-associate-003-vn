# Các câu lệnh thường dùng với terraform

## terraform init

Theo như hashicorp câu lệnh init này có chức năng như sau:

> Khởi tạo working directory bao gồm các files config và cài đặt các plugins cần thiết cho providers.

Bên cạnh đó câu lệnh `terraform init` có một vài options như:

`terraform init -backend-config=PATH` - dùng để chỉ định ra đường dẫn cụ thể của backend-config file.

hoặc là

`terraform init -backend-config="KEY=VALUE"` - chỉ định cặp `KEY / VALUE` cho backend-config.

`terraform init` cũng cache source code của các plugins đã được download trong môi trường local.

## terraform plan

Theo như hashicorp câu lệnh plan có chức năng như sau:

> Tạo plan thực thi, cho phép người dùng có thể xem trước những sự thay đổi mà terraform sẽ tạo ra cho các infra resources.

Với câu lệnh này, chúng ta có thể biết được resources nào sẽ được tạo mới, cập nhật hoặc bị huỷ sau khi những sự thay đổi được phản ánh trong thực tế.

Ví dụ với việc tạo mới resource

![Screenshot 2024-07-27 at 11 05 41](https://github.com/user-attachments/assets/61a35914-3e85-4dfa-87ce-ece833a668e0)

Việc cập nhật resource trông sẽ như sau

![Screenshot 2024-07-27 at 11 18 09](https://github.com/user-attachments/assets/75fccf37-4e17-49c8-8f5f-eed78bf455fb)

Với option `-out`, bạn có thể lưu plan để sử dụng sau đó.

## terraform apply

Theo như hashicorp, câu lệnh này dùng để:

> Thực thi các hành động đã được terraform plan trước đó để tạo, cập nhật hoặc huỷ infrastructure resource.

With above example about creating new `aws_vpc`, after executing `terraform apply` with answering my choice "yes" or "no" on creating resource:

Với ví dụ phía trên về việc tạo mới `aws_vpc`, sau khi chạy `terraform apply` với câu trả lời là "yes".

![Screenshot 2024-07-27 at 11 11 48](https://github.com/user-attachments/assets/353ffcff-30bb-48eb-b8ae-c0c12f959dca)

Bạn có thể có được vpc của chính mình trên AWS cloud như dưới đây:

![Screenshot 2024-07-27 at 11 16 36](https://github.com/user-attachments/assets/037530ce-b7db-4565-903d-61d1175b20bb)

Chú ý rằng khi chạy lệnh apply, ngay cả khi có những resources bị rollback thì những resources nào được tạo thành công vẫn sẽ được deployed mà không hề bị rollback.

### -refesh option

Với option `refresh=false` câu lệnh `terraform apply -refesh=false` sẽ bỏ qua quá trình refresh state trước khi apply.

### -replace option

Sử dụng để "tạo lại" resource.

Ví dụ:

```tf
provider "aws" {
  region = "ap-northeast-1"
}

resource "aws_instance" "example" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"

  tags = {
    Name = "example-instance"
  }
}
```

Bạn có thể chạy câu lệnh dưới đây để "tạo lại" aws_instance

```sh
terraform apply -replace=aws_instance.example
```

### Với config file rỗng

Với config file (.tf) rỗng, nếu bạn chạy `terraform apply` toàn bộ resources sẽ bị huỷ.

## terraform destroy

Như cái tên của chính mình, câu lệnh này dùng để huỷ đi các resources.

Gõ lệnh và đây là thông báo bạn sẽ nhận được từ terraform:

![Screenshot 2024-07-27 at 11 18 35](https://github.com/user-attachments/assets/a9a7cc20-2695-46f4-b605-2212c35e36b2)

Có 2 cách dùng chính với câu lệnh này như sau.

### Chỉ huỷ một phần resource

Bước 1: Chạy `terraform state rm` để loại bỏ đi resource mà ta **KHÔNG MUỐN HUỶ** khỏi state.

Bước 2: Chạy `terraform destroy`

### Force destroy

`terraform destroy -auto-approve` - huỷ toàn bộ resources mà không cần confirm.

## terraform state

Đây là một nhóm các câu lệnh dùng để quản lí state. Bạn có thể gõ lệnh `terraform state` vào terminal, tất cả các subcommands sẽ được show ra.

Theo kinh nghiệm cá nhân, tôi thường sử dụng lệnh `terraform state list` để liệt kê các resources có trong state.

![Screenshot 2024-07-27 at 11 27 00](https://github.com/user-attachments/assets/2df2ee24-86ab-4f16-b2bd-3c366c686d30)

## terraform console

Launch interactive console của terraform.

Lệnh này đọc terraform configure ở working dir hiện tại và terraform state file nên CLI phải có khả năng lock state để tránh sự thay đổi state.

## terraform force-unlock

Khi chạy lệnh `terraform plan` hoặc `terraform apply` (với backend là `local`), sẽ sinh ra file `.terraform.tfstate.lock.info` để lock state lại nhằm tránh tình trạng state bị chỉnh sửa cùng một lúc bởi nhiều người.

Bẻ khoá (lock) state hiện thời

```sh
terrform force-unlock LOCK_ID
```

Terraform "lock" state để tránh các concurrent modifies có thể gây ra lỗi.

Chỉ có một vài backend hỗ trợ locking:

- GCP
- Azure
- AWS S3 with DynamoDB

## terraform validate

Câu lệnh này dùng để kiểm tra và báo cáo lỗi trong module, attribute names, value types để đảm bảo chúng đúng với cú pháp cũng như đảm bảo sự thống nhất bên trong.

## terraform refresh

Đọc toàn bộ các settings từ các remote objects và cập nhật lại state để match với remote objects.

Đây chính là bước đầu tiên được chạy khi thực thi lệnh `terraform plan`

`terraform plan -refresh-only`: Chỉ cập nhật state file mà không làm thay đổi gì đến infra của provider. Thường dùng trong trường hợp khi resource online bị chỉnh sửa bằng tay, ta sẽ chạy lệnh này để cập nhật lại state file. Do `terraform refesh` bị deprecated nên hãy sử dụng `terraform plan -refresh-only` với cùng tác dụng như nhau.

## terraform import

Để import các resources có sẵn vào terraform state. Cho phép ta có thể đưa các resources không nhất thiết được tạo bằng terraform vào sự quản lí của terraform mà không cần phải tạo lại resources.

```sh
terraform import ADDRESS ID
```

## terraform output

Đọc giá trị của biến output từ terraform state.

## terraform fmt

Format terraform config file. Đảm bảo codebases có tính thống nhất cao.

Tuỳ chọn `-recursive` giúp format với cả các files trong các thư mục con.

## terraform graph

Đưa ra đồ thị dependency trong config hiện thời dưới dạng ngôn ngữ `DOT`. Ngoài ra nó cũng có thể visual config hoặc execute plan.
