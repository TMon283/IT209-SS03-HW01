# Bài 1: SSH Port Hardening

## 1. Thay đổi SSH Port

Đăng nhập VPS bằng user `devops`:

```bash
ssh devops@<IP_ADDRESS_DROPLET>
```

Kiểm tra user:

```bash
whoami
# devops
```

Mở port `2222` trên UFW trước khi restart SSH:

```bash
sudo ufw allow 2222/tcp
sudo ufw status
```

Cấu hình SSH:

```bash
sudo nano /etc/ssh/sshd_config
```

Thay:

```text
#Port 22
```

thành:

```text
Port 2222
```

Kiểm tra cấu hình:

```bash
sudo sshd -t
```

Restart SSH:

```bash
sudo systemctl restart ssh
```

## 2. Kiểm tra

Từ máy cá nhân, kết nối port mới:

```bash
ssh -p 2222 devops@<IP_ADDRESS_DROPLET>
```

Kết quả: **Kết nối thành công bằng SSH Key.**

Kiểm tra port cũ:

```bash
ssh -p 22 devops@<IP_ADDRESS_DROPLET>
```

Kết quả: **Connection refused / Connection timed out.**

Sau khi xác nhận port `2222` hoạt động, đóng port `22`:

```bash
sudo ufw delete allow 22/tcp
```

## 3. Kết quả

* SSH Port: `22` → `2222`
* User: `devops`
* Port `2222`: hoạt động
* Port `22`: bị chặn
* UFW: đã cấu hình
