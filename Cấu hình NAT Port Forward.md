# 📘 Lab 03 – Cấu hình NAT Port Forward trên pfSense

## 1. Mục đích

* Hiểu cách hoạt động của **NAT (Network Address Translation)**
* Cho phép truy cập từ mạng **WAN** vào một dịch vụ trong **LAN**
* Thực hành cấu hình **Port Forward** trên pfSense

---

## 2. Mô tả

Trong mạng nội bộ (**LAN**), các máy không thể được truy cập trực tiếp từ Internet.

**NAT Port Forward** cho phép:

* Chuyển tiếp truy cập từ **WAN → LAN**
* Ví dụ:

```text id="v3cn9t"
WAN IP:8080 → 192.168.48.11:80
```

---

## 3. Mô hình

### pfSense

* **WAN**: DHCP
* **LAN**: `192.168.48.1`

### Server nội bộ

* **IP**: `192.168.48.11`
* **Port**: `80` (Web Server)

---

## 4. Các bước thực hiện

### Bước 1: Truy cập NAT

Vào menu:

```text id="vbihh7"
Firewall → NAT
```

Chọn tab:

```text id="vchd8x"
Port Forward
```

---

### Bước 2: Thêm Rule NAT

[NATportPicture1](.Image/NATportPicture1.png)


---

### Bước 3: Cấu hình NAT Port Forward

| Tham số                  | Giá trị          |
| ------------------------ | ---------------- |
| **Interface**            | `WAN`            |
| **Protocol**             | `TCP`            |
| **Destination**          | `WAN Address`    |
| **Destination Port**     | `8080`           |
| **Redirect Target IP**   | `192.168.48.11`  |
| **Redirect Target Port** | `80`             |
| **Description**          | `NAT Web Server` |

---

### Bước 4: Tạo Firewall Rule

* Tick chọn:

```text id="m5f0jn"
Add associated firewall rule
```

Hoặc tạo thủ công tại:

```text id="s8ck0d"
Firewall → Rules → WAN
```

---

### Bước 5: Lưu cấu hình

* Nhấn:

```text id="d2n7lt"
Save
```

* Sau đó nhấn:

```text id="dvg9y2"
Apply Changes
```

---

## 5. Kiểm tra kết quả

Sau khi cấu hình xong, bạn có thể kiểm tra bằng cách truy cập từ phía WAN:

```text id="bz5zju"
http://<WAN-IP>:8080
```

Ví dụ:

```text id="f2w5fw"
http://192.168.x.x:8080
```

Nếu cấu hình đúng, pfSense sẽ chuyển tiếp truy cập đến:

```text id="5r4b3v"
192.168.48.11:80
```

---

## 6. Lưu ý

* Đảm bảo máy `192.168.48.11` đang chạy **Web Server**
* Kiểm tra firewall trên máy server (**Windows / Linux**)
* Nếu không truy cập được, cần kiểm tra:

  * Port có mở đúng không
  * IP server có đúng không
  * Rule **WAN** đã được tạo chưa
  * NAT Rule đã **Apply Changes** chưa

---

---
