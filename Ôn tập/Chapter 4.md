
### 1. Phân tích Luồng Lưu lượng (Traffic Flow)

Mục tiêu là xác định nguồn (sources) và đích (destinations) của lưu lượng, đồng thời phân tích hướng và tính đối xứng của dòng dữ liệu.

- **Xác định các thành phần chính:**
    - **User Communities (Cộng đồng người dùng):** Nhóm các nhân viên sử dụng cùng một tập hợp ứng dụng (ví dụ: bộ phận Kế toán, bộ phận R&D).
    - **Data Stores (Kho dữ liệu):** Nơi chứa dữ liệu ứng dụng như Server, Server Farm, SAN, Mainframe, hoặc tủ băng từ backup.
- **Các loại luồng lưu lượng điển hình:**
    - **Terminal/Host:** Thường là bất đối xứng (asymmetric). Terminal gửi vài ký tự, Host gửi lại nhiều ký tự (ví dụ: Telnet).
    - **Client/Server:** Loại phổ biến nhất (ví dụ: Web, File share). Client gửi yêu cầu (gói tin nhỏ), Server phản hồi dữ liệu (gói tin lớn). Luồng này thường là hai chiều và bất đối xứng.
    - **Thin Client:** Xử lý dữ liệu diễn ra trên server, client chỉ hiển thị. Cần băng thông ổn định.
    - **Peer-to-Peer (P2P):** Hai chiều và đối xứng (symmetric). Các thiết bị giao tiếp bình đẳng, không có phân cấp (ví dụ: hội nghị truyền hình, chia sẻ file ngang hàng).
    - **Server/Server:** Giao tiếp giữa các server để sao chép dữ liệu, backup, hoặc đồng bộ thư mục.
    - **Distributed Computing:** Nhiều máy tính cùng xử lý một tác vụ (ví dụ: render phim, mô phỏng kỹ thuật). Yêu cầu phân tích kỹ về độ kết nối chặt hay lỏng giữa các node.
- **Lưu lượng VoIP:** Cần lưu ý VoIP có 2 luồng riêng biệt: Luồng tín hiệu (Call setup/teardown - Client/Server) và luồng thoại (Audio stream - Peer-to-Peer qua giao thức RTP).

### 2. Phân tích Tải trọng Lưu lượng (Traffic Load)

Mục đích là tính toán dung lượng băng thông cần thiết để tránh nghẽn mạng.

- **Cách tính toán:** Dựa trên số lượng trạm (stations), thời gian rỗi trung bình và thời gian truyền tải gói tin.
- **Hành vi ứng dụng:** Cần xác định số lượng người dùng đồng thời, tần suất sử dụng và kích thước trung bình của các đối tượng dữ liệu (file, email, web page).
- **Overhead của giao thức:** Cần tính đến kích thước header của các giao thức (TCP/IP, Ethernet...) khi tính toán tải thực tế. Ví dụ, một gói tin Ethernet nhỏ nhất cũng tốn 64 bytes.

### 3. Phân tích Hành vi Lưu lượng (Traffic Behavior)

Ngoài tải trọng, cần xem xét cách các giao thức hoạt động trên mạng:

- **Broadcast/Multicast:** Các gói tin broadcast (địa chỉ đích FF:FF:FF:FF:FF:FF) làm gián đoạn CPU của tất cả thiết bị trong miền broadcast. Cần kiểm soát để tránh bão broadcast.
- **Hiệu quả mạng (Network Efficiency):**
    - **Kích thước khung tin (Frame Size):** Sử dụng khung tin lớn nhất có thể (MTU) giúp tăng hiệu quả truyền tải cho các ứng dụng truyền file.
    - **Windowing và Flow Control:** Cơ chế cửa sổ (Windowing) của TCP cho phép gửi nhiều gói tin trước khi cần xác nhận (ACK). Cần tinh chỉnh kích thước cửa sổ để tối ưu hóa băng thông.
    - **Cơ chế phục hồi lỗi:** Các giao thức có cơ chế truyền lại (retransmission) kém có thể gây lãng phí băng thông.

### 4. Yêu cầu về Chất lượng Dịch vụ (QoS Requirements)

Đây là phần quan trọng để xác định ứng dụng nào cần ưu tiên. Tài liệu chia ứng dụng thành hai loại: **Linh hoạt (Flexible)** và **Không linh hoạt (Inflexible)**.

- **Các thông số kỹ thuật QoS (dựa trên chuẩn ATM và IETF):**
    - **CBR (Constant Bit Rate):** Dành cho ứng dụng thời gian thực cần băng thông cố định (như Voice, Video không nén).
    - **VBR (Variable Bit Rate):**
        - _rt-VBR (Real-time):_ Cho ứng dụng nén video/voice nhạy cảm với độ trễ.
        - _nrt-VBR (Non-real-time):_ Cho ứng dụng giao dịch quan trọng nhưng chịu được trễ nhẹ.
    - **UBR (Unspecified Bit Rate):** Dịch vụ "Best-effort" (nỗ lực tối đa), không cam kết băng thông, dùng cho file transfer, email.
    - **ABR (Available Bit Rate):** Sử dụng băng thông còn dư của mạng, có cơ chế phản hồi tắc nghẽn.
- **Mô hình IETF cho IP:**
    - **IntServ (Integrated Services):** Sử dụng giao thức RSVP để dành riêng tài nguyên cho từng luồng dữ liệu (Guaranteed Service hoặc Controlled-Load Service),.
    - **DiffServ (Differentiated Services):** Đánh dấu gói tin (DSCP) để các router xử lý ưu tiên (PHB - Per-Hop Behaviors) mà không cần dành riêng tài nguyên từng luồng.
- **Voice Quality:** Yêu cầu độ trễ thấp, jitter thấp và tỷ lệ mất gói tin cực thấp để đảm bảo Grade of Service (GoS).

**Tóm lại:** Chương 4 hướng dẫn cách chuyển đổi các yêu cầu trừu tượng của khách hàng thành các thông số kỹ thuật cụ thể về băng thông, độ trễ và QoS, làm cơ sở để thiết kế sơ đồ mạng (Topology) ở Chương 5.

Dựa trên tài liệu **Top-Down Network Design - Chương 4**, dưới đây là câu trả lời chi tiết cho các câu hỏi của bạn về đặc tả lưu lượng mạng.

---

# Tổng hợp Kiến thức Chương 4: Đặc tả Lưu lượng Mạng

## 1. Sáu loại luồng lưu lượng (Traffic Flows)

Trong thiết kế mạng, việc phân loại luồng lưu lượng giúp xác định mô hình băng thông và hướng di chuyển của dữ liệu. Có 6 loại chính:

1. **Terminal/Host (Thiết bị đầu cuối/Máy chủ):**
    
    - **Mô tả:** Đây thường là luồng lưu lượng bất đối xứng (asymmetric). Thiết bị đầu cuối gửi một vài ký tự (như phím gõ) đến máy chủ, và máy chủ phản hồi lại nhiều ký tự (dữ liệu màn hình).
    - **Ví dụ:** Telnet là ví dụ điển hình. Khi người dùng gõ, mỗi ký tự được gửi đi trong một gói tin, và máy chủ phản hồi lại các cập nhật màn hình,.
2. **Client/Server (Khách/Chủ):**
    
    - **Mô tả:** Đây là loại phổ biến nhất hiện nay. Client gửi yêu cầu đến Server (thường là gói tin nhỏ), và Server phản hồi lại dữ liệu (thường là các luồng dữ liệu lớn). Luồng này thường là hai chiều (bidirectional) và bất đối xứng (asymmetric).
    - **Ví dụ:** Duyệt web (HTTP), gửi nhận email, truy cập cơ sở dữ liệu,.
3. **Thin Client (Máy trạm tinh giản):**
    
    - **Mô tả:** Một dạng đặc biệt của mô hình Client/Server. Phần lớn việc xử lý dữ liệu diễn ra trên máy chủ trung tâm, client chỉ có nhiệm vụ hiển thị và nhận input. Dữ liệu từ server xuống client có thể rất lớn, đặc biệt khi nhiều máy khởi động cùng lúc.
    - **Ví dụ:** Các hệ thống chạy ứng dụng qua Citrix hoặc Remote Desktop,.
4. **Peer-to-Peer (Ngang hàng - P2P):**
    
    - **Mô tả:** Luồng lưu lượng thường là hai chiều và đối xứng (symmetric). Các thiết bị giao tiếp bình đẳng, không có phân cấp rõ ràng ai là chủ hay khách. Mỗi thiết bị vừa gửi vừa nhận lượng dữ liệu tương đương.
    - **Ví dụ:** Hội nghị truyền hình (Videoconferencing), chia sẻ file ngang hàng,.
5. **Server/Server (Máy chủ/Máy chủ):**
    
    - **Mô tả:** Giao tiếp giữa các máy chủ để thực hiện các tác vụ nền như sao lưu, đồng bộ hóa. Luồng này thường là hai chiều và tính đối xứng phụ thuộc vào ứng dụng.
    - **Ví dụ:** Sao chép dữ liệu (replication), sao lưu (backup), đồng bộ thư mục (directory services),.
6. **Distributed Computing (Điện toán phân tán):**
    
    - **Mô tả:** Nhiều nút máy tính cùng làm việc để giải quyết một tác vụ phức tạp. Dữ liệu di chuyển giữa trình quản lý tác vụ và các nút tính toán. Tùy thuộc vào việc các nút được liên kết chặt chẽ (tightly coupled) hay lỏng lẻo (loosely coupled) mà tần suất trao đổi dữ liệu sẽ khác nhau.
    - **Ví dụ:** Render hình ảnh đồ họa, mô phỏng kỹ thuật, khai thác dữ liệu,.

---

## 2. Tại sao lưu lượng VoIP khó đặc tả và lập kế hoạch?

Lưu lượng Voice over IP (VoIP) đặt ra thách thức lớn cho việc thiết kế mạng vì nó bao gồm **hai luồng lưu lượng riêng biệt với đặc điểm khác nhau**:

1. **Luồng thoại (Audio/Voice Stream):**
    
    - Đây là luồng truyền tải giọng nói thực tế, sử dụng giao thức RTP (Real-Time Transport Protocol).
    - Đặc điểm: Hoạt động theo mô hình **Peer-to-Peer** (giữa hai điện thoại hoặc hai PC).
    - Yêu cầu: Nhạy cảm với độ trễ (delay), biến thiên trễ (jitter) và mất gói tin,.
2. **Luồng kiểm soát cuộc gọi (Call Control/Signaling):**
    
    - Đây là luồng dùng để thiết lập, duy trì và ngắt cuộc gọi (ví dụ: quay số, đổ chuông), sử dụng các giao thức như H.323, SIP hoặc SCCP.
    - Đặc điểm: Hoạt động theo mô hình **Client/Server** (giữa điện thoại và máy chủ xử lý cuộc gọi như Cisco Unified Communications Manager).
    - Yêu cầu: Không quá khắt khe về độ trễ như luồng thoại nhưng yêu cầu tính sẵn sàng cao,.

**Thách thức:** Nhà thiết kế phải tính toán băng thông và QoS cho hai luồng này một cách riêng biệt vì chúng có thể đi theo các đường dẫn khác nhau trong mạng.

---

## 3. Tại sao cần quan tâm đến lưu lượng Broadcast?

Bạn cần quan tâm đến lưu lượng Broadcast (quảng bá) vì những lý do sau:

- **Làm gián đoạn CPU của mọi thiết bị:** Gói tin Broadcast (địa chỉ đích FF:FF:FF:FF:FF:FF) buộc **tất cả** các trạm (stations) trong miền broadcast phải ngắt xử lý để kiểm tra gói tin, ngay cả khi gói tin đó không dành cho chúng. Điều này gây lãng phí tài nguyên xử lý trên toàn mạng,.
- **Chiếm dụng băng thông:** Mặc dù một gói tin broadcast có thể nhỏ, nhưng nếu số lượng lớn (ví dụ do bão broadcast hoặc cấu hình sai), chúng có thể làm nghẽn mạng.
- **Hiệu ứng dây chuyền:** Các thiết bị lớp 2 (Switch/Bridge) mặc định sẽ chuyển tiếp broadcast ra tất cả các cổng, làm lan rộng ảnh hưởng (Broadcast radiation),.
- **Ngưỡng cảnh báo:** Nếu lưu lượng broadcast/multicast chiếm hơn **20%** tổng lưu lượng mạng, hiệu suất mạng sẽ bị suy giảm nghiêm trọng và cần phải phân đoạn lại mạng (dùng Router hoặc VLAN).

---

## 4. Sự khác biệt giữa đặc tả QoS của ATM và IETF

Hai tổ chức này tiếp cận Chất lượng dịch vụ (QoS) theo các mô hình dịch vụ khác nhau:

### a. Đặc tả của ATM (ATM Forum)

ATM định nghĩa các loại dịch vụ (service categories) dựa trên bản chất của luồng dữ liệu (thời gian thực hay không) và các tham số như tốc độ cell (PCR, SCR), độ trễ (maxCTD) và tỉ lệ mất cell (CLR). Các loại dịch vụ bao gồm:

- **CBR (Constant Bit Rate):** Dành cho ứng dụng thời gian thực cần băng thông cố định (Voice/Video không nén),.
- **rt-VBR (Real-time Variable Bit Rate):** Dành cho ứng dụng thời gian thực có băng thông biến đổi (Video nén).
- **nrt-VBR (Non-real-time VBR):** Dành cho ứng dụng không thời gian thực nhưng cần cam kết băng thông.
- **UBR (Unspecified Bit Rate):** Dịch vụ "nỗ lực tối đa" (Best-effort), không cam kết gì (Email, file transfer).
- **ABR (Available Bit Rate):** Sử dụng băng thông còn dư, có cơ chế phản hồi tắc nghẽn.
- **GFR (Guaranteed Frame Rate):** Đảm bảo tốc độ khung tối thiểu.

### b. Đặc tả của IETF (Integrated Services Working Group)

IETF tập trung vào môi trường IP và định nghĩa các dịch vụ QoS thông qua giao thức RSVP (Resource Reservation Protocol) để đảm bảo chất lượng cho các luồng dữ liệu,. Hai loại dịch vụ chính là:

- **Controlled-Load Service (Dịch vụ kiểm soát tải):** Cung cấp chất lượng dịch vụ tương đương với việc chạy trên một mạng không bị nghẽn (unloaded network). Nó dành cho các ứng dụng nhạy cảm với quá tải nhưng không cần đảm bảo trễ tuyệt đối,.
- **Guaranteed Service (Dịch vụ đảm bảo):** Cung cấp giới hạn trễ chắc chắn (mathematically provable bounds) về độ trễ hàng đợi đầu-cuối. Nó đảm bảo gói tin sẽ không bị đến muộn hơn một khoảng thời gian nhất định,.

**Tóm lại:** ATM phân loại chi tiết dựa trên đặc tính bit/cell và quản lý băng thông cứng, trong khi IETF (IntServ) tập trung vào việc đặt trước tài nguyên (reservation) để đảm bảo độ trễ và trải nghiệm dịch vụ trên nền tảng gói tin (packet).

