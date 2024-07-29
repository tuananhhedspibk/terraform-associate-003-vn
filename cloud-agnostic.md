# Cloud agnostic

## Tổng quan

Là khả năng app có thể vận hành trên bất kì nền tảng cloud nào mà không phụ thuộc vào bất cứ một tính năng cụ thể của nền tảng đó (AWS, GCP, Azure).

Điều này giúp hệ thống có thể chuyển đổi dễ dàng giữa các nền tảng Cloud mà không làm ảnh hưởng đến kiến trúc tổng quan của hệ thống.

## Các đặc điểm cơ bản của cloud-agnostic

- `Interoperability`: khả năng hoạt động liền mạch trên các môi trường cloud khác nhau.
- `Portability`: Ứng dụng dễ dàng migrate, deploy giữa các cloud platform khác nhau.
- `Independence`: Không phụ thuộc vào bất kì một API hoặc services cụ thể nào.
- `Flexibility`: Khả năng lựa chọn cloud provider phù hợp với mình cũng như thay đổi provider một cách dễ dàng.

Để đạt được mục tiêu `cloud-agnostic` các tổ chức thường dùng:

- Docker (container hoá)
- Kubernetes (Orchestration tool)
