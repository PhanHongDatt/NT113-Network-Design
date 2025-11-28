Nếu chúng ta tạm bỏ qua vấn đề về Bảng địa chỉ IP (coi như đã điền đúng) và vấn đề công nghệ GRE (coi như bạn đã có phần giải trình hợp lý), thì khi soi kỹ vào báo cáo 1, vẫn còn **4 LỖI LỚN** về mặt logic thiết kế và dự toán thiết bị.

Những lỗi này cho thấy sự "thiếu đồng bộ" giữa lời văn chém gió (phần lý thuyết) và thực tế mua sắm (phần bảng giá).

Dưới đây là phân tích chi tiết:

### 1. Lỗi "Hồn Trương Ba, Da Hàng Thịt" tại Chi nhánh (Lãng phí khủng khiếp)

- **Mô tả lỗi:** Tại bảng chi phí thiết bị2, bạn mua 6 con **Cisco Nexus 93180YC-FX3** cho cả hệ thống (Trụ sở + 2 Chi nhánh).
    
- **Phân tích:**
    
    - **Nexus 9300** là dòng Switch chuyên dụng cho **Data Center** (trung tâm dữ liệu) để chạy máy chủ ảo hóa, độ trễ thấp. Việc đặt nó ở Trụ sở (nơi có Private Cloud) là **ĐÚNG**.
        
    - Tuy nhiên, việc đặt Nexus 9300 làm Core Switch cho **Chi nhánh Hà Nội & Đà Nẵng** là **SAI LẦM**. Chi nhánh chỉ có người dùng (User), không có Server Farm khủng. Dùng Nexus ở chi nhánh giống như "mua xe Container để chở pizza". Nó vừa đắt, vừa khó quản lý (Nexus chạy hệ điều hành NX-OS khác với Catalyst), vừa không hỗ trợ tốt các tính năng cho người dùng (như POE, 802.1X cho PC).
        
- **Cách sửa:**
    
    - Tại Trụ sở: Giữ nguyên Nexus 93180YC.
        
    - Tại Chi nhánh: Đổi sang dòng **Cisco Catalyst 9500** hoặc dùng chính **Catalyst 9300** làm Core (Collapsed Core). Giá rẻ hơn 1 nửa và phù hợp tính năng Campus.
        

### 2. Lỗi "Nói một đằng, mua một nẻo" về Wifi

- **Mô tả lỗi:**
    
    - Trong phần giải pháp 3,4, bạn viết rất hay về "Triển khai Wi-Fi công ty và Wi-Fi khách", "Wireless Router".
        
    - Nhưng trong bảng chi phí thiết bị 5, **KHÔNG CÓ MỘT CON ACCESS POINT (AP) NÀO ĐƯỢC MUA**.
        
- **Phân tích:** Bạn mua Router, Core Switch, Access Switch, Server... nhưng quên mất thiết bị phát sóng Wifi. Hệ thống mạng không dây của bạn đang "chạy bằng niềm tin".
    
- **Lỗi thuật ngữ:** Tại mục6, bạn dùng từ **"Wireless Router"**. Đây là từ dùng cho thiết bị gia đình (cục Wifi nhà mạng). Doanh nghiệp Enterprise dùng từ **"Wireless Access Point (AP)"**.
    
- **Cách sửa:**
    
    - Bổ sung vào bảng chi phí thiết bị: **Cisco Catalyst 9120AX** hoặc **9130AX** (Số lượng tùy theo diện tích văn phòng, ví dụ mỗi chi nhánh 5-10 con).
        
    - (Tùy chọn) Bổ sung thêm **Wireless LAN Controller (WLC)** hoặc ghi chú là chạy "Embedded WLC trên Switch 9300".
        

### 3. Lỗi "Zero Trust" nhưng thiếu công cụ thực thi (Thiếu NAC)

- **Mô tả lỗi:**
    
    - Mục 1.2.1 7và 1.5 8 bạn khẳng định hệ thống đảm bảo **"Zero Trust Security"**.
        
    - Tuy nhiên, bảng thiết bị bảo mật 9 chỉ có Firewall (Palo Alto).
        
- **Phân tích:** Firewall chỉ bảo vệ chặn cửa ra vào. Còn **Zero Trust** yêu cầu kiểm soát ngay tại cổng mạng của nhân viên (Ví dụ: Máy kế toán cắm nhầm vào cổng IT thì phải bị chặn ngay). Firewall không làm được việc này ở lớp Access. Bạn cần một thiết bị **Network Access Control (NAC)** để xác thực và đẩy Dynamic VLAN. Không có NAC (như Cisco ISE), tuyên bố "Zero Trust" của bạn chỉ là lý thuyết suông.
    
- **Cách sửa:** Thêm thiết bị/license **Cisco ISE** hoặc ít nhất là giải trình việc xác thực 802.1X được thực hiện qua Radius Server tích hợp đâu đó (nhưng với mô hình Enterprise thì nên mua ISE).
    

### 4. Lỗi lưu trữ Log (Thiếu ổ cứng cho SIEM)

- **Mô tả lỗi:**
    
    - Mục 1.5 10 bạn triển khai **SIEM (Wazuh)** để giám sát log.
        
    - Mục 5.1.2 11 bạn mua Storage Node dùng **"NVMe SSD"** (loại đắt tiền nhất) cho Private Cloud.
        
- **Phân tích:**
    
    - Log của công ty tài chính cần lưu trữ lâu dài (6-12 tháng) theo luật. Dung lượng Log rất lớn (Big Data).
        
    - Việc lưu Log lên ổ cứng **NVMe** (giá siêu đắt, chuyên dùng cho Database tốc độ cao) là một sự lãng phí tiền bạc khủng khiếp. Không ai lưu Log nguội (Cold Data) trên ổ NVMe cả.
        
- **Cách sửa:**
    
    - Trong phần Storage Node, nên chia làm 2 loại ổ đĩa:
        
        1. **NVMe/SSD:** Để chạy máy ảo, Database (Hot Data).
            
        2. **HDD (Dung lượng lớn):** Để lưu trữ Log Wazuh, Backup (Cold Data). Hoặc mua thêm một thiết bị **NAS/SAN** riêng dùng ổ HDD giá rẻ để lưu Log.
            

### Tóm lại:

Nếu bạn khắc phục được 4 điểm này, đồ án của bạn sẽ rất chặt chẽ:

1. **Đừng mua Nexus cho Chi nhánh** (Đổi sang Catalyst).
    
2. **Mua thêm Wifi AP** vào bảng giá (Đừng để quên).
    
3. **Thêm giải pháp NAC** (Cisco ISE) để hiện thực hóa Zero Trust.
    
4. **Tối ưu lưu trữ Log** (Đừng dùng dao mổ trâu NVMe để cắt tiết gà Log file).