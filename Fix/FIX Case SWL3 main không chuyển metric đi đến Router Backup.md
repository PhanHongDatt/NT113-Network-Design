Bảng định tuyến `show ip route` bạn vừa gửi đã **XÁC NHẬN CHÍNH XÁC** nguyên nhân tại sao VLAN không đi được qua Router Backup.

**Nguyên nhân:** Router Backup bị **"Mù"**. Nó hoàn toàn **không có bất kỳ dòng OSPF (`O`) nào**. Nó không biết `10.1.10.0`, `10.1.20.0`... nằm ở đâu cả.

- Khi gói tin từ VLAN đi đến Router: **Thành công** (Switch L3 đẩy sang).
    
- Khi Router muốn trả lời: Nó tra bảng định tuyến, không thấy địa chỉ đích -> **Hủy gói tin (Drop)**.
    

Dưới đây là 3 bước để khắc phục tình trạng này trên Packet Tracer:

### Bước 1: Kiểm tra cấu hình OSPF trên Router Backup

Hãy đảm bảo bạn đã khai báo đúng mạng nối với Switch L3. Vào `HoChiMinh(Backup)` và gõ:

Bash

```
conf t
router ospf 1
 ! Đây là mạng nối giữa Router Backup và Switch L3
 network 10.2.2.0 0.0.0.3 area 0
 ! Quảng bá luôn mạng nối Internet (để test)
 network 10.2.0.0 0.0.0.255 area 0
 end
```

### Bước 2: Kiểm tra cấu hình OSPF trên Switch L3 Main

Router Backup không học được Route có thể do Switch L3 "giấu" không chịu quảng bá. Vào `HCM-HQ-SWL3-Main` và đảm bảo có đủ các dòng sau (Dựa trên file config ):

Bash

```
conf t
router ospf 1
 ! Phải có dòng này thì Router Backup mới nói chuyện được với Switch
 network 10.2.2.0 0.0.0.3 area 0
 
 ! Phải có các dòng này thì Router mới biết VLAN nằm ở đâu
 network 10.1.10.0 0.0.0.255 area 0
 network 10.1.20.0 0.0.0.255 area 0
 network 10.1.30.0 0.0.0.255 area 0
 network 10.1.40.0 0.0.0.255 area 0
 network 10.1.50.0 0.0.0.255 area 0
 network 10.1.60.0 0.0.0.255 area 0
 network 10.1.70.0 0.0.0.255 area 0
 end
```

### Bước 3: Tuyệt chiêu cuối (Reset OSPF)

Nếu cấu hình đã đúng y hệt như trên mà bảng định tuyến vẫn trống trơn (Bệnh kinh điển của Packet Tracer), bạn hãy làm hành động này để ép chúng đồng bộ lại:

**Trên Router Backup:**

Bash

```
clear ip ospf process
! Nhấn 'yes'
```

**Trên Switch L3 Main:**

Bash

```
clear ip ospf process
! Nhấn 'yes'
```

---

### Kết quả mong đợi

Sau khi làm xong, bạn gõ lại `show ip route` trên Router Backup. Bạn **BẮT BUỘC** phải thấy kết quả tương tự như sau thì hệ thống mới chạy được:

Plaintext

```
Gateway of last resort is 0.0.0.0 to network 0.0.0.0
...
O     10.1.10.0/24 [110/2] via 10.2.2.2, GigabitEthernet0/1
O     10.1.20.0/24 [110/2] via 10.2.2.2, GigabitEthernet0/1
...
S* 0.0.0.0/0 [1/0] via 10.2.0.1
```

Khi nào thấy chữ **`O`** xuất hiện, lúc đó bạn ping từ VLAN sẽ thông ngay lập tức!