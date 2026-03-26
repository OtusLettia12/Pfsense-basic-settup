# 🚀 pfSense Lab - Giới hạn băng thông theo IP trong LAN

## 📌 Giới thiệu

Bài lab này mô phỏng việc sử dụng **pfSense Firewall** để **giới hạn băng thông mạng** cho **một máy cụ thể trong LAN** bằng tính năng **Traffic Shaper / Limiters**.

Mục tiêu của bài lab là kiểm soát băng thông của một máy Windows 10 trong môi trường mạng nội bộ, phục vụ cho các mục đích:

* Quản lý băng thông người dùng
* Mô phỏng chính sách QoS cơ bản
* Thực hành Firewall Rule + Traffic Shaping trên pfSense

---

## 🎯 Mục tiêu bài lab

* Cài đặt và cấu hình pfSense trong môi trường ảo hóa
* Tạo **Limiter** cho Upload / Download
* Áp dụng giới hạn băng thông cho **duy nhất 1 máy**
* Kiểm tra hiệu quả bằng công cụ đo tốc độ Internet

### Máy bị giới hạn

```text
Windows 10 - 192.168.48.13
```

---


## 🖥️ Môi trường triển khai

* **Hypervisor**: VMware Workstation
* **Firewall**: pfSense
* **Client**: Windows 10
* **LAN Subnet**: `192.168.48.0/24`
* **Target Host**: `192.168.48.13`

---

## ⚙️ Cấu hình thực hiện

### 1. Tạo Limiter

Vào menu:

```text
Firewall → Traffic Shaper → Limiters
```

Tạo 2 limiter:

#### 📥 Download Limiter

* **Name**: `WIN10_Download`
* **Bandwidth**: cấu hình theo nhu cầu (ví dụ `3 Mbps` hoặc `5 Mbps`)
- ![LimitPicture1](.Images/LimitPicture1.png)

#### 📤 Upload Limiter

* **Name**: `WIN10_Upload`
* **Bandwidth**: cấu hình theo nhu cầu
- ![LimitPicture2](.Images/LimitPicture2.png)

---

### 2. Tạo Firewall Rule cho máy Windows 10

Vào menu:

```text
Firewall → Rules → LAN
```

Tạo rule mới với các thông số:

* **Action**: `Pass`
* **Interface**: `LAN`
* **Address Family**: `IPv4`
* **Protocol**: `Any`

#### Source

* **Type**: `Address or Alias`
* **IP**: `192.168.48.13`

#### Destination

* **Any**

#### Description

```text
Limit bandwidth Win10
```
- ![LimitPicture3](.Images/LimitPicture3.png)
---


### 3. Gán Limiter vào Rule

Trong phần **Advanced Options** của rule:

#### In / Out pipe

* **In pipe** → `WIN10_Upload`
* **Out pipe** → `WIN10_Download`

- ![LimitPicture4](.Images/LimitPicture4.png)

> 📌 Giải thích:
>
> * **In pipe** = lưu lượng từ client đi vào firewall → Upload
> * **Out pipe** = lưu lượng từ firewall trả về client → Download

---


## 📊 Kết quả kiểm tra

Sau khi cấu hình xong, tiến hành đo tốc độ mạng trên máy Windows 10.

### 🔹 Trước khi giới hạn băng thông

- ![LimitPicture5](.Images/LimitPicture5.png)

**Kết quả:**

* Download: **27.74 Mbps**
* Upload: **36.79 Mbps**
* Ping: **12.90 ms**

---

### 🔹 Sau khi áp dụng giới hạn

 ![LimitPicture6](.Images/LimitPicture6.png)

**Kết quả:**

* Download: **3.54 Mbps**
* Upload: **23.05 Mbps**
* Ping: **13.50 ms**

---

