

### 1. Phân tích Mục tiêu Kỹ thuật (Trong Thiết kế Mạng Top-Down)

Giai đoạn này tập trung vào việc chuyển đổi các yêu cầu kinh doanh (từ Chương 1) thành các thông số kỹ thuật cụ thể để lựa chọn công nghệ phù hợp.

**a. Các mục tiêu kỹ thuật chính**

- **Tính mở rộng (Scalability):** Khả năng mạng phát triển (thêm người dùng, ứng dụng, vị trí mới) mà không làm giảm hiệu suất. Cần xác định tốc độ tăng trưởng trong 1-2 năm tới.
- **Tính sẵn sàng (Availability):**
    - Là tỷ lệ thời gian mạng hoạt động bình thường.
    - Công thức: $Availability = \frac{MTBF}{MTBF + MTTR}$.
    - _MTBF (Mean Time Between Failure):_ Thời gian trung bình giữa các lần hỏng hóc.
    - _MTTR (Mean Time To Repair):_ Thời gian trung bình để sửa chữa.
    - Độ sẵn sàng cao thường được mong đợi là "Five Nines" (99.999%).
- **Hiệu năng mạng (Network Performance):** Bao gồm các yếu tố:
    - _Băng thông (Bandwidth):_ Dung lượng lý thuyết.
    - _Thông lượng (Throughput):_ Lượng dữ liệu thực tế truyền được không lỗi.
    - _Độ trễ (Delay/Latency):_ Thời gian truyền tin (gồm trễ lan truyền, trễ xử lý, trễ hàng đợi).
    - _Jitter:_ Sự biến thiên của độ trễ, ảnh hưởng lớn đến Voice/Video.
- **Bảo mật (Security):** Xác định tài sản cần bảo vệ (phần cứng, phần mềm, dữ liệu) và phân tích rủi ro (tấn công từ chối dịch vụ, mất dữ liệu).
- **Tính quản lý (Manageability):** Khả năng giám sát lỗi, cấu hình, hiệu năng và bảo mật (mô hình FCAPS).
- **Khả năng chi trả (Affordability):** Đảm bảo thiết kế phù hợp ngân sách, đôi khi cần tận dụng lại thiết bị cũ hoặc công nghệ giá rẻ hơn (ví dụ: dùng dây đồng thay vì cáp quang ở cự ly ngắn).

**b. Sự đánh đổi (Tradeoffs)**

- Không thể đạt được tất cả mục tiêu cùng lúc. Ví dụ: Tăng tính sẵn sàng (dự phòng) sẽ làm giảm khả năng chi trả (tăng chi phí); Tăng bảo mật quá mức có thể giảm tính dễ sử dụng (Usability) hoặc hiệu năng.
- Cần ưu tiên các mục tiêu dựa trên ngân sách (ví dụ: 100 điểm để phân bổ cho các mục tiêu).

---

### 2. Dự phòng mạng LAN & Giao thức Spanning Tree (Trong Kỹ thuật/Cấu hình)

Nội dung này tập trung vào việc thiết kế mạng có tính dự phòng (Redundancy) để tăng tính sẵn sàng, và cách xử lý các vấn đề phát sinh (vòng lặp) do dự phòng gây ra.

**a. Vấn đề của dự phòng Layer 2 (L2 Loops)** Khi có nhiều đường kết nối giữa các switch để dự phòng, các vòng lặp vật lý có thể gây ra 3 vấn đề nghiêm trọng nếu không có giao thức kiểm soát:

1. **Bão Broadcast (Broadcast Storms):** Các gói tin broadcast chạy vòng tròn vô tận, tiêu tốn băng thông và làm treo CPU của switch.
2. **Mất ổn định bảng MAC (MAC Database Instability):** Switch nhận cùng một khung tin (frame) từ nhiều cổng khác nhau, làm sai lệch bảng chuyển mạch liên tục.
3. **Trùng lặp khung tin (Duplicate Unicast Frames):** Máy đích nhận được nhiều bản sao của cùng một gói tin, gây lỗi cho ứng dụng.

**b. Giao thức Spanning Tree (STP - 802.1D)** STP giải quyết vấn đề vòng lặp bằng cách khóa logic (block) các đường dẫn dư thừa, chỉ giữ lại một đường đi duy nhất tại một thời điểm.

- **Quy trình hoạt động của STP:**
    1. **Bầu chọn Root Bridge:** Switch có **Bridge ID thấp nhất** sẽ làm Root. Bridge ID = Priority (mặc định 32768) + MAC Address.
    2. **Chọn Root Port:** Trên mỗi switch không phải Root, chọn 1 cổng có chi phí (cost) thấp nhất để đến Root Bridge.
    3. **Chọn Designated Port:** Trên mỗi đoạn mạng (segment), chọn 1 cổng để chuyển tiếp dữ liệu.
    4. **Block các cổng còn lại:** Các cổng không phải Root Port hay Designated Port sẽ bị khóa (Blocking/Discarding) để ngắt vòng lặp.

**c. Các biến thể của STP**

- **PVST+ (Per-VLAN Spanning Tree Plus):** Chuẩn của Cisco, chạy một tiến trình STP riêng cho mỗi VLAN. Cho phép cân bằng tải (VLAN 10 đi đường A, VLAN 20 đi đường B).
- **RSTP (Rapid STP - 802.1w):** Tốc độ hội tụ nhanh hơn nhiều so với STP truyền thống (vài mili-giây so với 50 giây). Các trạng thái cổng rút gọn còn: Discarding, Learning, Forwarding.
- **MSTP (Multiple STP - 802.1s):** Gom nhiều VLAN vào một instance STP để giảm tải CPU cho switch.

**d. Các tính năng bảo vệ STP (STP Security)**

- **PortFast:** Cấu hình trên cổng nối với máy tính/người dùng cuối, giúp cổng chuyển ngay sang trạng thái Forwarding mà không cần chờ hội tụ, tránh gây timeout cho DHCP.
- **BPDU Guard:** Đi kèm với PortFast. Nếu cổng PortFast nhận được gói tin BPDU (tức là có switch lạ cắm vào), cổng sẽ tự động tắt (err-disable) để ngăn vòng lặp.

**e. Giao thức dự phòng Gateway (First Hop Redundancy Protocols - FHRP)** Để đảm bảo máy tính luôn ra được Internet khi Router/Gateway chính bị hỏng:

- **HSRP (Hot Standby Router Protocol):** Chuẩn của Cisco. Một Router Active, một Router Standby. Chia sẻ cùng một địa chỉ IP ảo và MAC ảo.
- **VRRP (Virtual Router Redundancy Protocol):** Chuẩn mở, tương tự HSRP.
- **GLBP (Gateway Load Balancing Protocol):** Cho phép cân bằng tải traffic ra Internet trên nhiều router cùng lúc.

---

# Hướng Dẫn Cấu Hình Spanning Tree Protocol (STP)

Tài liệu tập trung vào việc cấu hình **PVST+** (Per-VLAN Spanning Tree Plus) - giao thức mặc định trên Cisco Catalyst, và **Rapid PVST+** (RSTP).

## 1. Cấu hình PVST+ (Mặc định và Tùy chỉnh Root Bridge)

Trên Cisco Switch, chế độ mặc định là PVST+. Để tối ưu hóa mạng, bước quan trọng nhất là cấu hình **Root Bridge** thay vì để Switch tự bầu chọn (thường dựa trên địa chỉ MAC thấp nhất nếu độ ưu tiên mặc định bằng nhau),.

### Cách 1: Sử dụng lệnh Macro (Khuyên dùng)

Lệnh này giúp Switch tự động tính toán giá trị Priority thấp hơn giá trị hiện tại để trở thành Root.

- **Cấu hình Switch làm Root Bridge chính (Primary):**
    
    ```
    Switch(config)# spanning-tree vlan <vlan-id> root primary
    ```
    
    _(Lệnh này ép Switch trở thành Root cho VLAN cụ thể)_.
    
- **Cấu hình Switch làm Root Bridge dự phòng (Secondary):**
    
    ```
    Switch(config)# spanning-tree vlan <vlan-id> root secondary
    ```
    
    _(Lệnh này đặt Priority ở mức thấp thứ hai, phòng khi Root chính bị lỗi)_.
    

### Cách 2: Cấu hình độ ưu tiên (Priority) thủ công

Bạn có thể thiết lập giá trị cụ thể cho Priority (giá trị mặc định là 32768). Giá trị càng thấp thì khả năng làm Root càng cao.

```
Switch(config)# spanning-tree vlan <vlan-id> priority <value>
```

_Ví dụ:_ `spanning-tree vlan 1 priority 24576`,. _Lưu ý: Giá trị priority phải là bội số của 4096 (ví dụ: 4096, 8192, ...)._

---

## 2. Cấu hình Rapid PVST+ (RSTP)

Rapid PVST+ cung cấp thời gian hội tụ nhanh hơn nhiều so với STP truyền thống. Để chuyển đổi chế độ STP sang Rapid PVST+, thực hiện như sau:

**Bước 1: Chuyển chế độ Spanning-tree**

```
Switch(config)# spanning-tree mode rapid-pvst
```

**Bước 2: Cấu hình loại liên kết (Link Type) trên Interface** Đối với các đường nối giữa các Switch (thường là Full-duplex), RSTP coi chúng là point-to-point để hội tụ nhanh. Nếu cần chỉ định rõ:

```
Switch(config)# interface <interface-id>
Switch(config-if)# spanning-tree link-type point-to-point
Switch(config-if)# end
```

_Lệnh `clear spanning-tree detected-protocols` có thể được dùng để xóa các giao thức STP đã phát hiện trước đó để bắt đầu lại quá trình đàm phán_.

---

## 3. Các tính năng bảo vệ và tối ưu STP (PortFast & BPDU Guard)

Đây là các cấu hình quan trọng cho các cổng kết nối với thiết bị đầu cuối (PC, Laptop) để bỏ qua các trạng thái chờ (Listening/Learning) và ngăn chặn vòng lặp do người dùng cắm nhầm thiết bị lạ.

### Cấu hình PortFast

Giúp cổng chuyển ngay sang trạng thái Forwarding khi kết nối.

```
Switch(config)# interface <interface-id>
Switch(config-if)# spanning-tree portfast
```

_Cảnh báo: Chỉ nên bật trên các cổng nối với thiết bị đầu cuối (single host), không bật trên cổng nối với Hub hoặc Switch khác_.

### Cấu hình BPDU Guard

Nếu cổng PortFast nhận được gói tin BPDU (tức là có Switch lạ cắm vào), cổng sẽ tự động bị tắt (error-disabled) để bảo vệ mạng.

```
Switch(config-if)# spanning-tree bpduguard enable
```

---

## 4. Các lệnh kiểm tra (Verification)

Sau khi cấu hình, tài liệu đề cập đến các lệnh sau để kiểm tra trạng thái STP,:

- **Xem thông tin STP tổng quan:**
    
    ```
    show spanning-tree
    ```
    
    _Lệnh này cho biết Switch nào là Root, Priority hiện tại, và trạng thái các cổng (Role, Sts)_.
    
- **Xem thông tin STP của một VLAN cụ thể:**
    
    ```
    show spanning-tree vlan <vlan-id>
    ```
    
- **Xem tóm tắt cấu hình đang chạy:**
    
    ```
    show running-config
    ```
    

### Ví dụ về kết quả hiển thị (Output) quan trọng:

Khi gõ `show spanning-tree`, nếu Switch hiện tại là Root, bạn sẽ thấy dòng thông báo:

> `This bridge is the root`.

Thông tin về **Port Role** (Vai trò cổng) và **Port State** (Trạng thái cổng) cũng được hiển thị, ví dụ:

- **Role:** Desg (Designated), Root, Altn (Alternate).
- **Sts (Status):** FWD (Forwarding), BLK (Blocking).

### Tóm lại

Kiến thức Chương 2 cung cấp nền tảng để:

1. **Thiết kế (Design):** Xác định rõ các chỉ số kỹ thuật cần đạt được (Availability, Scalability...).
2. **Kỹ thuật (Technical):** Hiểu cơ chế chống vòng lặp (STP) và cơ chế dự phòng gateway (HSRP) để xây dựng hệ thống mạng ổn định, tin cậy cao.
---

## 💡 Tổng Hợp Kiến Thức Trọng Tâm

### 1. Phân tích Mục tiêu Kỹ thuật (Technical Goals)
Thiết kế mạng là nghệ thuật của sự **đánh đổi (Trade-offs)**. Bạn cần nhớ 4 chỉ số vàng:
* **Scalability:** Khả năng mở rộng (thêm node/user mà không thay đổi kiến trúc cốt lõi).
* **Availability:** Độ sẵn sàng ($A = \frac{MTBF}{MTBF + MTTR}$).
* **Performance:** Hiệu năng (không chỉ là tốc độ mà còn là độ trễ và sự ổn định).
* **Affordability:** Khả năng chi trả (cân bằng giữa tính năng và túi tiền).

### 2. Spanning Tree Protocol (STP)
STP là "người hùng" thầm lặng trong Layer 2, giúp chúng ta có đường link dự phòng nhưng không làm chết mạng do vòng lặp.
* **Cơ chế:** Khóa (Block) các cổng dư thừa để tạo ra một cấu trúc cây (Tree) duy nhất.
* **Root Bridge:** "Gốc" của cây mạng, là điểm mốc để tính toán mọi đường đi.


---

## Phần 1: Nhận biết (Knowledge)

**Câu 1: Công thức tính độ sẵn sàng (Availability) dựa trên MTBF và MTTR là gì?**
A. $MTTR / (MTBF + MTTR)$
B. $MTBF / (MTBF + MTTR)$
C. $MTBF / MTTR$
D. $(MTBF + MTTR) / MTBF$

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Availability là tỷ lệ thời gian hệ thống "sống" (MTBF) trên tổng thời gian vòng đời (thời gian sống + thời gian chết để sửa chữa MTTR).

**Câu 2: Trong STP, Switch nào sẽ được bầu làm Root Bridge?**
A. Switch có địa chỉ MAC cao nhất.
B. Switch có Bridge ID thấp nhất.
C. Switch có nhiều cổng nhất.
D. Switch có Priority cao nhất.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Bridge ID = Priority + MAC. STP ưu tiên giá trị nhỏ nhất. Nếu Priority bằng nhau mới xét đến MAC nhỏ nhất.

**Câu 3: Thuật ngữ "Five Nines" trong tính sẵn sàng của mạng nghĩa là gì?**
A. Độ sẵn sàng đạt 99.999%.
B. Mạng có 5 thiết bị dự phòng.
C. Mạng hoạt động 9 giờ/ngày, 5 ngày/tuần.
D. Độ trễ dưới 5ms.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: A**
> **Giải thích:** Đây là tiêu chuẩn vàng của hạ tầng viễn thông, tương đương với thời gian chết (downtime) chỉ khoảng ~5 phút mỗi năm.

**Câu 4: Mục tiêu kỹ thuật "Scalability" đề cập đến điều gì?**
A. Khả năng bảo mật của mạng.
B. Khả năng mở rộng của mạng mà không ảnh hưởng hiệu suất.
C. Khả năng dễ dàng quản lý.
D. Khả năng chi trả của khách hàng.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Một mạng có tính Scalability tốt cho phép thêm 1000 users mà không phải mua lại Core Switch mới.

**Câu 5: Giao thức nào giúp ngăn chặn vòng lặp (loop) ở Layer 2?**
A. OSPF
B. DHCP
C. STP (Spanning Tree Protocol)
D. HSRP

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: C**
> **Giải thích:** OSPF là định tuyến (Layer 3). DHCP cấp IP. HSRP dự phòng Gateway. Chỉ có STP xử lý loop ở Layer 2.

**Câu 6: Trong mô hình quản lý mạng FCAPS, chữ "F" đại diện cho gì?**
A. File Management
B. Fault Management (Quản lý lỗi)
C. Fast Management
D. Future Management

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** FCAPS là: Fault (Lỗi), Configuration (Cấu hình), Accounting (Cước phí/Thống kê), Performance (Hiệu năng), Security (Bảo mật).

**Câu 7: Một cổng trên Switch được cấu hình PortFast sẽ chuyển sang trạng thái nào ngay lập tức khi kết nối?**
A. Blocking
B. Listening
C. Learning
D. Forwarding

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: D**
> **Giải thích:** PortFast bỏ qua các bước nghe/học của STP (mất ~30s) để cho phép thiết bị đầu cuối kết nối ngay lập tức.

**Câu 8: Hiện tượng "Broadcast Storm" xảy ra khi nào?**
A. Khi có quá nhiều router trong mạng.
B. Khi có vòng lặp Layer 2 khiến các gói tin broadcast truyền vô tận.
C. Khi dây cáp mạng bị đứt.
D. Khi cấu hình sai địa chỉ IP.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Vì Frame Layer 2 không có chỉ số TTL (Time to Live) như gói tin IP, nên nếu có vòng lặp, chúng sẽ chạy mãi mãi làm treo Switch.

**Câu 9: Bridge Priority mặc định trên các Switch Cisco là bao nhiêu?**
A. 1
B. 100
C. 32768
D. 65535

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: C**
> **Giải thích:** Đây là giá trị mặc định của nhà sản xuất. Để ép một Switch làm Root, ta thường hạ số này xuống (ví dụ 4096).

**Câu 10: Loại cáp nào được khuyến nghị để kết nối các tòa nhà (Building) trong mạng Campus để tránh nhiễu điện từ?**
A. Cáp đồng trục (Coaxial)
B. Cáp quang (Fiber Optic)
C. Cáp xoắn đôi UTP Cat5e
D. Cáp xoắn đôi STP

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Cáp quang truyền bằng ánh sáng, không dẫn điện nên miễn nhiễm hoàn toàn với nhiễu điện và sét lan truyền giữa các tòa nhà.

---

## Phần 2: Hiểu (Comprehension)

**Câu 11: Tại sao việc hiểu "Sự đánh đổi" (Tradeoffs) lại quan trọng trong thiết kế mạng?**
A. Vì khách hàng luôn có ngân sách vô hạn.
B. Vì các mục tiêu kỹ thuật thường mâu thuẫn nhau.
C. Vì thiết bị mạng không bao giờ hỏng.
D. Vì không cần thiết kế mạng.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Ví dụ: Muốn bảo mật cực cao (Security) thì người dùng phải qua nhiều bước xác thực, dẫn đến giảm tính tiện dụng (Usability).

**Câu 12: Sự khác biệt giữa Băng thông (Bandwidth) và Thông lượng (Throughput) là gì?**
A. Chúng là một.
B. Băng thông là dung lượng lý thuyết, Thông lượng là lượng dữ liệu thực tế truyền được.
C. Thông lượng luôn cao hơn Băng thông.
D. Băng thông dùng cho WAN, Thông lượng dùng cho LAN.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Băng thông là "độ rộng mặt đường", Thông lượng là "lượng xe thực tế lưu thông". Thông lượng luôn nhỏ hơn hoặc bằng băng thông.

**Câu 13: Tại sao cần sử dụng BPDU Guard trên các cổng đã bật PortFast?**
A. Để tăng tốc độ mạng.
B. Để ngăn chặn việc vô tình cắm một Switch khác vào cổng đó gây ra loop.
C. Để cấp nguồn PoE.
D. Để định tuyến gói tin nhanh hơn.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Cổng PortFast dành cho PC. Nếu ai đó cắm Switch vào cổng này, BPDU Guard sẽ phát hiện BPDU và khóa cổng ngay lập tức để tránh Loop.

**Câu 14: Giá trị MTBF (Mean Time Between Failure) 4000 giờ có ý nghĩa gì?**
A. Mạng sẽ hỏng mỗi 4000 giờ một lần.
B. Thời gian trung bình giữa các lần gặp sự cố là 4000 giờ.
C. Mạng cần 4000 giờ để sửa chữa.
D. Mạng chỉ hoạt động được 4000 giờ rồi hỏng hoàn toàn.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** MTBF càng cao thì thiết bị càng bền bỉ và tin cậy.

**Câu 15: Trong STP, vai trò của cổng "Root Port" là gì?**
A. Cổng gửi dữ liệu ra khỏi Switch.
B. Cổng có đường đi ngắn nhất (cost thấp nhất) để đến Root Bridge.
C. Cổng kết nối với máy tính.
D. Cổng bị khóa.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Mỗi Non-Root Switch chỉ có duy nhất 1 Root Port để hướng về "gốc" của cây Spanning Tree.

**Câu 16: Vấn đề "MAC Database Instability" (Mất ổn định bảng MAC) xảy ra như thế nào trong mạng có loop?**
A. Switch không học được MAC nào.
B. Switch nhận cùng một khung tin từ nhiều cổng khác nhau, làm bảng MAC nhảy liên tục.
C. Switch bị đầy bộ nhớ.
D. Switch xóa toàn bộ bảng MAC.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Khi frame chạy vòng tròn, Switch thấy MAC nguồn AAA ở cổng 1, rồi lại thấy ở cổng 2, dẫn đến việc cập nhật bảng MAC liên tục (Flapping).

**Câu 17: PVST+ (Per-VLAN Spanning Tree Plus) có ưu điểm gì so với STP truyền thống (CST)?**
A. Nó chạy nhanh hơn.
B. Nó cho phép cân bằng tải bằng cách sử dụng các Root Bridge khác nhau cho các VLAN khác nhau.
C. Nó không cần cấu hình.
D. Nó loại bỏ hoàn toàn việc dùng BPDU.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** PVST+ cho phép link 1 là Active cho VLAN 10, link 2 là Active cho VLAN 20, tránh lãng phí băng thông của link bị Block.

**Câu 18: Khi thiết kế mạng Wireless, tại sao cần khảo sát thực địa (Site Survey)?**
A. Để biết chỗ nào mát mẻ để đặt thiết bị.
B. Để kiểm tra vật cản và nhiễu sóng ảnh hưởng đến vùng phủ sóng.
C. Để đếm số lượng người dùng.
D. Để chọn màu sắc cho Access Point.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Tường bê tông, lò vi sóng hay thiết bị Bluetooth đều có thể làm chết sóng Wifi, cần khảo sát để đặt AP đúng chỗ.

**Câu 19: Độ trễ lan truyền (Propagation delay) phụ thuộc vào yếu tố nào?**
A. Kích thước gói tin.
B. Tốc độ CPU của router.
C. Khoảng cách vật lý và tốc độ ánh sáng trong môi trường truyền dẫn.
D. Dung lượng hàng đợi.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: C**
> **Giải thích:** Đây là hằng số vật lý. Tín hiệu điện/quang cần thời gian để đi hết chiều dài sợi cáp.

**Câu 20: Tại sao nên thiết kế mạng theo mô hình phân cấp (Hierarchical)?**
A. Để mạng phức tạp hơn.
B. Để dễ dàng mở rộng, cô lập lỗi và quản lý chức năng riêng biệt.
C. Để bán được nhiều thiết bị hơn.
D. Để tăng số lượng Broadcast domain.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Mô hình Core - Distribution - Access giúp việc thay đổi ở một vùng không làm ảnh hưởng đến toàn bộ hệ thống.

---

## Phần 3: Vận dụng (Application)

**Câu 21: Một khách hàng yêu cầu mạng phải hoạt động 24/7 (uptime 99.999%). Bạn sẽ đề xuất giải pháp nào?**
A. Sử dụng thiết bị rẻ tiền để dễ thay thế.
B. Thiết kế dư thừa (Redundancy) toàn diện: Nguồn, Link, Thiết bị, HSRP.
C. Chỉ sử dụng một đường truyền Internet.
D. Không cần làm gì cả.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** 99.999% không cho phép bất kỳ điểm lỗi đơn lẻ (Single Point of Failure) nào tồn tại.

**Câu 22: Bạn có 3 Switch nối vòng tròn. SW1 (Priority 32768, MAC AA), SW2 (Priority 32768, MAC BB), SW3 (Priority 4096, MAC CC). Switch nào sẽ là Root Bridge?**
A. SW1
B. SW2
C. SW3
D. Không xác định.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: C**
> **Giải thích:** Priority 4096 của SW3 nhỏ hơn giá trị mặc định 32768 của 2 Switch kia nên nó thắng tuyệt đối.

**Câu 23: Khách hàng phàn nàn mạng chậm. Bạn kiểm tra thấy Interface WAN có utilization > 90%. Đây là vấn đề về:**
A. Availability
B. Performance (Capacity)
C. Security
D. Usability

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Utilization > 90% nghĩa là nghẽn cổ chai (Congestion), đường truyền không đủ sức tải (Capacity).

**Câu 24: Công ty muốn tách biệt lưu lượng Wifi Khách và Wifi Nội bộ. Bạn sẽ làm gì?**
A. Mua 2 đường truyền Internet riêng.
B. Cấu hình SSID khác nhau gán vào các VLAN khác nhau và dùng ACL chặn.
C. Đặt mật khẩu Wifi thật khó.
D. Cấm khách dùng Wifi.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Đây là phương pháp Segmentation (phân đoạn) cơ bản bằng VLAN để bảo mật thông tin nội bộ.

**Câu 25: Để tối ưu hóa STP, bạn muốn SW1 làm Root cho VLAN 10 và SW2 làm Root cho VLAN 20. Bạn cần cấu hình gì?**
A. Tăng Priority của SW1 cho VLAN 10.
B. Giảm Priority của SW1 cho VLAN 10 và giảm Priority của SW2 cho VLAN 20.
C. Chỉnh cost của các cổng.
D. Tắt STP.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Hạ thấp Priority là cách chuẩn nhất để chỉ định một Switch cụ thể đóng vai trò Root Bridge.

**Câu 26: Thiết kế mạng cho một tòa nhà cũ không có trần giả để đi dây. Ràng buộc này thuộc loại nào?**
A. Ràng buộc về ngân sách.
B. Ràng buộc về chính trị.
C. Ràng buộc về kiến trúc/môi trường (Architectural/Environmental).
D. Ràng buộc về thời gian.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: C**
> **Giải thích:** Các giới hạn về kết cấu hạ tầng sẵn có của công trình được xếp vào ràng buộc môi trường/kiến trúc.

**Câu 27: Yếu tố nào sau đây ảnh hưởng tiêu cực nhất đến chất lượng thoại VoIP?**
A. Băng thông lớn.
B. Jitter (Biến thiên độ trễ) cao.
C. Mã hóa dữ liệu.
D. Sử dụng cáp quang.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Jitter làm các gói tin thoại đến không đều, gây hiện tượng tiếng bị giật, robot hoặc mất tiếng dù băng thông có thể vẫn đủ.

**Câu 28: Switch A nối với Switch B bằng 2 dây cáp. Để tăng băng thông mà không bị STP block, bạn nên dùng:**
A. VLAN
B. HSRP
C. EtherChannel (Link Aggregation)
D. OSPF

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: C**
> **Giải thích:** EtherChannel gộp nhiều dây vật lý thành 1 ống logic, STP sẽ chỉ nhìn thấy 1 link duy nhất nên không block dây nào cả.

**Câu 29: Khi thiết kế mạng cho bệnh viện, ưu tiên hàng đầu là gì?**
A. Chi phí thấp nhất.
B. Tính sẵn sàng (Availability) và độ tin cậy.
C. Tốc độ truy cập Facebook nhanh.
D. Sử dụng thiết bị mới nhất.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Trong y tế, mạng sập đồng nghĩa với việc không thể truy cập bệnh án điện tử, ảnh hưởng trực tiếp đến tính mạng.

**Câu 30: Bạn phát hiện Switch Core đang quá tải CPU do xử lý quá nhiều gói tin broadcast. Giải pháp tận gốc là?**
A. Mua Switch mạnh hơn.
B. Chia nhỏ Broadcast domain bằng VLANs và Layer 3 Switch.
C. Chặn tất cả broadcast.
D. Rút bớt dây mạng.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Router/L3 Switch không chuyển tiếp Broadcast. Việc chia nhỏ VLAN giúp giới hạn "vùng ảnh hưởng" của gói tin broadcast.

---

## Phần 4: Vận dụng cao (High Application)

**Câu 31: Doanh nghiệp muốn triển khai Video Conference. Bạn sẽ ưu tiên lưu lượng này như thế nào?**
A. Gán vào hàng đợi thấp nhất.
B. Gán vào hàng đợi ưu tiên cao (Priority Queue) để giảm delay/jitter.
C. Chỉ cần tăng băng thông Internet là đủ.
D. Sử dụng TCP để đảm bảo tin cậy.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Video là Real-time traffic. Nó cần được "nhảy hàng" xử lý trước (Priority) để tránh bị trễ.

**Câu 32: Trong thiết kế Redundancy, nếu sử dụng HSRP, điều gì xảy ra khi Router Active hỏng?**
A. Mạng bị mất kết nối hoàn toàn.
B. Người dùng phải tự đổi địa chỉ Gateway.
C. Router Standby tự động nhận vai trò Active bằng Virtual IP.
D. Phải khởi động lại Switch.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: C**
> **Giải thích:** HSRP giúp chuyển đổi Router dự phòng chỉ trong vài giây mà người dùng không hề hay biết.

**Câu 33: Hệ thống mạng có MTBF = 2000 giờ và MTTR = 2 giờ. Tính độ sẵn sàng?**
A. 99.9%
B. 99.0%
C. ~99.9% ($2000 / 2002$)
D. 95%

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: C**
> **Giải thích:** $A = 2000 / (2000 + 2) = 0.99900099...$ (tức 99.9%).

**Câu 34: Kết nối tối ưu cho các bungalow rải rác cách xa nhau trong Resort rộng lớn?**
A. Kéo dây cáp đồng (Copper).
B. Sử dụng cáp quang (Fiber) hoặc Wireless Point-to-Multipoint.
C. Sử dụng Hub để khuếch đại tín hiệu.
D. Dùng sóng siêu âm.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Cáp đồng chỉ đi được tối đa 100m. Khoảng cách lớn trong Resort buộc phải dùng cáp quang hoặc sóng vô tuyến công suất cao.

**Câu 35: Tại sao cần phân tích "Tradeoffs" giữa Security và Performance?**
A. Vì Security làm máy tính chạy nhanh hơn.
B. Vì quá trình mã hóa/giải mã và kiểm tra ACL tốn tài nguyên CPU và gây thêm trễ.
C. Vì Security không quan trọng.
D. Vì Performance không quan trọng.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Càng nhiều lớp bảo mật (Deep Packet Inspection, VPN Encryption), CPU càng phải làm việc nhiều, dẫn đến throughput giảm.

**Câu 36: Nếu tất cả các Switch đều để Priority mặc định, yếu tố nào quyết định Root Bridge?**
A. Switch nào khởi động trước.
B. Switch có địa chỉ MAC nhỏ nhất.
C. Switch có địa chỉ MAC lớn nhất.
D. Switch có nhiều cổng nhất.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Đây là quy tắc "Tie-breaker" của STP khi Priority hòa nhau.

**Câu 37: Thiết kế mạng cho sàn giao dịch chứng khoán. Mục tiêu quan trọng nhất là?**
A. Affordability.
B. Low Latency (Độ trễ thấp) và High Availability.
C. Scalability.
D. Manageability.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Trong chứng khoán, chậm 1 mili giây là mất tiền tỷ. Do đó trễ cực thấp và luôn luôn sống là yếu tố sống còn.

**Câu 38: Tại sao nên hạn chế kích thước của một Broadcast domain?**
A. Để tiết kiệm địa chỉ IP.
B. Để giảm thiểu ảnh hưởng của broadcast storm lên CPU của mọi thiết bị.
C. Để dễ vẽ sơ đồ.
D. Để dùng ít Switch hơn.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Mỗi gói tin broadcast bắt buộc mọi PC trong domain đó phải dừng việc đang làm để xử lý gói tin, gây lãng phí CPU toàn hệ thống.

**Câu 39: Để ngăn người dùng tự cắm Switch lạ vào cổng Access làm thay đổi topology STP, ta dùng?**
A. BPDU Filter
B. BPDU Guard
C. Root Guard
D. Loop Guard

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** BPDU Guard sẽ đặt cổng vào trạng thái "Err-disable" ngay khi thấy 1 gói BPDU đi vào cổng vốn chỉ dành cho PC.

**Câu 40: Một thiết kế mạng tốt theo hướng Top-Down cần bắt đầu từ đâu?**
A. Mua thiết bị Cisco.
B. Phân tích yêu cầu ứng dụng và mục tiêu kinh doanh.
C. Đi dây cáp.
D. Cấu hình IP.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Triết lý Top-Down: Business -> Application -> Data -> Infrastructure.

---

## Phần 5: Hiểu sâu / Thực tiễn

**Câu 41: Trong dự án KTX (The Muse), việc sử dụng Wall-plate AP thay vì AP hành lang giải quyết vấn đề gì?**
A. Affordability.
B. Performance - Giảm suy hao tín hiệu qua tường và tăng trải nghiệm người dùng.
C. Manageability.
D. Scalability.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Tường KTX thường dày, sóng Wifi xuyên qua sẽ yếu đi rất nhiều. Đặt AP ngay trong phòng giúp băng thông đạt mức tối đa.

**Câu 42: Tại sao FastPay lại cần kênh truyền riêng (Leased Line) thay vì FTTH dân dụng?**
A. Vì công ty nhiều tiền.
B. Vì Leased Line có cam kết băng thông và độ ổn định (SLA) khắt khe cho giao dịch.
C. Vì Leased Line dễ lắp đặt hơn.
D. Vì FTTH không bảo mật.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** FTTH (cáp quang gia đình) là băng thông chia sẻ, không có cam kết chất lượng khi đứt cáp quang biển. Leased Line có hợp đồng cam kết uptime 99.9%.

**Câu 43: Lệnh `standby 1 priority 150` trên Router A và `priority 100` trên Router B nghĩa là?**
A. Router B sẽ là Active.
B. Router A sẽ là Active vì Priority cao hơn.
C. Cả 2 cùng Active.
D. Không có ý nghĩa gì.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Trong HSRP, Priority cao hơn sẽ giành quyền kiểm soát đường truyền chính (Active).

**Câu 44: Việc tách VLAN Kế toán và VLAN IT phục vụ mục tiêu gì trong bảo mật?**
A. Availability.
B. Confidentiality (Tính bí mật) - Ngăn IT truy cập dữ liệu lương.
C. Integrity.
D. Scalability.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Confidentiality đảm bảo rằng chỉ những người có quyền mới được xem dữ liệu tương ứng.

**Câu 45: Tại sao Switch Access lại được nối với Core Switch bằng đường Trunk?**
A. Để tiết kiệm dây.
B. Để cho phép lưu lượng của nhiều VLAN cùng đi qua một kết nối vật lý.
C. Để tăng tốc độ.
D. Để cấp nguồn PoE.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Đường Trunk sử dụng tag 802.1Q để phân biệt gói tin thuộc VLAN nào khi truyền giữa các Switch.

**Câu 46: Cổng Uplink báo lỗi "Late Collision". Nguyên nhân là gì?**
A. Dây cáp quá dài (>100m) hoặc Duplex Mismatch.
B. Switch bị hỏng.
C. IP trùng nhau.
D. Chưa cấu hình VLAN.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: A**
> **Giải thích:** Late Collision thường xảy ra khi Switch gửi xong gói tin mới phát hiện có va chạm, do tín hiệu va chạm đi về quá chậm vì dây quá dài hoặc xung đột chế độ Full/Half Duplex.

**Câu 47: Zabbix dùng giao thức nào chủ yếu để lấy thông tin từ thiết bị mạng?**
A. FTP
B. SNMP (Simple Network Management Protocol).
C. SMTP.
D. HTTP.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** SNMP là giao thức chuẩn toàn cầu để các phần mềm giám sát (Zabbix, PRTG) "hỏi" trạng thái của thiết bị mạng.

**Câu 48: Tại sao VPN Site-to-Site lại cần IPsec?**
A. Để tăng tốc độ truyền.
B. Để mã hóa dữ liệu truyền qua Internet công cộng.
C. Để nén dữ liệu.
D. Để định tuyến tốt hơn.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Internet là môi trường mở. IPsec biến dữ liệu thành mật mã để kẻ xấu dù có bắt được gói tin cũng không đọc được nội dung.

**Câu 49: Lệnh `encapsulation dot1q <vlan-id>` trên sub-interface dùng để?**
A. Để Router biết sub-interface thuộc VLAN nào và xử lý tag 802.1Q.
B. Để bật cổng.
C. Để đặt địa chỉ IP.
D. Để chạy OSPF.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: A**
> **Giải thích:** Đây là câu lệnh bắt buộc trong mô hình Router-on-a-stick để Router định tuyến được giữa các VLAN.

**Câu 50: Sử dụng 2 đường truyền từ 2 nhà cung cấp (ISP) khác nhau gọi là gì?**
A. Single Homing.
B. Dual Homing (hoặc Multihoming).
C. Switching.
D. Routing.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Multihoming giúp doanh nghiệp vẫn giữ được kết nối Internet ngay cả khi một nhà mạng (ví dụ VNPT) bị đứt cáp.