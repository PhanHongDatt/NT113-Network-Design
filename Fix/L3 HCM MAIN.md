+ Một số lỗi còn tồn đọng và cần được fix trong L3
# 4. ĐỀ XUẤT CẢI TIẾN (dựa trên đủ dữ liệu)

## ✔ Sửa lỗi OSPF
Mạng này bị sai wild card vì theo cấu hình là GigabitEthernet1/0/1 → 10.1.1.2/30
suy ra wild card là 0.0.0.3 

```
router ospf 1  
no network 10.1.1.0 0.0.0.255 area 0  
network 10.1.1.0 0.0.0.3 area 0
```

## ✔ Thêm HSRP tracking uplink
Cần phải thêm nếu không trường hợp đứt cáp => L3 vẫn Active nhưng mạng không hoạt động => Không chuyển qua Standby để hoạt động được
```
interface Vlan10 
standby 10 track g1/0/1 decrement 30
```

Vì CISCO KHÔNG hỗ trợ câu lệnh decrement => Buộc phải giảm priority xuống để khi mặc định có vấn đề xảy ra => 105 -10 =95 thấp hơn Standby 
conf t
```
! --- VLAN 10 (Kế toán) ---
interface Vlan10
 ! Ha Priority tu 150 xuong 105
 standby 10 priority 105
 ! Track cong uplink 1 (mac dinh tru 10)
 standby 10 track GigabitEthernet 1/0/1
 ! Track cong uplink 2 (mac dinh tru 10)
 standby 10 track GigabitEthernet 1/0/2
 exit

! --- VLAN 20 (Quản lý rủi ro) ---
interface Vlan20
 standby 20 priority 105
 standby 20 track GigabitEthernet 1/0/1
 standby 20 track GigabitEthernet 1/0/2
 exit

! --- VLAN 30 (IT) ---
interface Vlan30
 standby 30 priority 105
 standby 30 track GigabitEthernet 1/0/1
 standby 30 track GigabitEthernet 1/0/2
 exit

! --- VLAN 40 (Wifi Company) ---
interface Vlan40
 standby 40 priority 105
 standby 40 track GigabitEthernet 1/0/1
 standby 40 track GigabitEthernet 1/0/2
 exit

! --- VLAN 50 (Wifi Guest) ---
interface Vlan50
 standby 50 priority 105
 standby 50 track GigabitEthernet 1/0/1
 standby 50 track GigabitEthernet 1/0/2
 exit

! --- VLAN 60 (Network Management) ---
interface Vlan60
 standby 60 priority 105
 standby 60 track GigabitEthernet 1/0/1
 standby 60 track GigabitEthernet 1/0/2
 exit

! --- VLAN 70 (Data Center) ---
interface Vlan70
 standby 70 priority 105
 standby 70 track GigabitEthernet 1/0/1
 standby 70 track GigabitEthernet 1/0/2
 end

! Lưu cấu hình
write

```


## Tiến hành kiểm tra sau khi đã thực hiện cấu hình
=> **Kết quả kiểm tra trên L3 MAIN**

```
HCM-HQ-SWL3-Main#show standby brief

P indicates configured to preempt.

|

Interface Grp Pri P State Active Standby Virtual IP

Vl10 10 105 P Active local 10.1.10.3 10.1.10.1

Vl20 20 105 P Active local 10.1.20.3 10.1.20.1

Vl30 30 105 P Active local 10.1.30.3 10.1.30.1

Vl40 40 105 P Active local 10.1.40.3 10.1.40.1

Vl50 50 105 P Active local 10.1.50.3 10.1.50.1

Vl60 60 105 P Active local 10.1.60.3 10.1.60.1

Vl70 70 105 P Active local 10.1.70.3 10.1.70.1
```

Kết quả mong đợi: Chuyển từ ACTIVE sang Standby sau khi ngắt kết nối cổng inf g1/0/1
conf t interface GigabitEthernet 1/0/1 shutdown end
## ✔ Tắt Telnet → Bật SSH
Cần tắt telnet vì vấn đề bảo mật, nếu để telnet thì toàn bộ lưu lượng được gửi đi dưới dàn plantext gây nguy hiểm cho hệ thống mạng
Mật khẩu dạng type 7 dễ giải mã => Không đáp ứng được nhu cầu doanh nghiệp
```
line vty 0 4 transport input ssh
```
	
## ✔ Bật CEF
→ L3 switch **không thể hoạt động hiệu quả** khi routing mà không có CEF.  
→ CPU tăng, hiệu năng giảm mạnh.
```
ip cef
```
=> BỎ VÌ CISCO KHÔNG HỖ TRỢ IP CEF
## ✔ Thêm phân chia VLAN bằng ACL (tùy chính sách)


# Kiểm lỗi
Có — sau khi bạn đã sửa các lỗi (OSPF, HSRP, DHCP, SSH, STP…), **bạn hoàn toàn có thể kiểm tra mô hình hoạt động đúng hay chưa trong Cisco Packet Tracer** bằng một bộ quy trình kiểm thử chuẩn doanh nghiệp.

Tôi sẽ đưa bạn một **bộ checklist + câu lệnh kiểm tra + cách test thực nghiệm** để bạn xác nhận toàn bộ mô hình hoạt động chính xác.

---

# ✅ PHẦN 1 — KIỂM TRA DHCP

**Mục tiêu:**

- Client trong mỗi VLAN nhận IP đúng dải
    
- Gateway đúng
    
- DNS đúng
    
- DHCP failover hoạt động khi MAIN down
    

### ✔ Kiểm tra cấp IP

Trên PC của từng VLAN:

**PC → IP Configuration → DHCP**  
Kiểm tra IP:

- VLAN10 → 10.1.10.x
    
- VLAN20 → 10.1.20.x
    
- VLAN30 → 10.1.30.x
    
- VLAN40 → 10.1.40.x
    
- VLAN50 → 10.1.50.x (nếu có DHCP pool)
    
- VLAN60 → 10.1.60.x
    
- VLAN70 → 10.1.70.x
    

Nếu nhận được IP → DHCP OK.

### ✔ Kiểm tra default-gateway

Ping gateway:

`ping 10.1.10.1 ping 10.1.20.1 ...`

→ Thành công = HSRP hoạt động + DHCP đúng.

### ✔ Kiểm tra failover DHCP

Tắt SWL3 MAIN (Power Off trong PT)  
→ Các PC renew IP (ipconfig /renew trong PT hoặc disconnect cable):

`ipconfig /renew`

Nếu nhận được IP trong dải 121–254 → **Split-scope hoạt động đúng.**

---

# ✅ PHẦN 2 — KIỂM TRA HSRP

**Mục tiêu:**

- MAIN đang Active
    
- Backup ở Standby
    
- Đổi Active khi MAIN down
    
- Tracking hoạt động
    

### ✔ Kiểm tra trạng thái HSRP

Trên SWL3 MAIN:

`show standby`

Kết quả mong muốn:

- State: **Active**
    
- Virtual IP: đúng .1
    
- Priority: 150
    
- Preempt: enabled
    

Trên SWL3 BACKUP:

`show standby`

- State: **Standby**
    
- Priority: 100
    

### ✔ Test failover

1. Power off SWL3 MAIN
    
2. Trên BACKUP:
    

`show standby`

→ State chuyển từ **Standby → Active**

3. Ping gateway từ PC  
    → Phải tiếp tục hoạt động, không mất nhiều hơn 5–10s.
    

### ✔ Test tracking uplink

Rút dây uplink của SWL3 MAIN:

- Nếu cấu hình tracking đúng:  
    → Priority giảm  
    → MAIN nhường Active cho BACKUP  
    → Client đi ra WAN theo BACKUP
    

---

# ✅ PHẦN 3 — KIỂM TRA OSPF

**Mục tiêu:**

- Hình thành neighbor
    
- LAN route được quảng bá
    
- HCM ↔ Hà Nội ↔ Đà Nẵng đi được với nhau
    

### ✔ Kiểm tra neighbor

Trên SWL3 MAIN:

`show ip ospf neighbor`

Kết quả mong muốn:

- 1–2 neighbors (Router Main, hoặc SWL3 Backup, hoặc Router WAN)
    
- State FULL/ - FULL
    

### ✔ Kiểm tra LSDB

`show ip ospf database`

→ Phải thấy tất cả VLAN (10/20/30/40/50/60/70) trong LSDB.

### ✔ Kiểm tra bảng định tuyến

`show ip route ospf`

→ Phải xuất hiện đầy đủ các mạng remote từ Hà Nội & Đà Nẵng.

### ✔ Test thực tế

Trên PC VLAN10 (HCM):

`ping <PC ở Hà Nội> ping <PC ở Đà Nẵng>`

Nếu ping thành công → OSPF OK.

---

# ✅ PHẦN 4 — KIỂM TRA INTER-VLAN ROUTING

**Mục tiêu:**

- Mỗi VLAN ping được VLAN khác (nếu chưa đặt ACL)
    
- Kiểm tra xử lý routing của SWL3
    

Từ VLAN10 ping:

`ping 10.1.20.100 ping 10.1.30.100 ping 10.1.40.100`

→ Nếu thành công → L3 routing OK.

Nếu bạn đặt ACL để tách VLAN:  
→ Ping sẽ bị chặn theo đúng chính sách.

---

# ✅ PHẦN 5 — KIỂM TRA STP

**Mục tiêu:**

- SWL3 MAIN là root bridge
    
- Không có loop
    
- Port role đúng
    

### ✔ Lệnh kiểm tra

`show spanning-tree vlan 10`

Kết quả mong muốn:

- Root ID = MAC 000c.8504.b1xx
    
- This bridge is the root
    

Kiểm tra toàn bộ VLAN:

`show spanning-tree summary`

---

# ✅ PHẦN 6 — KIỂM TRA SECURITY (SSH/Telnet)

### ✔ Kiểm tra SSH

Từ PC (Command Prompt):

`ssh -l admin <IP SWL3 Main>`

Nếu kết nối SSH thành công → Security OK.

### ✔ Kiểm tra Telnet đã tắt

`telnet <IP SWL3 Main>`

=> Nếu “Connection refused” → OK.

---

# ✅ PHẦN 7 — KIỂM TRA HIỆU NĂNG (CEF)

Trong Packet Tracer, CEF không có tác dụng thật sự, nhưng:

`show ip cef`

→ Nếu bảng hiện ra → CEF đang bật (mô phỏng).

---

# 📌 KẾT LUẬN: TOÀN BỘ CÁC BÀI TEST CẦN THỰC HIỆN

| Thành phần | Cách kiểm tra                        | PT có hỗ trợ? |
| ---------- | ------------------------------------ | ------------- |
| DHCP       | cấp IP, renew, failover              | ✔             |
| HSRP       | show standby, failover, tracking     | ✔             |
| OSPF       | neighbor, routing table, ping remote | ✔             |
| Inter-VLAN | ping giữa VLAN                       | ✔             |
| STP        | show spanning-tree                   | ✔             |
| SSH        | thử kết nối SSH                      | ✔             |
| ACL        | test deny/permit                     | ✔             |
| NAT        | không thuộc L3 switch                | –             |
