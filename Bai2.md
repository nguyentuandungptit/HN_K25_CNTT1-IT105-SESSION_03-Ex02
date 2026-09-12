# BÁO CÁO HIỆU CHỈNH YÊU CẦU PHÂN HỆ KHO DƯỢC RIKKEICARE
**Khóa học:** Phân tích & Thiết kế Hệ thống (IT105)  
**Session:** 03 - Vận dụng cơ bản: Chuẩn hóa Khảo sát & Phân loại Yêu cầu  
**Vai trò thực hiện:** Lead System Analyst (Lead SA)

---

## PHẦN 1: BÓC TÁCH VÀ SỬA LỖI SAI NGHIỆP VỤ (3 LỖI SAI)

### 1. Hạng mục 1: Môi trường Hệ thống
- **Nguyên nhân sai:** Chuyên viên tập sự nhầm lẫn giữa quy định pháp lý hành chính bên ngoài với môi trường vận hành nội bộ. Thông tư của Bộ Y tế là quy định pháp lý vĩ mô do cơ quan quản lý nhà nước ban hành, tổ chức không thể tự thay đổi hay kiểm soát trực tiếp mà bắt buộc phải tuân thủ.
- **Hậu quả nghiệp vụ:** Hệ thống thiếu cơ chế cập nhật tự động hoặc thay đổi linh hoạt theo các thông tư/quy định mới của Bộ Y tế, dẫn đến nguy cơ RikkeiCare vi phạm pháp luật y tế và bị đình chỉ hoạt động.

### 2. Hạng mục 2: Khảo sát quy trình AS-IS
- **Nguyên nhân sai:** Chuyên viên bỏ qua bằng chứng thực tế tại hiện trường (bệnh nhân khiếu nại nhận thuốc quá hạn) và chủ quan tin vào trí nhớ cá nhân của Dược sĩ thay vì đánh giá quy trình ghi chép/kiểm soát thực tế.
- **Hậu quả nghiệp vụ:** Hệ thống không xây dựng các tính năng cảnh báo và tự động chặn xuất kho thuốc hết hạn/sắp hết hạn, dẫn đến rủi ro nghiêm trọng đến sức khỏe bệnh nhân, tổn hại uy tín bệnh viện và đối mặt với các tranh chấp pháp lý.

### 3. Hạng mục 3: Phân loại Yêu cầu
- **Nguyên nhân sai:** Lẫn lộn giữa Yêu cầu Chức năng (FR) với Yêu cầu Phi chức năng (NFR); hành động "tự động khóa các lô thuốc hết hạn" là một tính năng xử lý nghiệp vụ cụ thể của hệ thống (dùng động từ hành động) chứ không phải chỉ số chất lượng hay hiệu năng.
- **Hậu quả nghiệp vụ:** Đội ngũ lập trình viên sẽ xếp tính năng này vào nhóm tiêu chuẩn kỹ thuật/ràng buộc phi chức năng thay vì thiết kế luồng nghiệp vụ kiểm soát kho (Business Logic Layer), khiến phần mềm bị thiếu tính năng chặn xuất hàng tự động cốt lõi.

---

## PHẦN 2: BẢNG PHÂN LOẠI MÔI TRƯỜNG & YÊU CẦU (ĐÃ ĐIỀN KHUYẾT)

### 1. Bảng Phân loại Môi trường Hệ thống

| Yếu tố khảo sát tại Kho Dược | Thuộc loại Môi trường | Tác động trực tiếp đến phần mềm |
| :--- | :--- | :--- |
| **Kỹ năng vi tính & thói quen ghi sổ của Dược sĩ** | Môi trường Nội bộ | Cần giao diện tối giản, hỗ trợ quét mã vạch để thao tác nhập/xuất kho nhanh chóng, chính xác. |
| **Thông tư & Chế tài xử phạt của Bộ Y tế** | Môi trường Bên ngoài | Bắt buộc khóa tự động thuốc hết hạn, lưu audit log toàn bộ lịch sử xuất/nhập thuốc phục vụ thanh tra. |
| **Hạ tầng máy chủ và mạng LAN nội bộ bệnh viện** | **Môi trường Nội bộ** | **Bắt buộc hỗ trợ cơ chế xác nhận kép (Dual-Authorization) qua mạng LAN/máy chủ nội bộ giữa Dược sĩ và Dược sĩ Trưởng khoa đối với danh mục thuốc kiểm soát đặc biệt (RESTRICTED - thuốc hướng thần, gây nghiện) trước khi xuất kho; đồng thời đảm bảo hệ thống phản hồi tức thì và hoạt động ổn định 24/7 kể cả khi mất kết nối Internet ngoài.** |

---

### 2. Đặc tả Yêu cầu Chức năng (FR) và Phi chức năng (NFR) cho Phân hệ Kho Dược

#### A. Yêu cầu Chức năng (Functional Requirement - FR)
- **Mã yêu cầu:** `FR-PHARM-01` (Cơ chế Duyệt Kép Thuốc Kiểm Soát Đặc Biệt)
- **Nội dung đặc tả:**  
  *"Khi Dược sĩ thực hiện lệnh xuất kho đối với các loại thuốc thuộc danh mục kiểm soát đặc biệt (RESTRICTED: thuốc hướng thần, gây nghiện), hệ thống bắt buộc phải tạm dừng lệnh xuất và yêu cầu xác nhận duyệt kép (Dual-Authorization) bằng mã OTP/chữ ký số từ Dược sĩ Trưởng khoa Dược trước khi hoàn tất tạo phiếu xuất kho."*

#### B. Yêu cầu Phi chức năng (Non-Functional Requirement - NFR)
- **Mã yêu cầu:** `NFR-PHARM-01` (Hiệu năng & Độ khả dụng xử lý giao dịch kho)
- **Nội dung đặc tả:**  
  *"Hệ thống Kho Dược phải đảm bảo độ khả dụng (System Availability) đạt tối thiểu **99.99%** (thời gian downtime không quá 52.6 phút/năm), xử lý và ghi nhận giao dịch xuất/nhập kho (bao gồm cả bước kiểm tra cảnh báo hạn dùng) trong thời gian phản hồi **$\le 1.0$ giây** ở điều kiện chịu tải **500 giao dịch đồng thời (Concurrent Transactions)** trên mạng LAN nội bộ."*
