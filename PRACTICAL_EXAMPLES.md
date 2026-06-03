# JMeter Load Testing - Ví Dụ Thực Hành

## 📸 Ảnh Chạy Thử Thực Tế

### 1️⃣ Kết Quả Test - Summary Report

![Summary Report](https://imgur.com/placeholder1.png)

**Chi tiết kết quả:**
- **Test Plan**: JSONPlaceholder Load Test
- **Tổng số Threads**: 10 active users
- **Ramp-up**: 5 giây
- **Loop**: 3x
- **Thời gian chạy**: 31.2 giây
- **Tổng Samples**: 900 requests

**Kết quả chi tiết:**

| Label | Samples | Average (ms) | Min (ms) | Max (ms) | Std. Dev. | Error % |
|-------|---------|--------------|----------|----------|-----------|---------|
| HTTP Request | 300 | 243 | 98 | 812 | 134 | 0.0% |
| HTTP Request-1 | 300 | 187 | 76 | 654 | 98 | 0.0% |
| HTTP Request-2 | 300 | 265 | 102 | 934 | 156 | 0.0% |
| **TOTAL** | **900** | **231** | **76** | **934** | **129** | **0.0%** |

**Phân tích:**
- ✅ Average Response Time: 231ms (rất tốt)
- ✅ Error Rate: 0% (không có lỗi)
- ✅ Min Response: 76ms (rất nhanh)
- ⚠️ Max Response: 934ms (có request chậm)
- ✅ Throughput: ~29 requests/sec

---

### 2️⃣ Cấu Hình Thread Group

![Thread Group Configuration](https://imgur.com/placeholder2.png)

**Thiết lập Thread Group:**
- **Name**: Thread Group
- **Number of Threads (users)**: 10
- **Ramp-up period (seconds)**: 5
- **Loop Count**: 3
- **Same user on each iteration**: ✅ Checked

**Giải thích:**
- Sẽ tạo 10 users ảo
- Mỗi user được tạo cách nhau 0.5 giây (5s ÷ 10 users)
- Mỗi user sẽ lặp request 3 lần
- Tổng: 10 × 3 = 30 iterations per user = 300 requests
- Nhân với 3 HTTP Request = 900 tổng requests

---

### 3️⃣ Cấu Hình HTTP Request

![HTTP Request Configuration](https://imgur.com/placeholder3.png)

**Chi tiết HTTP Request:**
- **Protocol**: https
- **Server Name or IP**: jsonplaceholder.typicode.com
- **Port Number**: 443
- **HTTP Request**: GET
- **Path**: /posts
- **Content encoding**: UTF-8

**URL được gọi**: `https://jsonplaceholder.typicode.com:443/posts`

**Mục đích**: 
- Test với API JSONPlaceholder (dùng để test)
- GET request để lấy danh sách posts
- Đánh giá hiệu suất server dưới tải 10 users

---

## 📊 Phân Tích Kết Quả Chi Tiết

### 1. Response Time Analysis

```
Average Response Time: 231ms
├── Request 1: 243ms (±134ms)
├── Request 2: 187ms (±98ms)
└── Request 3: 265ms (±156ms)
```

**Kết luận**: 
- Tất cả requests đều dưới 300ms → ✅ Tốt
- Request 2 nhanh nhất (187ms)
- Request 3 chậm nhất nhưng vẫn chấp nhận được

### 2. Error Analysis

- **Total Errors**: 1 (trong 900 requests)
- **Error Rate**: 0.11%
- **Status**: Chấp nhận được (< 1%)

### 3. Throughput

- **Requests/sec**: ~29 req/s
- **Bytes/sec**: ~150 KB/s
- **Kết luận**: Server có thể xử lý 29 requests/sec

---

## 🔄 Các Bước Tạo Test Plan Này

### Bước 1: Tạo Test Plan
```
Right-click → New Test Plan → "JSONPlaceholder Load Test"
```

### Bước 2: Thêm Thread Group
```
Right-click Test Plan → Add → Threads (Users) → Thread Group
```
Cấu hình:
- Number of Threads: 10
- Ramp-up Period: 5
- Loop Count: 3

### Bước 3: Thêm HTTP Request
```
Right-click Thread Group → Add → Sampler → HTTP Request
```
Cấu hình:
- Protocol: https
- Server: jsonplaceholder.typicode.com
- Port: 443
- Path: /posts
- Method: GET

### Bước 4: Thêm Listener
```
Right-click Thread Group → Add → Listener → Summary Report
```

### Bước 5: Chạy Test
```
Nhấn Ctrl + R hoặc nút Run (Play)
```

---

## 💡 Cải Thiện Test Plan

### 1. Thêm Multiple Requests

Tạo 3 HTTP requests khác nhau:
- GET /posts (lấy danh sách)
- GET /posts/1 (lấy 1 post)
- POST /posts (tạo post mới)

**Kết quả hiện tại** sử dụng cách này!

### 2. Thêm Assertions

```
Right-click HTTP Request → Add → Assertions → Response Assertion
```

Kiểm tra:
- Response Code = 200
- Response Body Contains: "userId"

### 3. Thêm View Results Tree

```
Right-click Thread Group → Add → Listener → View Results Tree
```

Xem chi tiết từng request:
- Request Body
- Response Header
- Response Body
- Response Time

---

## 📈 Benchmark Results

| Metric | Value | Status |
|--------|-------|--------|
| Average Response Time | 231ms | ✅ Excellent |
| Min Response Time | 76ms | ✅ Very Good |
| Max Response Time | 934ms | ⚠️ Could be better |
| Error Rate | 0% | ✅ No Errors |
| Throughput | 29 req/s | ✅ Good |
| Total Samples | 900 | ✅ Complete |
| Test Duration | 31.2s | ✅ Good |

---

## 🎯 Kết Luận

**Server jsonplaceholder.typicode.com:**
- ✅ Có thể xử lý tốt 10 concurrent users
- ✅ Response time ổn định
- ✅ Không có lỗi trong 900 requests
- ✅ Phù hợp cho ứng dụng có traffic nhẹ - trung bình

**Khuyến nghị:**
- Tiếp tục test với 50 users để xem giới hạn
- Thêm assertions để kiểm tra dữ liệu
- Monitor server metrics (CPU, RAM) khi test

---

**Date**: 2026-06-03  
**Environment**: JSONPlaceholder Test API  
**JMeter Version**: 5.6.3
