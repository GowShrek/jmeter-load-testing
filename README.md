# 🚀 JMeter Load Testing — Báo Cáo Thực Hành

> Bài thực hành Load Testing sử dụng Apache JMeter theo hướng dẫn của Simplilearn

---

## 📌 Mục Tiêu

- Hiểu khái niệm **Load Testing** và tại sao cần kiểm thử tải
- Cài đặt và cấu hình **Apache JMeter**
- Tạo **Test Plan** để kiểm thử hiệu năng một REST API
- Phân tích kết quả qua **Summary Report** và **Graph Results**

---

## 🛠️ Công Cụ Sử Dụng

| Công cụ | Phiên bản | Mục đích |
|---------|-----------|----------|
| Apache JMeter | 5.6.3 | Công cụ load testing chính |
| Java JDK | 21+ | Môi trường chạy JMeter |
| JSONPlaceholder API | - | API mẫu để test (`jsonplaceholder.typicode.com`) |
| Windows 10/11 | - | Hệ điều hành |

---

## ⚙️ Cấu Hình Test Plan

### Thread Group (Nhóm người dùng ảo)

| Thông số | Giá trị | Mô tả |
|----------|---------|-------|
| Number of Threads | `10` | 10 người dùng ảo đồng thời |
| Ramp-up Period | `5` giây | Thời gian để khởi động đủ 10 thread |
| Loop Count | `3` | Mỗi user lặp lại request 3 lần |
| **Tổng requests** | **300** | 10 × 3 × (số sampler) |

### HTTP Request Sampler

```
Protocol  : https
Server    : jsonplaceholder.typicode.com
Port      : 443
Method    : GET
Path      : /posts
```

### Listeners (Công cụ xem kết quả)

- ✅ **Summary Report** — tổng hợp thống kê
- ✅ **View Results Tree** — xem chi tiết từng request
- ✅ **Graph Results** — biểu đồ trực quan

---

## 📸 Ảnh Minh Họa

### 1. Cấu hình Thread Group

![Thread Group Configuration](images/screenshot2-thread-group.png)

> Cài đặt 10 virtual users với ramp-up 5 giây và loop 3 lần

---

### 2. HTTP Request Sampler

![HTTP Request Sampler](images/screenshot3-http-request.png)

> GET request đến `jsonplaceholder.typicode.com/posts`

---

### 3. Kết Quả Summary Report

![Summary Report](images/screenshot1-summary-report.png)

> Tổng hợp kết quả sau khi chạy 900 requests

---

## 📊 Kết Quả Load Test

| Chỉ số | Kết quả |
|--------|---------|
| Tổng số Requests | 900 |
| Average Response Time | 231 ms |
| Min Response Time | 76 ms |
| Max Response Time | 934 ms |
| Throughput | 29.4 requests/giây |
| Error Rate | 0.11% |
| Test Duration | ~31 giây |

### Nhận xét kết quả

- **Response time trung bình 231ms** — nằm trong ngưỡng chấp nhận được (< 500ms)
- **Error rate 0.11%** — rất thấp, chỉ 1 request thất bại trên 900
- **Throughput 29.4 req/s** — API xử lý ổn định dưới tải 10 concurrent users
- **Max response time 934ms** — một vài request bị chậm do server load tức thời

---

## 📁 Cấu Trúc Repo

```
jmeter-load-testing/
├── README.md               # Báo cáo này
├── load-test.jmx           # File Test Plan của JMeter
└── images/
    ├── screenshot1-summary-report.png
    ├── screenshot2-thread-group.png
    └── screenshot3-http-request.png
```

---

## 📝 Các Bước Thực Hiện

1. **Cài đặt Java** — tải từ [java.com](https://www.java.com/en/download/)
2. **Cài đặt JMeter** — tải từ [jmeter.apache.org](https://jmeter.apache.org/download_jmeter.cgi)
3. **Tạo Test Plan** — thêm Thread Group → HTTP Request → Listeners
4. **Chạy test** — bấm nút Run (▶️)
5. **Phân tích kết quả** — xem Summary Report

---

## 📚 Tài Liệu Tham Khảo

- [Apache JMeter Official Documentation](https://jmeter.apache.org/usermanual/index.html)
- [Simplilearn JMeter Tutorial](https://www.youtube.com/watch?v=NTyY8wKSvik)
- [JSONPlaceholder Free API](https://jsonplaceholder.typicode.com/)

---

## 👤 Tác Giả

**GowShrek** — Bài thực hành môn Kiểm thử phần mềm

> Repo nộp bài: [github.com/GowShrek/jmeter-load-testing](https://github.com/GowShrek/jmeter-load-testing)
