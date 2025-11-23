Trong sơ đồ mạng của bạn (tại **Section 13 - VLAN_MANAGEMENT**), thiết bị có nhãn **FCM** thường là viết tắt của **Firewall Central Management** (Quản lý tường lửa tập trung).

Đối với một công ty tài chính có nhiều chi nhánh và sử dụng Hybrid Cloud như FastPay, bạn không thể đăng nhập thủ công vào từng con Firewall (HQ, Branch, Cloud) để cấu hình từng dòng lệnh được. Việc này rất dễ gây lỗi (ví dụ: mở port ở HQ nhưng quên mở ở Cloud).

**Máy FCM** chính là "nhạc trưởng" giải quyết vấn đề này.

Dưới đây là gợi ý thiết bị cụ thể cho FCM dựa trên 2 phương án tường lửa chúng ta đã bàn trước đó:

---

### PHƯƠNG ÁN A: NẾU BẠN CHỌN PALO ALTO (Khuyên dùng)

Nếu bạn đã chọn Firewall Palo Alto cho hệ thống, thì FCM của bạn chính là thiết bị chạy phần mềm **Panorama**.

- **Tên thiết bị:** **Palo Alto M-200 Management Appliance**.
    
- **Vai trò:**
    
    - **Quản lý đồng bộ:** Bạn chỉ cần viết Rule 1 lần trên con M-200 này, nó sẽ tự động đẩy xuống Firewall ở HQ, Chi nhánh và cả trên AWS.
        
    - **Log Collector:** Nó đóng vai trò là kho lưu trữ Log khổng lồ. Mọi nhật ký tấn công từ các Firewall con sẽ được gửi về đây để lưu trữ (quan trọng cho kiểm toán tài chính).
        
- **Cấu hình phần cứng (M-200):**
    
    - **CPU:** 8 Cores / 16 Threads.
        
    - **RAM:** 32 GB.
        
    - **Storage:** 4 x 4TB HDD (RAID 10) -> Cần dung lượng lớn để lưu Log trong 6-12 tháng.
        
    - **Management Capacity:** Quản lý tối đa 1,000 thiết bị.
        
- **Giá tham khảo:**
    
    - Thiết bị M-200: **~$15,000 USD** (380 triệu VNĐ).
        
    - License quản lý (25 thiết bị): **~$5,000 USD/năm**.
        

---

### PHƯƠNG ÁN B: NẾU BẠN CHỌN FORTINET

Nếu bạn chọn FortiGate để tiết kiệm chi phí, thì FCM của bạn là **FortiManager**.

- **Tên thiết bị:** **FortiManager FMG-200G**.
    
- **Vai trò:**
    
    - Quản lý tập trung toàn bộ FortiGate trong hệ thống.
        
    - Tính năng **Fabric Connector**: Giúp đồng bộ chính sách bảo mật với AWS/GCP (SDN Connector).
        
    - Tự động hóa việc update firmware cho toàn hệ thống.
        
- **Cấu hình phần cứng (FMG-200G):**
    
    - **Storage:** 8 TB (Lưu log thoải mái).
        
    - **Capacity:** Quản lý tối đa 30 thiết bị FortiGate (Đủ cho mô hình của bạn).
        
- **Giá tham khảo:**
    
    - Thiết bị: **~$6,000 USD** (150 triệu VNĐ).
        
    - License (FortiCare): **~$2,000 USD/năm**.
        

---

### Cập nhật vào Bảng chi phí thiết bị (Mục 5.1.3 trong PDF)

Để hoàn thiện đồ án, bạn nên đưa thông tin này vào phần chi phí quản lý. Tôi sẽ lấy cấu hình **Palo Alto M-200** làm chuẩn Enterprise (vì bạn đang xây dựng mô hình ngân hàng lớn).

|**Loại thiết bị**|**Mã sản phẩm**|**Số lượng**|**Mô tả chức năng**|**Đơn giá (USD)**|**Thành tiền (VNĐ)**|
|---|---|---|---|---|---|
|**FCM Appliance**|**Palo Alto M-200**|1|Thiết bị quản lý tường lửa tập trung (Panorama), lưu trữ Log tập trung 16TB, đồng bộ chính sách Hybrid Cloud.|$15,000|381,000,000|
|**License FCM**|Panorama Device Lic|1 gói|Bản quyền quản lý cho 25 thiết bị Firewall.|$5,000|127,000,000|

### Tại sao cần "Máy FCM" này trong đồ án?

Trong phần bảo vệ đồ án, nếu giảng viên hỏi: _"Làm sao em đảm bảo chính sách bảo mật ở AWS giống hệt như ở Data Center?"_

Bạn sẽ trả lời: _"Dạ, em sử dụng thiết bị **FCM (Palo Alto M-200)**. Em cấu hình **Device Group** và **Template** trên FCM, sau đó push đồng loạt xuống cả Firewall vật lý và Firewall ảo trên Cloud, đảm bảo tính nhất quán (Consistency) và tránh sai sót của con người ạ."_ -> Câu trả lời này chắc chắn ăn điểm tối đa.