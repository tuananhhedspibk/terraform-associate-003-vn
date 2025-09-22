# Sensitive data

## Tổng quan

**Sensitive data** là các thông tin nhạy cảm như `key` hoặc `database password`, ...

Ta có thể sử dụng backend như `S3` để mã hoá state cũng như các thông tin nhạy cảm.

State best pratice:

1. Coi state như `sensitive data`.
2. Mã hoá state backend.
3. Control access tới state file.

## State được lưu dưới dạng plain text

Ngay cả với các thông tin được đánh dấu là `sensitive`. Terraform run có thể in các giá trị "nhạy cảm" này ra log.
