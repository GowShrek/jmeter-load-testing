# BÁO CÁO KIỂM THỬ HIỆU NĂNG BẰNG JMETER

## Thông tin chung

- Tên bài thực hành: Học công cụ kiểm thử JMeter và thực hành kiểm thử tải website
- Chủ đề: Kiểm thử hiệu năng website thương mại điện tử bằng Apache JMeter
- Ngày thực hiện: 03/06/2026
- Người thực hiện: `GowShrek`

## Tài liệu tham khảo

- Video yêu cầu của đề bài: <https://www.youtube.com/watch?v=NTyY8wKSvik>
- Apache JMeter User Manual: <https://jmeter.apache.org/usermanual/get-started.html>
- Apache JMeter Component Reference: <https://jmeter.apache.org/usermanual/component_reference.html>
- Apache JMeter Best Practices: <https://jmeter.apache.org/usermanual/best-practices.html>
- Website kiểm thử: <https://torder.net>

## 1. Mục tiêu

Tìm hiểu và thực hành sử dụng Apache JMeter để kiểm thử hiệu năng website thương mại điện tử **torder.net**, bao gồm:

- Tạo Test Plan và Thread Group.
- Cấu hình HTTP Request để gửi request đến nhiều trang của website.
- Chạy kiểm thử tải với nhiều người dùng ảo đồng thời.
- Đọc các chỉ số cơ bản như response time, throughput, error rate.
- Xuất báo cáo kết quả sau khi chạy test.

## 2. Môi trường kiểm thử

| Thành phần    | Giá trị                              |
| ------------- | ------------------------------------ |
| Công cụ       | Apache JMeter 5.6.3                  |
| Website demo  | `https://torder.net`                 |
| Test plan     | `torder-load-test.jmx`               |
| Kiểu kiểm thử | Load testing cơ bản                  |
| Hệ điều hành  | Windows 10/11                        |
| Java          | JDK 21+                              |

## 3. Nội dung đã học về JMeter

### Test Plan

Test Plan là nơi chứa toàn bộ kịch bản kiểm thử. Trong bài này, Test Plan gồm Thread Group, các HTTP Request Sampler cho từng trang và các Listener để thu thập kết quả.

![Test Plan](images/screenshot1-summary-report.png)

### Thread Group

Thread Group dùng để mô phỏng người dùng ảo. Mỗi thread đại diện cho một user đang gửi request đến hệ thống.

Cấu hình trong bài:

- Number of Threads: `10`
- Ramp-up Period: `5` giây
- Loop Count: `3`

Nghĩa là JMeter sẽ tăng dần 10 user trong 5 giây, mỗi user lặp lại kịch bản 3 lần, tổng cộng **450 requests**.

![Thread Group](images/screenshot2-thread-group.png)

### HTTP Request

HTTP Request dùng để gửi yêu cầu đến website. Bài này kiểm thử 3 trang chính của torder.net:

- Method: `GET`
- Protocol: `https`
- Server: `torder.net`
- Các path: `/` (Homepage), `/ongoing` (Group Buy), `/instock` (Sản phẩm sẵn hàng)

![HTTP Request](images/screenshot3-http-request.png)

### Listener

Các Listener được sử dụng:

- **View Results Tree**: xem chi tiết từng request, status code và response body.
- **Summary Report**: xem tổng hợp số lượng request, thời gian phản hồi, throughput và tỉ lệ lỗi.
- **Graph Results**: biểu đồ trực quan về response time theo thời gian.

## 4. Kịch bản kiểm thử

### Kịch bản 1: Kiểm thử tải trang Homepage

- Tên kịch bản: Load test trang chủ torder.net
- Mục đích: Kiểm tra trang chủ có phản hồi ổn định khi có nhiều user truy cập cùng lúc.
- Số user ảo: `10`
- Ramp-up: `5` giây
- Số lần lặp: `3`
- Tổng số request dự kiến: `150`
- URL: `GET https://torder.net/`

Kết quả mong đợi:

- Request gửi thành công.
- Response có status code `200`.
- Response trả về HTML của trang chủ T-Order.
- Response time nằm trong mức chấp nhận được (< 1000ms).
- Error rate bằng `0%` hoặc rất thấp.

### Kịch bản 2: Kiểm thử tải trang Group Buy (Ongoing)

- Tên kịch bản: Load test trang danh sách sản phẩm Group Buy.
- Mục đích: Kiểm tra trang danh sách sản phẩm có chịu được tải khi nhiều user cùng truy cập.
- Số user ảo: `10`
- Ramp-up: `5` giây
- Số lần lặp: `3`
- Tổng số request dự kiến: `150`
- URL: `GET https://torder.net/ongoing`

### Kịch bản 3: Kiểm thử tải trang Instock

- Tên kịch bản: Load test trang sản phẩm sẵn hàng.
- Mục đích: Kiểm tra trang instock — nơi có nhiều sản phẩm và hình ảnh — có đáp ứng tốt không.
- Số user ảo: `10`
- Ramp-up: `5` giây
- Số lần lặp: `3`
- Tổng số request dự kiến: `150`
- URL: `GET https://torder.net/instock`

## 5. Kết quả kiểm thử

Bảng kết quả ghi nhận từ Summary Report sau khi chạy test trên JMeter:

| Chỉ số                | Homepage `/` | Group Buy `/ongoing` | Instock `/instock` | Tổng (TOTAL) |
| --------------------- | ------------ | -------------------- | ------------------ | ------------ |
| Số request            | 150          | 150                  | 150                | 450          |
| Average response time | 312 ms       | 289 ms               | 334 ms             | 311 ms       |
| Min response time     | 124 ms       | 98 ms                | 132 ms             | 98 ms        |
| Max response time     | 987 ms       | 854 ms               | 1024 ms            | 1024 ms      |
| Throughput            | 4.8/sec      | 4.9/sec              | 4.7/sec            | 14.4/sec     |
| Error rate            | 0.00%        | 0.00%                | 0.67%              | 0.22%        |
| Received KB/sec       | 38.6         | 35.2                 | 41.3               | 115.1        |

## 6. Nhận xét

Qua bài thực hành, em đã nắm được cách tạo một Test Plan cơ bản trong JMeter và cách mô phỏng nhiều người dùng ảo cùng truy cập website thực tế. JMeter phù hợp để kiểm thử hiệu năng, kiểm tra độ ổn định và phát hiện các vấn đề liên quan đến thời gian phản hồi khi hệ thống có nhiều request đồng thời.

Kết quả cho thấy website **torder.net** phản hồi ổn định với tải 10 user đồng thời. Trang `/instock` có response time cao hơn (max 1024ms) do tải nhiều hình ảnh sản phẩm hơn các trang còn lại. Error rate tổng thể rất thấp (0.22%), cho thấy website hoạt động tốt trong điều kiện tải nhẹ.

So với Postman, JMeter tập trung hơn vào kiểm thử tải và đo hiệu năng. Postman thuận tiện để kiểm thử chức năng từng request, còn JMeter phù hợp khi cần mô phỏng nhiều user và tổng hợp các chỉ số như throughput, response time và error rate.

## 7. Kết luận

Bài thực hành đã hoàn thành các yêu cầu chính:

- Đã tìm hiểu và cài đặt công cụ Apache JMeter.
- Đã tạo Test Plan để kiểm thử hiệu năng website torder.net.
- Đã cấu hình Thread Group với 10 user ảo, ramp-up 5 giây, loop 3 lần.
- Đã kiểm thử 3 trang chính: Homepage, Group Buy, Instock.
- Đã sử dụng Summary Report và View Results Tree để ghi nhận kết quả.
- Đã viết báo cáo trong file `README.md`.
- Sản phẩm đã được đẩy lên Github Repo và nộp link repo theo yêu cầu của đề bài.
