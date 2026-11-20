# Bộ Đề Trắc Nghiệm: Thiết Kế Mạng Top-Down & Thực Tiễn Dự Án

**Tags:** #NetworkDesign #Cisco #TopDown #DevOps #FastPay #KTX_Project
**Ngày ôn tập:** [[2026-01-06]]
**Trạng thái:** #Review

---

## Phần 1: Nhận biết (Knowledge)
*Mục tiêu: Nhớ lại các định nghĩa, thuật ngữ và khái niệm cơ bản.*

**Câu 1: Phương pháp thiết kế mạng Top-Down bắt đầu từ tầng nào của mô hình OSI?**
A. Tầng Vật lý (Physical)
B. Tầng Liên kết dữ liệu (Data Link)
C. Tầng Ứng dụng (Application)
D. Tầng Mạng (Network)

> [!success]- Xem Đáp án
> **Đáp án: C**
> *Giải thích:* Phương pháp Top-Down bắt đầu từ tầng trên cùng (Ứng dụng) để hiểu nhu cầu của người dùng và doanh nghiệp trước khi quyết định các thiết bị hạ tầng (tầng dưới).

**Câu 2: Trong chu trình vòng đời mạng PDIOO, giai đoạn nào tập trung vào việc xác định các yêu cầu kinh doanh và kỹ thuật ban đầu?**
A. Design
B. Plan
C. Implement
D. Optimize

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Giai đoạn Plan (Lập kế hoạch) bao gồm việc xác định các yêu cầu mạng chi tiết, phân tích hạ tầng cần thiết và xác định người dùng cần phục vụ.

**Câu 3: Cisco Express Forwarding (CEF) sử dụng hai bảng nào để chuyển tiếp gói tin trong phần cứng?**
A. Routing Table và ARP Table
B. FIB (Forwarding Information Base) và Adjacency Table
C. CAM Table và TCAM Table
D. RIB và FIB

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* CEF sử dụng bảng FIB (bản sao tối ưu của bảng định tuyến) và bảng Adjacency (chứa thông tin Layer 2 như MAC đích đã phân giải) để chuyển tiếp gói tin tốc độ cao.

**Câu 4: Mục tiêu nào sau đây là một ví dụ điển hình về "Mục tiêu Kinh doanh"?**
A. Sử dụng giao thức OSPF
B. Tăng lợi nhuận và thị phần
C. Đạt băng thông 10 Gbps
D. Sử dụng cáp quang Single-mode

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Tăng lợi nhuận, mở rộng thị trường, giảm chi phí là các mục tiêu kinh doanh. Các phương án còn lại là mục tiêu kỹ thuật để hỗ trợ mục tiêu kinh doanh đó.

**Câu 5: "Process Switching" trên router Cisco hoạt động như thế nào?**
A. Mọi gói tin đều được xử lý bởi phần cứng ASIC.
B. Gói tin đầu tiên được xử lý bởi CPU, các gói sau dùng cache.
C. Mọi gói tin đều được xử lý bởi CPU (General Purpose Processor).
D. Sử dụng bảng FIB được nạp sẵn.

> [!success]- Xem Đáp án
> **Đáp án: C**
> *Giải thích:* Process Switching là phương thức chậm nhất vì CPU phải xử lý từng gói tin một, tra cứu bảng định tuyến và tính toán CRC cho mỗi gói.

**Câu 6: Ràng buộc (Constraint) nào thường liên quan đến thời hạn hoàn thành dự án?**
A. Budget (Ngân sách)
B. Personnel (Nhân sự)
C. Schedule (Thời gian)
D. Politics (Chính trị)

> [!success]- Xem Đáp án
> **Đáp án: C**
> *Giải thích:* Schedule (Thời gian/Tiến độ) là một ràng buộc kinh doanh quan trọng, xác định khi nào dự án phải hoàn thành.

**Câu 7: Trong kiến trúc mạng, "Scalability" đề cập đến khả năng gì?**
A. Khả năng bảo mật của mạng.
B. Khả năng mạng mở rộng (tăng số lượng user, site) mà không ảnh hưởng nghiêm trọng đến hiệu suất.
C. Khả năng mạng hoạt động 24/7.
D. Khả năng dễ dàng quản lý.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Scalability (Tính mở rộng) là khả năng hệ thống phát triển (thêm người dùng, ứng dụng, chi nhánh) mà vẫn duy trì được hiệu suất.

**Câu 8: Thành phần nào trong bảng CEF Adjacency chứa thông tin header Layer 2 đã được tính toán trước?**
A. Next Hop IP
B. Outgoing Interface
C. MAC Header rewrite string
D. Subnet Mask

> [!success]- Xem Đáp án
> **Đáp án: C**
> *Giải thích:* Bảng Adjacency chứa thông tin Layer 2 (như MAC nguồn, MAC đích, EtherType) đã được tính toán trước (rewrite string) để router chỉ việc gắn vào gói tin và đẩy đi.

**Câu 9: Giai đoạn nào trong PDIOO diễn ra sau giai đoạn Implement (Triển khai)?**
A. Design
B. Plan
C. Optimize
D. Operate

> [!success]- Xem Đáp án
> **Đáp án: D**
> *Giải thích:* Chu trình chuẩn là: Plan -> Design -> Implement -> Operate (Vận hành) -> Optimize -> Retire.

**Câu 10: Tài liệu "RFP" là viết tắt của thuật ngữ nào trong thiết kế mạng?**
A. Request for Protocol
B. Request for Proposal
C. Routing for Packet
D. Request for Price

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* RFP (Request for Proposal) là tài liệu khách hàng gửi cho các nhà cung cấp/tư vấn để yêu cầu đề xuất giải pháp thiết kế mạng.

---

## Phần 2: Hiểu (Comprehension)
*Mục tiêu: Giải thích, so sánh và tóm tắt ý nghĩa của các khái niệm.*

**Câu 11: Tại sao phương pháp thiết kế Top-Down lại ưu việt hơn phương pháp "kết nối các điểm" (connect-the-dots)?**
A. Vì nó nhanh hơn và rẻ hơn.
B. Vì nó tập trung vào việc mua thiết bị mới nhất.
C. Vì nó đảm bảo thiết kế phục vụ đúng nhu cầu ứng dụng và mục tiêu kinh doanh của tổ chức.
D. Vì nó không cần khảo sát hiện trạng.

> [!success]- Xem Đáp án
> **Đáp án: C**
> *Giải thích:* Phương pháp Top-Down tránh việc mua sắm thiết bị không phù hợp bằng cách xác định rõ yêu cầu của ứng dụng và doanh nghiệp trước khi chọn công nghệ.

**Câu 12: Sự khác biệt chính giữa RIB (Routing Information Base) và FIB (Forwarding Information Base) là gì?**
A. RIB dùng cho control plane (xây dựng bản đồ mạng), FIB dùng cho data plane (chuyển tiếp gói tin).
B. RIB nằm trên Switch, FIB nằm trên Router.
C. RIB chỉ chứa tuyến đường tĩnh, FIB chứa tuyến đường động.
D. Không có sự khác biệt.
[Image of Control Plane vs Data Plane in Cisco Router]

> [!success]- Xem Đáp án
> **Đáp án: A**
> *Giải thích:* RIB là bảng định tuyến tổng thể (control plane), trong khi FIB là bảng được tối ưu hóa từ RIB để phục vụ việc chuyển tiếp gói tin tốc độ cao (data plane) trong CEF.

**Câu 13: "Chính trị nơi công sở" (Office Politics) có thể ảnh hưởng tiêu cực đến thiết kế mạng như thế nào?**
A. Nó giúp tăng ngân sách dự án.
B. Các nhóm nội bộ có thể che giấu thông tin hoặc cản trở dự án để bảo vệ quyền lợi riêng, dẫn đến thiết kế thiếu chính xác.
C. Nó giúp chọn được thiết bị tốt nhất.
D. Nó làm tăng tốc độ triển khai.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Các vấn đề chính trị (tranh giành quyền lực, mâu thuẫn giữa các bộ phận IT và Telecom, v.v.) có thể làm sai lệch thông tin yêu cầu hoặc gây khó khăn khi triển khai.

**Câu 14: Khi một gói tin đến router, nếu không tìm thấy mục nào khớp trong bảng FIB (và không có default route), điều gì sẽ xảy ra?**
A. Gói tin được gửi đến CPU để xử lý.
B. Gói tin bị hủy (drop).
C. Gói tin được gửi trả lại nguồn.
D. Gói tin được flood ra tất cả các cổng.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Trong cơ chế CEF, nếu đích đến không có trong bảng FIB (và không có default route), gói tin sẽ bị hủy ngay tại phần cứng (drop).

**Câu 15: Ý nghĩa của "Availability" (Tính sẵn sàng) trong bối cảnh thiết kế mạng là gì?**
A. Tốc độ truyền tải dữ liệu cao nhất.
B. Thời gian mạng hoạt động bình thường so với tổng thời gian (thường tính bằng phần trăm, ví dụ 99.99%).
C. Khả năng mở rộng mạng lưới.
D. Khả năng bảo mật dữ liệu.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Availability là tỷ lệ thời gian hệ thống hoạt động ổn định (uptime) trên tổng thời gian, thường được biểu thị bằng các con số "nines" (ví dụ 5 số 9 là 99.999%).

**Câu 16: Tại sao việc hiểu "Cấu trúc tổ chức" (Organizational Chart) của khách hàng lại quan trọng?**
A. Để biết ai là người ký séc thanh toán.
B. Để hiểu luồng thông tin di chuyển như thế nào giữa các phòng ban và xác định ai là người ra quyết định.
C. Để thiết kế sơ đồ đi dây cáp.
D. Để chọn giao thức định tuyến.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Sơ đồ tổ chức giúp xác định các bên liên quan (stakeholders), người dùng chính và luồng dữ liệu nghiệp vụ.

**Câu 17: Khái niệm "Punt" trong kiến trúc chuyển tiếp Cisco có nghĩa là gì?**
A. Chuyển tiếp gói tin bằng phần cứng.
B. Chuyển gói tin không thể xử lý bằng CEF lên CPU để xử lý mềm (ví dụ: gói tin có option IP, gói tin ARP).
C. Gửi gói tin ra giao diện Null0.
D. Nén gói tin.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* "Punt" là hành động đẩy gói tin từ data plane lên control plane (CPU) khi gói tin quá phức tạp hoặc yêu cầu xử lý đặc biệt mà phần cứng không làm được.

**Câu 18: Mối quan hệ giữa mục tiêu kinh doanh "Tăng năng suất nhân viên" và mục tiêu kỹ thuật là gì?**
A. Cần giảm băng thông để nhân viên không lướt web.
B. Cần tăng độ trễ mạng.
C. Cần tối ưu hóa thời gian phản hồi (Response time) và độ tin cậy mạng để ứng dụng chạy mượt mà.
D. Không liên quan.

> [!success]- Xem Đáp án
> **Đáp án: C**
> *Giải thích:* Để nhân viên làm việc hiệu quả (mục tiêu kinh doanh), hệ thống mạng phải phản hồi nhanh và không bị gián đoạn (mục tiêu kỹ thuật).

**Câu 19: "Load Sharing" (Chia sẻ tải) khác với "Redundancy" (Dự phòng) ở điểm nào?**
A. Redundancy chỉ dùng khi thiết bị chính hỏng, Load Sharing dùng tất cả các đường cùng lúc.
B. Redundancy tốn kém hơn.
C. Load Sharing không hỗ trợ dự phòng.
D. Chúng là một.

> [!success]- Xem Đáp án
> **Đáp án: A**
> *Giải thích:* Dự phòng (Redundancy) thường ám chỉ việc có thiết bị/đường truyền thay thế khi sự cố xảy ra (Active-Standby). Chia sẻ tải (Load Sharing) tận dụng tất cả các đường truyền/thiết bị để tăng hiệu suất trong điều kiện bình thường (Active-Active).

**Câu 20: Tại sao nên sử dụng mô hình thiết kế mạng phân cấp (Hierarchical Network Design)?**
A. Để làm cho mạng phức tạp hơn.
B. Để giảm chi phí mua thiết bị.
C. Để chia mạng thành các phần nhỏ dễ quản lý, dễ mở rộng và cô lập lỗi tốt hơn.
D. Để loại bỏ hoàn toàn Router.

> [!success]- Xem Đáp án
> **Đáp án: C**
> *Giải thích:* Mô hình phân cấp (Core, Distribution, Access) giúp tối ưu hóa luồng dữ liệu, dễ dàng mở rộng và khoanh vùng sự cố.

---

## Phần 3: Vận dụng (Application)
*Mục tiêu: Áp dụng kiến thức vào các tình huống giả định đơn giản.*

**Câu 21: Một công ty muốn triển khai VoIP (Voice over IP). Yêu cầu kỹ thuật quan trọng nhất cần thiết kế là gì?**
A. Tăng dung lượng ổ cứng Server.
B. Giảm độ trễ (Latency) và độ biến thiên trễ (Jitter).
C. Sử dụng cáp Cat5 cũ.
D. Chỉ sử dụng Wifi.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Ứng dụng thời gian thực như VoIP rất nhạy cảm với độ trễ và jitter. Nếu jitter cao, chất lượng thoại sẽ bị méo hoặc ngắt quãng.

**Câu 22: Bạn đang thiết kế mạng cho một thư viện công cộng (ngân sách thấp). Bạn nên ưu tiên điều gì?**
A. Độ sẵn sàng 99.999%.
B. Tính khả dụng (Usability) và chi phí thấp (Affordability), chấp nhận hiệu năng ở mức trung bình.
C. Bảo mật cấp độ quân sự.
D. Sử dụng thiết bị Core Switch đắt tiền nhất.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Dựa trên ràng buộc ngân sách và mục tiêu kinh doanh (cung cấp truy cập công cộng), việc cân đối chi phí và tính dễ sử dụng là ưu tiên hàng đầu, thay vì các tính năng cao cấp không cần thiết.

**Câu 23: Một Router có bảng FIB chứa: 192.168.1.0/24 -> Port A và 192.168.1.0/26 -> Port B. Gói tin đến 192.168.1.10 sẽ đi đường nào?**
A. Port A.
B. Port B.
C. Load balancing cả hai.
D. Drop.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Nguyên tắc "Longest Prefix Match" (Khớp tiền tố dài nhất). /26 dài hơn /24, và địa chỉ 192.168.1.10 thuộc cả hai subnet, nên Router chọn subnet cụ thể hơn là /26.

**Câu 24: Trong dự án KTX (The Muse), mục tiêu "Cung cấp Wifi phủ sóng toàn bộ các phòng và khu vực chung" giải quyết vấn đề kinh doanh gì?**
A. Tăng chi phí điện năng.
B. Nâng cao trải nghiệm sống và học tập của sinh viên, tạo lợi thế cạnh tranh cho khu trọ.
C. Để sinh viên chơi game nhiều hơn.
D. Để giám sát sinh viên.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Đáp ứng nhu cầu kết nối mọi lúc mọi nơi của sinh viên là một tiện ích quan trọng giúp thu hút và giữ chân khách hàng (sinh viên).

**Câu 25: Khi thiết kế mạng cho công ty FastPay (tài chính), yếu tố nào sau đây là quan trọng nhất?**
A. Tốc độ truy cập Facebook nhanh.
B. Bảo mật (Security) và Bảo vệ dữ liệu (Data Integrity).
C. Sử dụng thiết bị rẻ tiền.
D. Cho phép nhân viên tự do cài phần mềm.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Đối với tổ chức tài chính, việc bảo vệ dữ liệu giao dịch và thông tin khách hàng là mục tiêu kinh doanh sống còn.

**Câu 26: Bạn đang ở giai đoạn "Optimize" của PDIOO. Bạn phát hiện băng thông đường truyền Internet thường xuyên đạt 95% vào giờ cao điểm. Bạn nên làm gì?**
A. Mua ngay router mới.
B. Phân tích lưu lượng để xác định nguyên nhân, sau đó cấu hình QoS hoặc nâng cấp băng thông nếu cần.
C. Tắt Internet vào giờ cao điểm.
D. Quay lại giai đoạn Plan.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Giai đoạn Optimize dựa trên dữ liệu vận hành thực tế để tinh chỉnh. Việc phân tích trước khi hành động giúp giải quyết vấn đề tận gốc.

**Câu 27: Một doanh nghiệp yêu cầu hệ thống mạng phải hoạt động ngay cả khi một Core Switch bị hỏng. Bạn sẽ thiết kế như thế nào?**
A. Sử dụng một Core Switch duy nhất nhưng đắt tiền.
B. Sử dụng hai Core Switch chạy chế độ dự phòng (Redundancy) với các giao thức như HSRP/VRRP.
C. Chỉ sử dụng Router, không dùng Switch.
D. Yêu cầu nhân viên nghỉ làm khi Switch hỏng.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Redundancy (Dự phòng) thiết bị và đường truyền là giải pháp tiêu chuẩn để đảm bảo tính sẵn sàng cao (Availability).

**Câu 28: Khi thu thập thông tin hiện trạng mạng (Characterizing Existing Network), bạn thấy sơ đồ mạng cũ đã lạc hậu. Hành động đúng là gì?**
A. Bỏ qua và thiết kế mới hoàn toàn.
B. Sử dụng các công cụ khám phá (như CDP, LLDP) và khảo sát thực tế để cập nhật lại sơ đồ mạng.
C. Hỏi bảo vệ tòa nhà.
D. Giả định sơ đồ cũ vẫn đúng.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Hiểu rõ hiện trạng là bước quan trọng để thiết kế nâng cấp thành công. Việc cập nhật tài liệu giúp tránh các rủi ro không đáng có khi triển khai.

**Câu 29: Nếu công ty có chính sách "Cấm sử dụng thiết bị Huawei", đây là loại ràng buộc nào?**
A. Ràng buộc kỹ thuật.
B. Ràng buộc về ngân sách.
C. Ràng buộc về chính sách (Policy Constraint).
D. Ràng buộc về thời gian.

> [!success]- Xem Đáp án
> **Đáp án: C**
> *Giải thích:* Đây là một quy định phi kỹ thuật xuất phát từ chính sách nội bộ hoặc quy định pháp luật.

**Câu 30: Trong mô hình thiết kế KTX, việc sử dụng "Wall-plate AP" (AP gắn tường) trong mỗi phòng thay vì AP hành lang nhằm mục đích gì?**
A. Để trang trí phòng.
B. Đảm bảo sóng Wifi mạnh nhất cho từng phòng và tránh can nhiễu, đáp ứng mục tiêu trải nghiệm người dùng cao.
C. Để tiết kiệm chi phí dây mạng.
D. Để sinh viên không thể tự lắp Router riêng.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* AP gắn tường cung cấp vùng phủ sóng riêng biệt ("corner-to-corner") cho từng phòng, đảm bảo băng thông và tín hiệu tốt hơn so với việc xuyên tường từ hành lang.

---

## Phần 4: Vận dụng cao (High Application)
*Mục tiêu: Phân tích, đánh giá và giải quyết các vấn đề phức tạp, kết hợp nhiều kiến thức.*

**Câu 31: Một công ty muốn triển khai "Zero Trust Security" (như FastPay). Điều này ảnh hưởng thế nào đến thiết kế lớp Access?**
A. Không ảnh hưởng gì.
B. Cần triển khai 802.1X để xác thực mọi thiết bị kết nối, phân đoạn mạng (VLAN) chi tiết và áp dụng ACL chặt chẽ ngay tại biên.
C. Chỉ cần cài Firewall ở Core.
D. Chỉ cần đặt mật khẩu Wifi mạnh.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Zero Trust yêu cầu "không tin tưởng bất kỳ ai, xác thực mọi thứ". Do đó, việc kiểm soát truy cập phải được thực hiện ngay tại điểm kết nối đầu tiên (Access Layer).

**Câu 32: Trong dự án KTX, tại sao lại cần tách biệt VLAN-Guest và VLAN-Student?**
A. Để tiết kiệm địa chỉ IP.
B. Để đảm bảo an ninh (Security): Khách không được phép truy cập vào tài nguyên nội bộ của sinh viên hoặc hệ thống quản lý.
C. Để Router hoạt động nhanh hơn.
D. Vì thiết bị TP-Link bắt buộc phải làm vậy.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Phân tách VLAN (Traffic Isolation) là biện pháp bảo mật cơ bản để ngăn chặn truy cập trái phép giữa các nhóm người dùng có mức độ tin cậy khác nhau.

**Câu 33: Khi thiết kế mạng cho FastPay, việc sử dụng đường truyền "Internet Leased Line" thay vì FTTH thông thường cho trụ sở chính nhằm mục đích gì?**
A. Để có giá rẻ hơn.
B. Để đảm bảo cam kết băng thông quốc tế, độ trễ thấp và độ ổn định cao (SLA) cho các giao dịch tài chính.
C. Để dễ cấu hình hơn.
D. Để có IP động.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Leased Line cung cấp cam kết chất lượng dịch vụ (SLA) cao hơn, phù hợp với yêu cầu khắt khe về độ trễ và tính sẵn sàng của dịch vụ tài chính.

**Câu 34: Một hệ thống mạng đang gặp vấn đề CPU router luôn ở mức 100%. Kiểm tra thấy tính năng "Process Switching" đang bật. Giải pháp tối ưu là gì?**
A. Mua router mới mạnh hơn.
B. Bật tính năng CEF (Cisco Express Forwarding) để chuyển việc xử lý gói tin xuống phần cứng (ASIC/FIB).
C. Chặn bớt người dùng.
D. Khởi động lại router liên tục.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Chuyển từ Process Switching sang CEF giúp giảm tải CPU đáng kể vì gói tin được xử lý dựa trên bảng FIB đã tính toán trước, không cần CPU can thiệp vào từng gói.

**Câu 35: Trong thiết kế cho FastPay, tại sao lại cần Firewall riêng cho VPC (Virtual Private Cloud)?**
A. Vì nhà cung cấp Cloud không có tường lửa.
B. Để đảm bảo bảo vệ chuyên sâu (Defense in depth) ở lớp 7 (Application) và chống DDoS cụ thể cho các ứng dụng giao dịch, bổ sung cho tường lửa của nhà cung cấp hạ tầng.
C. Để tăng chi phí dự án.
D. Để kết nối VPN.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Firewall Layer 7 (như Palo Alto VM-Series) cung cấp khả năng kiểm soát ứng dụng và chống các mối đe dọa nâng cao mà tường lửa cơ bản của Cloud (thường chỉ là Layer 3/4) không làm được.

**Câu 36: Bạn cần kết nối 3 chi nhánh của FastPay. Tại sao chọn mô hình VPN Site-to-Site thay vì thuê đường dây riêng (Leased Line) giữa các chi nhánh?**
A. VPN Site-to-Site bảo mật hơn Leased Line.
B. VPN Site-to-Site tận dụng đường truyền Internet có sẵn, giúp tiết kiệm chi phí đáng kể so với thuê đường riêng đường dài, trong khi vẫn đảm bảo bảo mật nhờ mã hóa (IPSec).
C. VPN nhanh hơn.
D. Leased Line không hỗ trợ kết nối chi nhánh.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Đây là bài toán cân đối giữa chi phí (Affordability) và yêu cầu kỹ thuật. VPN là giải pháp hiệu quả về chi phí (Cost-effective) để kết nối các điểm xa nhau qua hạ tầng Internet công cộng.

**Câu 37: Trong sơ đồ mạng KTX, tại sao lại dùng Core Switch là dòng L3 (Layer 3) thay vì chỉ dùng Router và Switch L2?**
A. Để Switch L3 thực hiện định tuyến Inter-VLAN tốc độ cao (wire-speed routing), giảm tải cho Router biên, giúp Router tập trung vào NAT và kết nối WAN.
B. Vì Switch L3 rẻ hơn.
C. Vì Router không hỗ trợ VLAN.
D. Để cho đẹp đội hình.

> [!success]- Xem Đáp án
> **Đáp án: A**
> *Giải thích:* Switch L3 có khả năng định tuyến gói tin bằng phần cứng (ASIC) với tốc độ rất cao, phù hợp để làm Gateway cho các VLAN trong mạng LAN, trong khi Router thường xử lý bằng CPU chậm hơn.

**Câu 38: Trong kịch bản tự động hóa bằng Ansible, "Idempotency" (Tính lũy đẳng) có nghĩa là gì?
A. Chạy script nhiều lần sẽ tạo ra nhiều VLAN trùng nhau.
B. Chạy script nhiều lần cũng chỉ cho ra một kết quả mong muốn duy nhất (nếu cấu hình đã đúng thì không làm gì thêm).
C. Script sẽ tự động xóa hết cấu hình cũ.
D. Script sẽ chạy nhanh hơn lần đầu.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Idempotency đảm bảo an toàn khi chạy tự động hóa. Nó kiểm tra trạng thái hiện tại trước khi thay đổi, đảm bảo cấu hình cuối cùng luôn nhất quán mà không gây lỗi do lặp lại lệnh.

**Câu 39: Khi thiết kế địa chỉ IP cho KTX, tại sao nên dùng dải mạng /20 (ví dụ 10.10.0.0/20) cho VLAN Student thay vì /24?
A. Vì sinh viên cần mạng nhanh hơn.
B. Vì số lượng sinh viên và thiết bị (laptop, điện thoại) có thể rất lớn (> 254 thiết bị), cần dải IP rộng để tránh cạn kiệt địa chỉ IP (Scalability).
C. Vì /20 dễ cấu hình hơn.
D. Vì /24 không hỗ trợ DHCP.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Khả năng mở rộng (Scalability) yêu cầu quy hoạch địa chỉ IP phải dự trù cho sự phát triển số lượng thiết bị trong tương lai.

**Câu 40: Để giám sát hệ thống mạng (như trong bài tập Zabbix), giao thức nào là quan trọng nhất cần được cấu hình trên các thiết bị mạng?
A. HTTP
B. SNMP (Simple Network Management Protocol)
C. FTP
D. SMTP

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* SNMP là giao thức tiêu chuẩn để thu thập thông tin trạng thái, hiệu suất từ các thiết bị mạng về hệ thống giám sát trung tâm (NMS).

---

## Phần 5: Hiểu sâu / Thực tiễn (Deep Understanding/Practical)
*Mục tiêu: Liên hệ kiến thức với các dự án thực tế trong tài liệu (KTX, FastPay, Zabbix).*

**Câu 41: Trong dự án KTX "The Muse", giải pháp "Omada Software Controller" đóng vai trò gì trong mô hình quản lý?**
A. Là một Router để kết nối Internet.
B. Là phần mềm quản lý tập trung (Centralized Management), giúp IT cấu hình, giám sát toàn bộ AP và Switch từ một giao diện duy nhất, giảm chi phí vận hành.
C. Là phần mềm diệt virus cho sinh viên.
D. Là phần mềm kế toán.
[Image of Omada SDN Controller Network Topology]

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Controller giúp thực hiện mục tiêu "Easy centralized management" (Quản lý tập trung dễ dàng), cho phép nhân viên IT giám sát trạng thái mạng mọi lúc mọi nơi.

**Câu 42: Trong báo cáo FastPay, tại sao lại chọn "Palo Alto PA-3410" làm Firewall trụ sở chính?**
A. Vì nó có màu đẹp.
B. Vì nó là dòng Next-Generation Firewall (NGFW) hỗ trợ các tính năng bảo mật nâng cao (Layer 7 inspection, Threat Prevention) phù hợp với yêu cầu bảo mật cao của ngành tài chính.
C. Vì nó rẻ nhất thị trường.
D. Vì nó hỗ trợ Wifi.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Đáp ứng yêu cầu "Bảo mật cao: Firewall Layer 7" của doanh nghiệp. Palo Alto nổi tiếng với khả năng kiểm soát ứng dụng và chống xâm nhập sâu.

**Câu 43: Trong bài thực hành Zabbix, tại sao sinh viên chọn Ubuntu 22.04 thay vì 24.04 để cài đặt Zabbix Server?**
A. Vì Ubuntu 24.04 quá nặng.
B. Vì tính tương thích (Compatibility) và ổn định. Phiên bản mới (24.04) có thể chưa được hỗ trợ tốt hoặc gây lỗi cài đặt với phiên bản Zabbix hiện tại.
C. Vì giảng viên yêu cầu.
D. Vì Ubuntu 22.04 có giao diện đẹp hơn.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Trong thực tế triển khai (Implement phase), việc chọn phiên bản phần mềm tương thích (Stable/LTS) là rất quan trọng để tránh các lỗi không mong muốn, như sinh viên đã gặp phải trong báo cáo.

**Câu 44: Trong mô hình mạng KTX, tại sao "Core Switch" lại nối với "Router" bằng một đường "FTTH"? (Lưu ý: Sơ đồ logic trong tài liệu vẽ Router nối Internet qua FTTH).**
A. Đây là mô hình kết nối Internet gia đình, không phải mô hình Enterprise chuẩn.
B. FTTH (Fiber to the Home) là công nghệ truy cập Internet băng thông rộng phổ biến, phù hợp chi phí cho KTX, cung cấp kết nối Uplink ra Internet cho toàn hệ thống.
C. Vì cáp đồng không nối được.
D. Cả A và B đều có lý.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* FTTH là giải pháp WAN hiệu quả về chi phí cho các mô hình như KTX, cung cấp băng thông đủ lớn để chia sẻ cho nhiều người dùng.

**Câu 45: Trong bài tập FastPay, việc sử dụng "HSRP" (Hot Standby Router Protocol) giải quyết vấn đề gì?**
A. Cân bằng tải Internet.
B. Tăng tốc độ mạng.
C. Đảm bảo tính sẵn sàng cao (High Availability) cho Gateway. Nếu Router chính (Active) hỏng, Router phụ (Standby) sẽ tự động thay thế mà không làm mất kết nối của người dùng.
D. Để cấp IP động.
[Image of HSRP Active Standby Router Topology]

> [!success]- Xem Đáp án
> **Đáp án: C**
> *Giải thích:* HSRP tạo ra một Gateway ảo. Đây là kỹ thuật dự phòng (Redundancy) quan trọng ở lớp Distribution/Core để tránh điểm đơn lỗi (Single Point of Failure).

**Câu 46: Trong tài liệu về Ansible, lợi ích lớn nhất của việc dùng "Playbook" để cấu hình VLAN là gì?**
A. Không cần biết lệnh Cisco IOS.
B. Có thể cấu hình hàng loạt thiết bị (Scalability) một cách nhất quán (Consistency), giảm sai sót do con người và lưu lại lịch sử cấu hình (Infrastructure as Code).
C. Ansible chạy nhanh hơn gõ tay.
D. Ansible là phần mềm miễn phí.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Tự động hóa (Automation) giúp giải quyết bài toán quản lý quy mô lớn và đảm bảo tính đồng nhất trong cấu hình, là xu hướng của quản trị mạng hiện đại.

**Câu 47: Trong dự án KTX, thiết bị "EAP225-Outdoor" được sử dụng ở đâu và vì sao?**
A. Trong phòng sinh viên để sóng mạnh.
B. Tại khu vực hồ bơi và ngoài trời, vì nó có khả năng chịu được thời tiết (chống nước, bụi) và phủ sóng rộng.
C. Trong phòng Server để làm mát.
D. Tại quầy lễ tân.

> [!success]- Xem Đáp án
> **Đáp án: B**
> *Giải thích:* Việc lựa chọn thiết bị phải phù hợp với môi trường vật lý (Physical environment). Thiết bị Outdoor được thiết kế riêng cho điều kiện khắc nghiệt bên ngoài.

**Câu 48: Tại sao trong sơ đồ FastPay lại có các vùng "DMZ" (Demilitarized Zone)?**
A. Để đặt các máy chủ chứa dữ liệu mật.
B. Để đặt các máy chủ công khai (Web, Mail, DNS) cần truy cập từ Internet, nhằm cô lập chúng với mạng nội bộ (Internal Network), tăng cường bảo mật nếu các máy chủ này bị tấn công.
C. Để nhân viên nghỉ ngơi.
D. Để đặt máy chủ quản trị mạng.
[Image of DMZ Network Architecture with Firewall]

> [!success]- Xem