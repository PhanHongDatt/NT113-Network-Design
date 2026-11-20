

### 1. Đặc tả cơ sở hạ tầng mạng (Characterizing the Network Infrastructure)

Bước đầu tiên là vẽ lại bản đồ mạng và thu thập thông tin chi tiết về cấu trúc hiện tại:

- **Cấu trúc mạng (Topology):** Cần xác định cả mô hình Logic (phân cấp, mô đun, topo mạng) và mô hình Vật lý (vị trí thiết bị, kết nối cáp),.
- **Mô hình đặt tên và địa chỉ (Addressing and Naming):** Thu thập sơ đồ địa chỉ IP hiện tại, các vấn đề về subnet (như subnet không liên tục), và quy tắc đặt tên thiết bị/mạng,.
- **Hệ thống dây cáp (Wiring and Media):** Xác định loại cáp (quang single-mode/multi-mode, cáp đồng UTP/STP), khoảng cách cáp, và cấu trúc đi dây trong tòa nhà (đứng/ngang),.

### 2. Kiểm tra các ràng buộc về Kiến trúc và Môi trường

Nhà thiết kế cần khảo sát thực tế để xác định các yếu tố vật lý ảnh hưởng đến việc lắp đặt thiết bị mới:

- **Cơ sở vật chất:** Kiểm tra hệ thống điều hòa, thông gió, nguồn điện, không gian tủ rack, và an ninh vật lý (cửa khóa) tại các phòng máy,.
- **Môi trường không dây (Wireless):** Nếu triển khai wifi, cần khảo sát các yếu tố gây nhiễu sóng hoặc suy hao tín hiệu như sự phản xạ (reflection), hấp thụ (absorption), khúc xạ (refraction) của tường và vật cản,.

### 3. Kiểm tra "Sức khỏe" của hệ thống mạng (Health Check)

Đây là phần quan trọng nhất để tạo ra một "baseline" (mức chuẩn) nhằm so sánh hiệu quả sau khi nâng cấp. Các yếu tố cần đo lường bao gồm:

- **Tính sẵn sàng (Availability):** Đo lường thời gian hoạt động (MTBF) và thời gian sửa chữa (MTTR), lịch sử các lần "sập mạng" lớn,.
- **Hiệu suất và Băng thông (Utilization):** Phân tích mức độ sử dụng băng thông theo thời gian thực (ví dụ: theo từng phút hoặc giờ) và theo giao thức (tương đối và tuyệt đối),.
- **Độ chính xác (Accuracy):** Kiểm tra lỗi đường truyền, mất gói tin, tỉ lệ lỗi bit (BER) hoặc lỗi CRC trên các cổng switch/router,.
- **Hiệu quả (Efficiency):** Phân tích kích thước gói tin (packet sizes). Mạng hiệu quả thường sử dụng kích thước khung tin lớn nhất có thể để giảm overhead. Kích thước gói tin thường phân bố theo dạng "lưng lạc đà" (nhiều gói nhỏ và nhiều gói lớn, ít gói trung bình),.
- **Độ trễ (Delay & Response Time):** Đo thời gian phản hồi giữa các node mạng (ví dụ: A đến B, B đến C...) để xác định độ trễ hiện tại,.

### 4. Kiểm tra trạng thái thiết bị mạng (Device Status)

Sử dụng các lệnh kiểm tra trên Router/Switch (đặc biệt là thiết bị Cisco) để xem tình trạng tài nguyên phần cứng,:

- `show processes cpu`: Kiểm tra mức độ sử dụng CPU (tránh quá tải).
- `show memory`: Kiểm tra dung lượng bộ nhớ.
- `show interfaces`: Xem thống kê lỗi, drops, collisions trên cổng.
- `show buffers`: Kiểm tra tình trạng bộ nhớ đệm.

### 5. Các công cụ hỗ trợ

Chương này cũng giới thiệu các công cụ để thu thập thông tin mạng như:

- **Protocol analyzers:** Phân tích giao thức (ví dụ: Wireshark).
- **MRTG:** Đồ thị lưu lượng mạng.
- **CDP (Cisco Discovery Protocol):** Giao thức khám phá thiết bị lân cận của Cisco.
- **NetFlow:** Công nghệ thu thập và đo lường luồng dữ liệu.

**Tóm lại:** Chương 3 nhấn mạnh việc **"Khám bệnh"** cho hệ thống mạng hiện tại. Việc này giúp nhà thiết kế tránh được các rủi ro (như đặt thiết bị mới vào một hạ tầng cáp đã hỏng) và chứng minh được sự cải thiện hiệu suất sau khi dự án hoàn thành.

----

Dựa trên tài liệu **Top-Down Network Design - Chương 3**, dưới đây là một phân tích chuyên sâu về giai đoạn **"Đặc tả hệ thống mạng hiện tại" (Characterizing the Existing Internetwork)**.

Đây là bước nền tảng trong quy trình thiết kế mạng, được ví như việc "bắt mạch" cho một bệnh nhân trước khi kê đơn thuốc. Nếu không hiểu rõ hiện trạng, kỹ sư mạng không thể đề xuất giải pháp nâng cấp chính xác.

### 1. Tại sao phải đặc tả mạng hiện tại?

Việc thiết kế mạng không bao giờ bắt đầu từ con số 0 (trừ khi xây dựng mới hoàn toàn). Hầu hết các dự án là nâng cấp hoặc mở rộng. Chương 3 nhấn mạnh triết lý: _"Nếu chúng ta biết chúng ta đang ở đâu... chúng ta có thể đánh giá tốt hơn những gì cần làm"_.

- **Mục đích:** Tạo ra một **Baseline (Mức chuẩn)** về hiệu suất và cấu trúc. Baseline này dùng để so sánh hiệu quả trước và sau khi nâng cấp, chứng minh giá trị của thiết kế mới.
- **Rủi ro nếu bỏ qua:** Có thể mua thiết bị không tương thích, không giải quyết được nút thắt cổ chai (bottleneck) thực sự, hoặc xây dựng trên nền tảng cáp vật lý đã hỏng hóc.

### 2. Phân tích Chi tiết Cơ sở Hạ tầng (Infrastructure Analysis)

Quá trình này chia thành 3 mảng chính:

#### A. Cấu trúc mạng (Topology)

Cần phân biệt rõ hai loại bản đồ mạng:

- **Mô hình Vật lý (Physical Topology):** Xác định vị trí cụ thể của thiết bị (tòa nhà, tầng, tủ rack), loại cáp kết nối (quang, đồng), và sơ đồ đi dây (đứng/ngang),.
- **Mô hình Logic (Logical Topology):** Quan trọng hơn đối với kiến trúc sư mạng. Nó thể hiện mối quan hệ giữa các phân đoạn mạng (VLAN, subnet), kiến trúc phân cấp (Core/Distribution/Access), và các điểm biên giới an ninh.

#### B. Hệ thống Đặt tên và Địa chỉ (Addressing & Naming)

Không chỉ là liệt kê IP, mà là tìm ra các vấn đề tiềm ẩn:

- **Subnet không liên tục (Discontiguous Subnets):** Đây là vấn đề lớn đối với các giao thức định tuyến cũ (classful) hoặc cấu hình auto-summary, gây lỗi định tuyến.
- **Quy tắc đặt tên:** Kiểm tra xem hệ thống hiện tại có quy chuẩn đặt tên (ví dụ: `HCM-Core-SW01`) hay đặt tùy ý, gây khó khăn cho quản trị sau này.

#### C. Ràng buộc về Kiến trúc & Môi trường (Environmental Constraints)

Đây là yếu tố vật lý thường bị kỹ sư IT bỏ qua nhưng cực kỳ quan trọng khi triển khai thực tế,:

- **Cơ sở vật chất:** Hệ thống điều hòa (Air conditioning), thông gió, nguồn điện dự phòng, không gian tủ rack và an ninh vật lý (cửa khóa).
- **Vấn đề Mạng không dây (Wireless Issues):** Tài liệu nhấn mạnh 4 hiện tượng vật lý ảnh hưởng đến sóng Wi-Fi cần khảo sát:
    - _Phản xạ (Reflection):_ Sóng dội lại từ bề mặt kim loại.
    - _Hấp thụ (Absorption):_ Sóng bị vật liệu (tường dày, nước) hấp thụ làm yếu tín hiệu.
    - _Khúc xạ (Refraction):_ Sóng bị bẻ cong khi đi qua các môi trường khác nhau.
    - _Nhiễu xạ (Diffraction):_ Sóng bị tản ra khi gặp vật cản.

### 3. Kiểm tra "Sức khỏe" Mạng (Network Health Check)

Đây là phần cốt lõi của Chương 3, tập trung vào các chỉ số định lượng để đánh giá chất lượng mạng hiện tại:

#### A. Tính sẵn sàng (Availability)

- Đo lường thời gian "chết" (downtime) và nguyên nhân của những lần sập mạng gần nhất.
- Sử dụng các chỉ số MTBF (thời gian trung bình giữa các lần hỏng) và MTTR (thời gian trung bình để sửa chữa).

#### B. Hiệu suất & Băng thông (Utilization)

- **Utilization:** Phải đo lường theo các khung thời gian khác nhau (theo phút, theo giờ) để phát hiện các đỉnh lưu lượng (peaks). Nếu chỉ đo trung bình ngày, bạn sẽ bỏ lỡ các thời điểm nghẽn mạng.
- **Protocol Analysis:** Phân tích xem băng thông đang bị chiếm dụng bởi giao thức nào (tương đối và tuyệt đối). Ví dụ: Nếu băng thông chủ yếu là Broadcast/Multicast thay vì Unicast, mạng đang gặp vấn đề về thiết kế Layer 2.

#### C. Độ chính xác & Hiệu quả (Accuracy & Efficiency)

- **Accuracy (Độ chính xác):** Kiểm tra tỷ lệ lỗi (Error rate). Các lỗi như CRC errors, collisions (trên cổng switch) là dấu hiệu của cáp hỏng hoặc duplex mismatch (lệch pha song công).
- **Efficiency (Hiệu quả - Phân tích kích thước gói tin):**
    - Một mạng khỏe mạnh thường có biểu đồ kích thước gói tin dạng **"Lưng lạc đà" (Bimodal distribution)**: Nhiều gói rất nhỏ (64 bytes - là các gói tin ACK, keepalive) và nhiều gói rất lớn (1518 bytes - là các gói truyền dữ liệu).
    - Nếu mạng có nhiều gói kích thước trung bình hoặc kích thước lạ, đó có thể là dấu hiệu của phân mảnh (fragmentation) hoặc lỗi ứng dụng.

#### D. Độ trễ (Delay & Response Time)

- Đo thời gian phản hồi giữa các nút mạng quan trọng (Client - Server).
- Phân biệt giữa trễ do đường truyền (propagation delay) và trễ do xử lý tại thiết bị (processing delay).

### 4. Công cụ và Kỹ thuật thu thập (Tools & Techniques)

Để thực hiện các phân tích trên, Chương 3 đề xuất sử dụng các lệnh kiểm tra trạng thái thiết bị và công cụ hỗ trợ:

- **Lệnh Cisco IOS:** `show processes cpu` (xem tải CPU), `show memory` (xem RAM), `show interfaces` (xem lỗi CRC, drops), `show buffers`.
- **Công cụ phần mềm:**
    - _Protocol Analyzers (như Wireshark):_ Để bắt gói tin, phân tích lỗi sâu.
    - _MRTG:_ Vẽ đồ thị lưu lượng theo thời gian.
    - _NetFlow:_ Phân tích luồng dữ liệu (ai đang nói chuyện với ai).
    - _CDP (Cisco Discovery Protocol):_ Tự động khám phá sơ đồ kết nối thiết bị Cisco.

### Tóm lại

Chương 3 không chỉ là việc vẽ lại sơ đồ mạng. Nó là một quy trình **khám nghiệm tổng thể**: từ vật lý (cáp, nguồn, nhiễu sóng) đến logic (định tuyến, IP) và hiệu năng (băng thông, độ trễ, lỗi). Kết quả của chương này là đầu vào bắt buộc để sang Chương 4 (Phân tích lưu lượng) và Chương 5 (Thiết kế Topo mạng mới).

# Bộ Đề Trắc Nghiệm Chương 3: Đặc Tả Mạng Hiện Tại (Characterizing the Existing Network)

**Tags:** #NetworkDesign #CharacterizingNetwork #Cisco #TopDownDesign #UIT #FastPay #KTX
**Chủ đề:** Khảo sát và đánh giá hạ tầng mạng hiện tại.
**Ngày ôn tập:** [[2026-01-06]]

---

## 💡 Tổng Quan Kiến Thức Chương 3
Trước khi thiết kế hệ thống mới, kỹ sư phải thực hiện bước "khám lâm sàng" cho mạng hiện tại:
1. **Sơ đồ mạng (Topologies):** Phân biệt mô hình Vật lý (vị trí lắp đặt) và Logic (địa chỉ IP, VLAN).
2. **Hệ thống cáp (Wiring):** Kiểm tra MDF (tủ chính), IDF (tủ phụ) và giới hạn 100m của cáp đồng.
3. **Sức khỏe mạng (Health Check):** Đo lường CPU, Utilization và các lỗi lớp 1/2 (CRC, Collision).
4. **Công cụ khảo sát:** CDP (khám phá thiết bị lân cận), SNMP (giám sát), Wireshark (phân tích gói tin).

---

## Phần 1: Nhận biết (Knowledge)

**Câu 1: Mục đích chính của việc "Đặc tả hệ thống mạng hiện tại" là gì?**
A. Để tìm ra ai là người chịu trách nhiệm cho các lỗi mạng.
B. Để thiết lập một mức chuẩn (baseline) về hiệu suất và cấu trúc nhằm so sánh sau khi nâng cấp.
C. Để ngay lập tức thay thế toàn bộ thiết bị cũ.
D. Để sao chép cấu hình từ mạng của đối thủ cạnh tranh.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Thiết lập Baseline (đường cơ sở) là cực kỳ quan trọng để chứng minh hiệu quả của thiết kế mới. Nếu không biết mạng cũ chạy chậm thế nào, bạn không thể khẳng định mạng mới đã nhanh hơn.

**Câu 2: Lệnh nào trên Cisco IOS thường được sử dụng để kiểm tra mức độ sử dụng CPU của thiết bị trong quá trình khảo sát?**
A. show interfaces
B. show ip route
C. show processes cpu
D. show version

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: C**
> **Giải thích:** Lệnh này giúp xác định thiết bị có đang bị quá tải hay không thông qua các chỉ số tải CPU trong 5 giây, 1 phút và 5 phút.

**Câu 3: Trong mô hình mạng, "Main Distribution Frame" (MDF) thường là nơi chứa:**
A. Các thiết bị người dùng cuối (PC, Laptop).
B. Điểm kết nối chính của tòa nhà, thường kết nối với các đường trục (backbone) và mạng bên ngoài (WAN).
C. Các điểm truy cập không dây (Access Point).
D. Chỉ chứa dây điện thoại.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** MDF là điểm tập trung kết nối cao nhất trong một tòa nhà, từ đó tỏa tín hiệu đi các IDF (Intermediate Distribution Frame) ở các tầng hoặc các khu vực khác.
> 

**Câu 4: Khi khảo sát hệ thống cáp (Wiring), giới hạn độ dài chuẩn cho cáp đồng UTP từ tủ mạng đến máy trạm là bao nhiêu?**
A. 50 mét | B. 100 mét | C. 185 mét | D. 500 mét

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Theo tiêu chuẩn TIA/EIA, cáp xoắn đôi (UTP) chỉ đảm bảo tín hiệu dữ liệu truyền ổn định trong phạm vi 100 mét. Vượt quá khoảng cách này sẽ gây lỗi bit và mất kết nối.

**Câu 5: Giao thức nào của Cisco giúp tự động khám phá thông tin về các thiết bị lân cận (như tên, IP, model)?**
A. SNMP
B. CDP (Cisco Discovery Protocol)
C. OSPF
D. ICMP

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** CDP là giao thức lớp 2 của Cisco, cực kỳ hữu ích để vẽ lại sơ đồ mạng khi tài liệu bị thất lạc. Nó cho bạn biết chính xác thiết bị nào đang cắm vào cổng nào của mình.

---

## Phần 2: Hiểu (Comprehension)

**Câu 11: Tại sao việc xác định "Subnet không liên tục" (Discontiguous Subnets) trong mạng hiện tại lại quan trọng?**
A. Vì nó làm mạng chạy nhanh hơn.
B. Vì nó có thể gây ra lỗi định tuyến nếu sử dụng các giao thức cũ hoặc cấu hình summary sai.
C. Vì nó tiết kiệm địa chỉ IP.
D. Vì nó là mô hình bảo mật tốt nhất.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Subnet không liên tục (ví dụ hai dải 10.x.x.x bị chia cách bởi dải 172.16.x.x) sẽ khiến việc tóm tắt định tuyến (summarization) gặp lỗi, dẫn đến gói tin bị gửi sai hướng.

**Câu 12: Khi vẽ sơ đồ mạng, sự khác biệt giữa "Mô hình Vật lý" và "Mô hình Logic" là gì?**
A. Vật lý chỉ vẽ dây cáp; Logic chỉ vẽ Router.
B. Vật lý thể hiện vị trí thiết bị, tầng, tủ rack; Logic thể hiện luồng dữ liệu, địa chỉ IP, VLAN.
C. Vật lý dùng cho sếp xem; Logic dùng cho kỹ thuật xem.
D. Không có sự khác biệt.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Sơ đồ vật lý trả lời câu hỏi "Thiết bị nằm ở đâu?", sơ đồ logic trả lời câu hỏi "Dữ liệu đi như thế nào?".

**Câu 13: Tại sao cần kiểm tra "CRC errors" trên các cổng switch trong giai đoạn khảo sát?**
A. Để biết switch có bật nguồn không.
B. CRC errors cao thường chỉ ra vấn đề về lớp vật lý (cáp hỏng, nhiễu) hoặc lỗi duplex.
C. Để kiểm tra tốc độ CPU.
D. Để xem switch thuộc VLAN nào.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Lỗi CRC (Cyclic Redundancy Check) cho thấy khung tin bị hỏng khi truyền trên dây. Nguyên nhân phổ biến nhất là cáp kém chất lượng hoặc nhiễu điện từ.

**Câu 17: Biểu đồ phân bố kích thước gói tin dạng "Lưng lạc đà" (Bimodal) thường cho thấy điều gì?**
A. Mạng đang bị tấn công.
B. Phần lớn là gói tin rất nhỏ (ACK) và gói tin rất lớn (Data transfer), ít gói trung bình.
C. Mạng chỉ toàn gói tin nhỏ.
D. Mạng bị nghẽn.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Đây là trạng thái bình thường của mạng. Gói nhỏ dùng để điều khiển và phản hồi (ACK), gói lớn (1500 bytes) dùng để truyền dữ liệu hiệu quả nhất.
> 

**Câu 18: Khi một khách hàng nói "Mạng chậm lắm", kỹ sư mạng cần làm gì đầu tiên?**
A. Mua ngay đường truyền Internet mới.
B. Sử dụng các công cụ đo lường để định lượng (băng thông, độ trễ, gói tin lỗi) thay vì dựa vào cảm tính.
C. Cài lại Win cho khách hàng.
D. Khởi động lại Router.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Kỹ sư thiết kế phải chuyển đổi các phàn nàn "định tính" của khách hàng thành số liệu "định lượng" để tìm ra nút thắt cổ chai thực sự.

---

## Phần 3: Vận dụng (Application)

**Câu 22: Thấy cổng Switch có rất nhiều lỗi "Late Collision". Nguyên nhân có thể là gì?**
A. Dây cáp quá dài (>100m) hoặc cấu hình sai Duplex (một bên Full, một bên Half).
B. Server bị virus.
C. Switch bị hỏng phần mềm.
D. Chưa cấu hình IP.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: A**
> **Giải thích:** Late Collision là lỗi va chạm xảy ra sau khi đã gửi đi 64 bytes đầu tiên. Đây là dấu hiệu kinh điển của việc vi phạm độ dài cáp Ethernet hoặc xung đột chế độ Duplex.
> 

**Câu 25: Tại sao trong dự án FastPay, bộ phận Kế toán nên đặt vào một VLAN riêng biệt?**
A. Để họ có mạng nhanh hơn.
B. Để đảm bảo an toàn dữ liệu tài chính (Confidentiality) và ngăn chặn truy cập trái phép.
C. Để dễ sửa máy in.
D. Để tiết kiệm IP.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Việc phân đoạn mạng (Segmentation) giúp cô lập dữ liệu nhạy cảm, ngăn không cho người dùng từ các phòng ban khác (như Sale hay Guest) "nhìn" thấy các máy chủ kế toán.

**Câu 26: Kiểm tra Router cũ và thấy CPU luôn ở mức 90%. Tiến trình "IP Input" chiếm cao nhất. Gợi ý vấn đề gì?**
A. Router đang thực hiện Process Switching quá nhiều.
B. Router đang bị nóng.
C. Router đang chạy quá nhiều định tuyến tĩnh.
D. Không có vấn đề gì.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: A**
> **Giải thích:** "IP Input" cao nghĩa là CPU phải trực tiếp xử lý từng gói tin. Bạn nên kiểm tra xem CEF (Cisco Express Forwarding) đã được bật chưa để đẩy việc chuyển tiếp sang phần cứng.

**Câu 30: Mạng dùng lớp mạng 192.168.1.0/24 cho toàn bộ 500 nhân viên. Vấn đề là gì?**
A. Không đủ địa chỉ IP (chỉ có 254 IP khả dụng).
B. Địa chỉ IP quá dài.
C. Subnet mask sai.
D. Không có vấn đề gì.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: A**
> **Giải thích:** Một dải mạng /24 chỉ cho phép tối đa 254 thiết bị. Với 500 nhân viên, bạn cần đổi sang dải mạng lớn hơn (như /23) hoặc chia nhỏ thành các VLAN.

---

## Phần 4: Vận dụng cao (High Application)

**Câu 31: Khảo sát hiện trạng thấy mạng dùng Hub và cáp Cat3. Khách muốn triển khai VoIP. Kết luận?**
A. Có thể triển khai VoIP ngay.
B. Mạng không đáp ứng được. Cần thay Hub bằng Switch và đi lại cáp Cat5e/Cat6.
C. Chỉ cần thay Hub bằng Switch là đủ.
D. Chỉ cần thay cáp là đủ.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** VoIP yêu cầu độ trễ cực thấp và không có va chạm. Hub (vùng xung đột) và cáp Cat3 (tốc độ thấp) chắc chắn sẽ làm chất lượng thoại cực kỳ tệ.

**Câu 36: Trong Ansible, nếu chạy playbook 2 lần trên cùng một thiết bị, điều gì xảy ra nếu playbook đạt tính "idempotent"?**
A. Tạo ra 2 VLAN trùng tên lỗi.
B. Thiết bị báo lỗi.
C. Không có thay đổi gì xảy ra trong lần chạy thứ 2.
D. Xóa VLAN cũ đi tạo lại.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: C**
> **Giải thích:** Tính lũy đẳng (Idempotency) đảm bảo rằng Ansible chỉ thay đổi thiết bị khi nó chưa đạt đúng trạng thái mong muốn. Nếu cấu hình đã đúng rồi, lần chạy sau sẽ không làm gì cả.

**Câu 39: Biểu đồ băng thông Leased Line luôn phẳng lì ở mức tối đa 10Mbps trong giờ làm việc. Kết luận?**
A. Đường truyền rất ổn định.
B. Đường truyền đang bị nghẽn (Congestion), cần nâng cấp băng thông.
C. Zabbix bị lỗi.
D. Không ai dùng mạng.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Hiện tượng "flat-lining" (phẳng đầu) tại mức giới hạn cho thấy lưu lượng thực tế đang vượt quá khả năng của đường truyền.

---

## Phần 5: Hiểu sâu / Thực tiễn

**Câu 43: Tại sao trong bài lab Zabbix, sinh viên chọn Ubuntu 22.04 LTS thay vì 24.04 mới nhất?**
A. Vì thích giao diện cũ.
B. Vì tính tương thích và ổn định (Stability).
C. Vì Ubuntu 24.04 tốn tiền.
D. Vì giảng viên bắt buộc.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** Trong môi trường triển khai thực tế, sự ổn định của phiên bản LTS (Long Term Support) và sự tương thích của các gói phần mềm quan trọng hơn việc chạy theo phiên bản mới nhất chưa được kiểm chứng.

**Câu 46: Giao thức HSRP (Hot Standby Router Protocol) trong FastPay giải quyết vấn đề gì?**
A. Cân bằng tải Internet.
B. Dự phòng Gateway mặc định (Default Gateway Redundancy).
C. Chặn virus.
D. Tăng tốc độ Wi-Fi.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: B**
> **Giải thích:** HSRP giúp hai Router đóng vai trò là một "Gateway ảo". Nếu một Router hỏng, Router còn lại sẽ thay thế ngay lập tức mà người dùng không bị rớt mạng.
> 

**Câu 49: Trong bài lab Router-on-a-Stick, lệnh `encapsulation dot1q 10` trên sub-interface có ý nghĩa gì?**
A. Đặt IP cho cổng.
B. Bật cổng.
C. Khai báo rằng sub-interface này sẽ xử lý các frame thuộc VLAN 10 (theo chuẩn 802.1Q).
D. Cấu hình định tuyến OSPF.

> [!success]- Xem Đáp án & Giải thích
> **Đáp án: C**
> **Giải thích:** Router dùng lệnh này để "bóc tách" cái nhãn (tag) VLAN 10 được Switch đóng vào gói tin khi truyền qua đường trunk.