
Dựa trên yêu cầu thiết kế hệ thống cho công ty tài chính **FastPay** được mô tả trong tài liệu (yêu cầu bảo mật cao, Zero Trust, Hybrid Cloud, giao dịch trực tuyến độ trễ thấp)111, tôi xin đề xuất giải pháp tường lửa (Firewall) phù hợp nhất là dòng **Next-Generation Firewall (NGFW)** của **Palo Alto Networks**.

Đây là lựa chọn hàng đầu cho mô hình tài chính Enterprise cần tuân thủ **Zero Trust** 2và kiểm soát ứng dụng Lớp 73.

Dưới đây là gợi ý chi tiết về chủng loại, vị trí tích hợp và chi phí dự kiến:

### 1. Tường lửa biên cho Trụ sở chính (HQ Core Firewall)

Với hạ tầng sử dụng **Router Cisco C8300** 44và **Core Switch Nexus 93180YC**55, tường lửa cần có cổng kết nối tốc độ cao (10G/25G) và khả năng xử lý thông lượng lớn.

- **Model gợi ý:** **Palo Alto PA-3410** (Chạy chế độ High Availability - HA Active/Passive).
    
- **Vị trí tích hợp:**
    
    - Đặt giữa **Router biên (Cisco C8300)** và **Core Switch (Nexus 9300)**.
        
    - Đấu nối: Sử dụng cổng quang 10G/25G kết nối trực tiếp lên Core Switch để kiểm soát luồng traffic đi ra Internet và đi vào vùng Server Farm (Private Cloud).
        
- **Đặc điểm nổi bật (đáp ứng yêu cầu PDF):**
    
    - **Layer 7 Inspection:** Kiểm soát ứng dụng ở tầng 7 thay vì chỉ chặn port như firewall truyền thống, đáp ứng yêu cầu tại mục 1.56.
        
    - **Threat Prevention (IPS/IDS):** Tích hợp sẵn hệ thống ngăn ngừa xâm nhập để chặn các cuộc tấn công vào hệ thống tài chính7.
        
    - **SSL Decryption:** Giải mã lưu lượng HTTPS để kiểm tra mã độc ẩn trong các giao dịch mã hóa.
        
- **Chi phí dự kiến (Cặp HA + License 1 năm):**
    
    - Phần cứng (2 thiết bị): ~$60,000 USD.
        
    - License (Threat Prevention, DNS, WildFire): ~$30,000 USD/năm.
        
    - **Tổng: ~2.3 tỷ VNĐ.**
        

### 2. Tường lửa cho Chi nhánh (Branch Firewall)

Chi nhánh Hà Nội và Đà Nẵng cần kết nối VPN Site-to-Site về trụ sở 8 và bảo vệ truy cập Internet trực tiếp cho nhân viên.

- **Model gợi ý:** **Palo Alto PA-440**.
    
- **Vị trí tích hợp:**
    
    - Đặt ngay sau Router biên của chi nhánh hoặc thay thế luôn vai trò Gateway nếu quy mô chi nhánh nhỏ.
        
    - Kết nối VPN Site-to-Site về thiết bị PA-3410 tại trụ sở.
        
- **Đặc điểm:**
    
    - Đồng bộ chính sách bảo mật (Security Policy) với trụ sở chính.
        
    - Đảm bảo Zero Trust cho người dùng tại chi nhánh khi truy cập tài nguyên nội bộ.
        
- **Chi phí dự kiến (Cho 2 chi nhánh):**
    
    - Phần cứng (2 thiết bị): ~$4,000 USD.
        
    - License: ~$2,000 USD/năm.
        
    - **Tổng: ~150 triệu VNĐ.**
        

### 3. Tường lửa ảo hóa cho Private Cloud (Internal Segmentation)

Hệ thống Private Cloud dùng **Dell Servers** và **OpenStack** 9999 cần bảo vệ luồng dữ liệu "East-West" (giữa các máy ảo Database, App, Web) để ngăn chặn tấn công lan truyền.

- **Model gợi ý:** **VM-Series (VM-300 hoặc VM-500)**.
    
- **Vị trí tích hợp:**
    
    - Cài đặt dưới dạng máy ảo (Virtual Appliance) ngay trên hạ tầng ảo hóa (KVM/OpenStack) của các **Compute Node**1010.
        
    - Hoạt động như một Gateway ảo cho các VLAN/Subnet trong môi trường Cloud.
        
- **Đặc điểm:**
    
    - Thực thi chính sách vi phân đoạn (Micro-segmentation): Ví dụ VLAN Kế toán không thể truy cập VLAN IT trừ khi được phép11.
        
    - Ngăn chặn mã độc lây lan giữa các máy ảo.
        
- **Chi phí dự kiến:**
    
    - Tính theo Credit hoặc vCPU.
        
    - **Khoảng: $15,000 USD/năm (~380 triệu VNĐ).**
        

### 4. Tường lửa ứng dụng Web (WAF) cho Public Cloud

Hệ thống giao dịch trực tuyến trên AWS/GCP 12 cần bảo vệ chuyên biệt chống tấn công vào ứng dụng web (SQL Injection, XSS, DDoS).

- **Model gợi ý:** **Cloudflare Enterprise** hoặc **AWS WAF**.
    
- **Vị trí tích hợp:**
    
    - Đứng trước Load Balancer trên Cloud (AWS/GCP).
        
- **Đặc điểm:**
    
    - Bảo vệ Layer 7 cho giao dịch trực tuyến.
        
    - Chống DDoS quy mô lớn.
        
- **Chi phí dự kiến:**
    
    - **Khoảng: $5,000 USD/tháng (~1.5 tỷ VNĐ/năm).**
        

---

### Bảng tổng hợp chi phí dự kiến cho Tường lửa

Dựa trên cấu trúc chi phí trong PDF13131313, đây là bảng bổ sung cho phần thiết bị bảo mật:

| **Loại thiết bị** | **Mã sản phẩm (Gợi ý)** | **Số lượng** | **Mô tả chức năng**                                                     | **Đơn giá (USD)** | **Thành tiền (VNĐ)** |
| ----------------- | ----------------------- | ------------ | ----------------------------------------------------------------------- | ----------------- | -------------------- |
| **HQ Firewall**   | Palo Alto PA-3410       | 2            | Tường lửa biên, HA Mode, bảo vệ Data Center, Zero Trust, IPS/IDS lớp 7. | $30,000           | 1,520,000,000        |
| **Branch FW**     | Palo Alto PA-440        | 2            | Tường lửa chi nhánh, VPN Site-to-Site.                                  | $2,000            | 100,000,000          |
| **Internal FW**   | VM-Series (Lic)         | 1            | Tường lửa ảo hóa cho OpenStack Private Cloud (Subscription 1 năm).      | $15,000           | 380,000,000          |
| **License**       | Threat Prevention       | 1            | Gói đăng ký bảo mật, cập nhật mẫu mã độc 1 năm cho HQ.                  | $30,000           | 760,000,000          |
| **Tổng cộng**     |                         |              |                                                                         |                   | **~2,760,000,000**   |

**Lưu ý:** Mức giá này tương xứng với quy mô thiết bị mạng Core (Nexus 9300) và Server (Dell R750) mà bạn đã liệt kê trong file, đảm bảo tính đồng bộ "Enterprise Grade" cho toàn hệ thống.