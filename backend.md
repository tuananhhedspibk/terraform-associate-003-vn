# Terraform backend

## Tổng quan

Backend trong terraform chỉ ra rằng state được load, lưu trữ như thế nào.

Các loại backend types:

- `local`: lưu state file trong local file. Đây là default backend.
- `consul`: là một sự lựa chọn phổ biến để lưu Terraform state.
- `s3`: lưu terraform state trên Amazon S3.

### Tại sao lại sử dụng backend?

1. `Collab`: tránh việc phát sinh conflict khi nhiều members cùng làm việc trên cùng một môi trường infra.
2. `State locking`: tránh việc đồng thời sửa state cùng một lúc.
3. `Remote storage`: đảm bảo state file được lưu ở nơi tập trung và có tính bảo mật cao.
4. `Backup, Recovery`: tự động backup và recovery.

## Backend migration

Ta có thể chuyển (migrate) state từ backend này sang backend khác.

```sh
terraform init -migrate-state
```

Lệnh này sẽ giúp tái sử dụng lại các providers được đọc từ lock file.

![Screenshot 2024-05-25 at 22 14 17](https://github.com/tuananhhedspibk/tuananhhedspibk.github.io/assets/15076665/b6254670-7004-4597-a981-877f3ee1d10e)
