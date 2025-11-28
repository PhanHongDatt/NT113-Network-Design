Dựa trên kiến trúc mạng **FastPay** mà bạn đã thiết kế (có Private Cloud OpenStack, có kết nối VPN/Leased Line ổn định giữa các chi nhánh), vị trí triển khai **Cisco ISE** tối ưu nhất là **TẬP TRUNG (Centralized Deployment) tại Trụ sở chính (HQ)**.

Dưới đây là sơ đồ bố trí cụ thể và lý do tại sao:

### 1. Vị trí vật lý & Logic

Bạn **KHÔNG NÊN** mua thiết bị phần cứng Cisco SNS (Secure Network Server) đắt tiền. Hãy tận dụng hạ tầng Private Cloud mạnh mẽ của bạn.

- **Vị trí lắp đặt:** Data Center tại TP.HCM (Trụ sở chính).
    
- **Nền tảng:** Chạy dưới dạng **Máy ảo (Virtual Machine)** trên cụm Server OpenStack (Dell R750)1.
    
- **VLAN:** Đặt Interface của ISE vào **VLAN 99 (Management)** 2 hoặc VLAN Server Farm. Đây là nơi an toàn nhất, được Firewall bảo vệ kỹ càng.
    
- **Mô hình:** Triển khai 1 cụm gồm **2 Node (Primary & Secondary)** chạy song song (HA) để đảm bảo nếu một con server chết, nhân viên vẫn đăng nhập được mạng.
    

### 2. Sơ đồ luồng xác thực (Authentication Flow)

Để bạn dễ hình dung và vẽ vào đồ án, luồng hoạt động sẽ như sau:

1. **Tại Trụ sở (Local):**
    
    - Nhân viên cắm dây vào Switch hoặc bắt Wifi -> Switch/WLC gửi gói tin RADIUS đến IP của ISE (tại VLAN 99) -> ISE trả lời OK/Reject -> Nhân viên vào mạng.
        
2. **Tại Chi nhánh Hà Nội/Đà Nẵng (Remote):**
    
    - Nhân viên cắm dây vào Switch chi nhánh -> Switch chi nhánh gửi gói tin RADIUS chạy qua đường **VPN Site-to-Site/Leased Line** 3 về Trụ sở TP.HCM -> ISE xử lý -> Trả kết quả về lại Hà Nội.
        

> Câu hỏi phản biện: "Nếu đứt mạng VPN về trụ sở thì chi nhánh nghỉ làm à?"
> 
> Trả lời: "Dạ không. Trên Switch chi nhánh em cấu hình tính năng Critical Auth VLAN. Nếu không liên lạc được với ISE, Switch sẽ tự động cho phép nhân viên vào một VLAN hạn chế (chỉ ra được Internet) để làm việc cơ bản, khi nào có mạng lại sẽ xác thực lại sau."

### 3. Cập nhật vào Bảng chi phí thiết bị (Mục 5.1.3)

Trong bảng chi phí "Hạ tầng các thiết bị bảo mật và quản lý"4, bạn chưa có dòng nào cho Cisco ISE. Bạn cần thêm vào để hợp lý hóa việc triển khai "Zero Trust"5.

Bạn hãy thêm dòng sau:

|**Loại thiết bị**|**Tên sản phẩm**|**Số lượng**|**Mô tả chức năng**|**Đơn giá (Ước tính)**|**Thành tiền**|
|---|---|---|---|---|---|
|**Network Access Control (NAC)**|**Cisco ISE VM License** (Bản quyền phần mềm)|1 Hệ thống (2 Node HA)|Xác thực người dùng (802.1x), phân quyền VLAN động, kiểm soát thiết bị (Zero Trust). Gói **Premier License** cho 1000 Endpoints.|**$25,000**|**625,000,000 VNĐ**|

### 4. Tại sao không đặt ISE ở từng chi nhánh?

- **Chi phí:** License ISE rất đắt. Mua thêm node cho chi nhánh sẽ đội chi phí lên rất cao.
    
- **Đường truyền tốt:** Bạn đã thuê đường truyền chất lượng cao (Leased Line/FTTH Business)6, độ trễ thấp nên việc xác thực từ xa về HQ chỉ mất vài chục mili-giây, người dùng không cảm nhận được độ trễ.
    
- **Quản lý tập trung:** Đặt hết ở HQ giúp IT dễ dàng backup, update và đồng bộ chính sách (Policy).
    

Tóm lại:

Trong đồ án, hãy vẽ Cisco ISE nằm cạnh các máy chủ Controller của OpenStack tại TP.HCM, kết nối vào Core Switch Nexus qua VLAN Management. Đó là vị trí chuẩn Enterprise nhất.