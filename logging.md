# Terraform Log

## Enabled logging

Set `TF_LOG` env variable để cho phép `detailed log`

Set `TF_LOG_PATH` để thiết lập đường dẫn cho file lưu log.

## Verbose logging terraform

Để enable verbose logging trong terraform, ta cần thiết lập giá trị cho biến môi trường `TF_LOG` với các level logs như sau:

- `TRACE`: show các internal logs của terraform. Cho ta thấy được nhiều thông tin chi tiết hơn (các bước đi từ `terraform plan`, `terraform apply` hay `terraform destroy`)
- `DEBUG`
- `INFO`
- `WARN`
- `ERROR`
