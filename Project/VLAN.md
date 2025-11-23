Để hiểu tại sao cần VLAN (Virtual Local Area Network), bạn hãy tưởng tượng một công ty có 3 phòng ban: Kế toán, Kinh doanh và Kỹ thuật ngồi chung trong một văn phòng lớn.

Nếu không có VLAN (Mạng phẳng - Flat Network), mọi người đều nghe thấy tiếng ồn của nhau. Nếu một người "hét lên" (Broadcast), tất cả mọi người đều bị làm phiền.

**VLAN sinh ra để giải quyết vấn đề "lộn xộn" đó bằng cách chia nhỏ mạng vật lý thành nhiều mạng logic riêng biệt.**

Dưới đây là 4 lý do chính và ưu điểm khi sử dụng VLAN:

![Hình ảnh về VLAN network segmentation diagram](https://encrypted-tbn3.gstatic.com/licensed-image?q=tbn:ANd9GcTbWUw1uqE52Td97dviXmdS2QWuUozKCiG_IM65kHIVDQfd9fyjeIDnJJytOKl8UyqhPOK9MZhEX2FOoTNSzSMX8n3h_EjL71S8Ez7PVJ1ipafGVSM)

Shutterstock

### 1. Tăng hiệu năng mạng (Giảm Broadcast)

Đây là lý do kỹ thuật quan trọng nhất.

- **Vấn đề:** Trong mạng LAN thông thường, các gói tin quảng bá (Broadcast) như ARP, DHCP được gửi đến **tất cả** các cổng. Khi mạng lớn lên (hàng trăm máy), lượng "tiếng ồn" này rất lớn, làm nghẽn mạng và tốn tài nguyên xử lý của CPU các máy tính.
    
- **Giải pháp VLAN:** VLAN chia một mạng lớn thành các mạng nhỏ. Gói tin Broadcast của VLAN 10 sẽ chỉ nằm trong VLAN 10, không lan sang VLAN 20.
    
- **Kết quả:** Mạng chạy mượt hơn, băng thông được tiết kiệm cho dữ liệu quan trọng.
    

### 2. Tăng cường bảo mật (Security)

- **Vấn đề:** Nếu không có VLAN, một nhân viên thử việc cắm dây mạng vào Switch có thể "nhìn thấy" máy chủ chứa dữ liệu lương của phòng Kế toán. Hoặc nếu một máy bị nhiễm virus, nó có thể lan ra toàn công ty.
    
- **Giải pháp VLAN:** VLAN cô lập các nhóm này. Phòng Kế toán nằm trong một "bức tường kính" riêng. Người ở VLAN Kinh doanh (hay Wifi Guest) không thể truy cập dữ liệu của VLAN Kế toán dù họ cắm chung một Switch.
    
- **Kiểm soát:** Muốn đi từ VLAN này sang VLAN kia bắt buộc phải qua Router/Firewall, nơi bạn có thể đặt "trạm gác" (Access Control List) để chặn hoặc cho phép.
    

### 3. Tiết kiệm chi phí (Cost Efficiency)

- **Vấn đề:** Ngày xưa, để tách biệt mạng Kế toán và Kinh doanh, bạn phải mua 2 Switch vật lý riêng biệt.
    
- **Giải pháp VLAN:** Bạn chỉ cần mua **1 Switch** duy nhất. Bạn cấu hình cổng 1-10 cho Kế toán, cổng 11-20 cho Kinh doanh.
    
- **Kết quả:** Tận dụng tối đa hạ tầng thiết bị sẵn có, không cần mua thêm phần cứng rườm rà.
    

### 4. Quản lý linh hoạt (Flexibility)

- **Vấn đề:** Nếu mạng dựa trên vị trí vật lý, khi nhân viên Kế toán chuyển chỗ ngồi sang phòng Kinh doanh, bạn phải đi lại dây cáp rất cực khổ để họ vẫn giữ được kết nối vào mạng Kế toán.
    
- **Giải pháp VLAN:** Với VLAN, logic không phụ thuộc vật lý. Dù nhân viên Kế toán ngồi ở tầng 1 hay tầng 5, bạn chỉ cần cấu hình cổng mạng ở chỗ ngồi mới đó thuộc `VLAN Kế toán` là xong. Họ giữ nguyên IP, nguyên quyền truy cập mà không cần đi lại dây.
    

---

### Tóm tắt dễ nhớ:

Hãy coi **Switch** là một tòa nhà chung cư:

- Không có VLAN: Tất cả các phòng thông nhau, ai cũng đi vào phòng người khác được.
    
- Có VLAN: Mỗi phòng (VLAN) có khóa riêng. Người ở phòng 101 không thể sang phòng 102 trừ khi đi ra hành lang chung (Router) và có chìa khóa (Permission).
    

Trong mô hình doanh nghiệp của bạn, bạn có dự định tách riêng mạng **Wifi cho khách (Guest)** và **Mạng nội bộ** không? Đó là ứng dụng phổ biến nhất của VLAN đấy.