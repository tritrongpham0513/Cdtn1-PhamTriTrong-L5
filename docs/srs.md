# Đặc tả Yêu cầu Phần mềm (SRS) – Rút gọn

> **Dự án:** Smart CRM – Mekong Mobile  
> **Luồng nghiệp vụ:** L5 – Kho linh kiện thay thế  
> **Sinh viên:** Phạm Trí Trọng – MSSV: 2374802010525  
> **Track:** Software Engineering (SE)  
> **Phiên bản:** 1.0 – Học kỳ 1, Năm học 2026 – 2027  

---

## Mục lục

1. [Giới thiệu và Phạm vi](#1-giới-thiệu-và-phạm-vi)  
2. [Các bên liên quan](#2-các-bên-liên-quan)  
3. [Yêu cầu chức năng (FR) và User Story](#3-yêu-cầu-chức-năng-fr-và-user-story)  
4. [Yêu cầu phi chức năng (NFR)](#4-yêu-cầu-phi-chức-năng-nfr)  
5. [Ràng buộc và quy tắc nghiệp vụ](#5-ràng-buộc-và-quy-tắc-nghiệp-vụ)  
6. [Bảng truy vết yêu cầu (Traceability Matrix)](#6-bảng-truy-vết-traceability-matrix)  

---

## 1. Giới thiệu và Phạm vi

### 1.1. Bối cảnh

Công ty Cổ phần Bán lẻ & Dịch vụ **Mekong Mobile** là chuỗi bán lẻ và sửa chữa thiết bị di động, hiện vận hành **24 cửa hàng** và **6 trung tâm bảo hành** với khoảng 38 kỹ thuật viên phụ trách sửa chữa, trung bình tiếp nhận **260 phiếu bảo hành mỗi tháng**.

Hiện tại, tồn kho linh kiện thay thế tại các trung tâm bảo hành được ghi chép **thủ công bằng sổ giấy**, chỉ cập nhật vào cuối ngày. Thực trạng này gây ra các vấn đề trực tiếp:

- Số liệu tồn kho trong sổ **lệch 3–8%** so với thực tế trong kho.
- Kỹ thuật viên nhận phiếu sửa chữa xong **mới phát hiện hết linh kiện**, buộc phải hẹn lại khách — xảy ra khoảng **20 lần/tháng**, ảnh hưởng trực tiếp đến uy tín dịch vụ.
- Quản lý trung tâm **không nhận được cảnh báo sớm** khi linh kiện sắp cạn, dẫn đến thiếu hàng đột ngột.

Module **L5 – Kho linh kiện thay thế** được xây dựng nhằm số hóa toàn bộ quy trình nhập – xuất kho, đảm bảo tồn kho luôn chính xác tức thời và giảm thiểu tình trạng hẹn lại khách do thiếu linh kiện.


### 1.2. Luồng nghiệp vụ chọn và Phạm vi
> **Quản lý kho linh kiện thay thế: quản lý trung tâm ghi nhập kho, kỹ thuật viên xuất linh kiện cho phiếu bảo hành, hệ thống chặn xuất quá tồn, tự cập nhật số tồn và cảnh báo khi tồn dưới ngưỡng tối thiểu, đến khi mọi lần nhập – xuất được lưu vào lịch sử.**

### 1.3. Điều chủ ý KHÔNG làm (Mức WON'T của MoSCoW)
Để bảo đảm ranh giới của một đồ án cá nhân (xây dựng prototype 1–2 module cốt lõi chạy cục bộ):
1. **Không** xây dựng module tự động đặt hàng nhà cung cấp (Procurement / Purchasing System).
2. **Không** điều chuyển linh kiện tự động giữa các trung tâm bảo hành khác nhau (Inter-center transfer).
3. **Không** áp dụng mô hình AI/ML dự báo nhu cầu linh kiện theo chuỗi thời gian (chuyển sang học phần CĐTN2).
4. **Không** tích hợp cổng thanh toán trực tuyến tiền linh kiện cho khách hàng.

### 1.4. Bảng thuật ngữ dùng trong toàn tài liệu (Glossary)

| Thuật ngữ | Định nghĩa | Tên kỹ thuật |
|---|---|---|
| **Phiếu bảo hành** | Một yêu cầu bảo hành hoặc sửa chữa được ghi nhận, có mã duy nhất và vòng đời trạng thái | `ticket` |
| **Trạng thái phiếu** | Vị trí hiện tại của phiếu trong vòng đời: Mới → Đã phân công → Đang xử lý → Chờ linh kiện → Hoàn tất → Đã đóng | `ticket_status` |
| **Kỹ thuật viên** | Nhân viên thực hiện sửa chữa, có danh sách tay nghề và địa bàn làm việc | `technician` |
| **Linh kiện** | Bộ phận thay thế dùng trong sửa chữa, có mã và tồn kho theo trung tâm | `part` |
| **Tồn kho linh kiện** | Số lượng hiện có của một linh kiện tại một trung tâm bảo hành cụ thể | `part_stock` |
| **Trung tâm bảo hành** | Cơ sở nơi thực hiện bảo hành (Mekong Mobile hiện có 6 trung tâm) | `service_center` |
| **Giao dịch linh kiện** | Một lần nhập hoặc xuất linh kiện, ghi nhận đầy đủ ai, khi nào, bao nhiêu | `part_transaction` |
| **Linh kiện phiếu** | Linh kiện đã được xuất gắn với một phiếu bảo hành cụ thể | `ticket_part` |
| **Ngưỡng tối thiểu** | Mức tồn kho mà dưới đó hệ thống hiển thị cảnh báo cần đặt hàng bổ sung | `min_threshold` |
| **Nhập kho** | Loại giao dịch ghi nhận linh kiện mới về trung tâm, làm **tăng** số tồn | `IMPORT` *(giá trị của `transaction_type`)* |
| **Xuất kho** | Loại giao dịch ghi nhận linh kiện được lấy ra để sửa chữa, bắt buộc gắn `ticket_id`, làm **giảm** số tồn | `EXPORT` *(giá trị của `transaction_type`)* |
| **Quản lý trung tâm** | Nhân viên phụ trách vận hành trung tâm bảo hành, có quyền nhập kho và cấu hình ngưỡng tồn | `center_manager` |


---

## 2. Các bên liên quan và vai trò

| Vai trò | Số lượng | Quyền hạn — Được làm | Không được làm |
|---|:---:|---|---|
| **Kỹ thuật viên** | 38 | Xem **tồn kho linh kiện** tại trung tâm mình; tạo **giao dịch xuất kho** cho **phiếu bảo hành** của trung tâm mình đang ở trạng thái **Đang xử lý** hoặc **Chờ linh kiện** | Nhập kho; xem lịch sử **giao dịch linh kiện**; chỉnh sửa **ngưỡng tối thiểu**; xem dữ liệu trung tâm khác |
| **Quản lý trung tâm** | 6 | Xem **tồn kho linh kiện** tại trung tâm mình; tạo **giao dịch nhập kho**; cấu hình **ngưỡng tối thiểu** cho từng linh kiện; xem toàn bộ lịch sử **giao dịch linh kiện**; nhận cảnh báo khi **tồn kho linh kiện** xuống dưới **ngưỡng tối thiểu** | Xuất kho cho **phiếu bảo hành**; sửa hoặc xóa **giao dịch linh kiện** đã ghi nhận; xem hoặc chỉnh sửa dữ liệu trung tâm khác |

---





## 3. Yêu cầu chức năng (FR) và User Story

### 3.1. Danh sách yêu cầu chức năng

| Mã FR | Tên yêu cầu | Mô tả | US liên quan | MoSCoW |
|---|---|---|:---:|:---:|
| **FR-01** | Xem tồn kho linh kiện | Hiển thị danh sách linh kiện của trung tâm mà người dùng thuộc về, gồm mã, tên, số tồn, ngưỡng tối thiểu; tìm được theo mã hoặc tên; không hiển thị dữ liệu trung tâm khác | US-01 | MUST |
| **FR-02** | Ghi nhận nhập kho | Quản lý trung tâm nhập kho một linh kiện hợp lệ với số lượng nguyên > 0; sau khi lưu, số tồn tăng đúng bằng số lượng nhập và có đúng 1 giao dịch nhập kho được ghi. Số lượng ≤ 0 hoặc linh kiện không tồn tại thì bị từ chối | US-02 | MUST |
| **FR-03** | Xuất linh kiện cho phiếu bảo hành | Kỹ thuật viên xuất kho với số lượng nguyên > 0, bắt buộc gắn với phiếu bảo hành cùng trung tâm đang ở trạng thái **Đang xử lý** hoặc **Chờ linh kiện**; sau khi lưu, số tồn giảm đúng bằng số lượng xuất và có đúng 1 giao dịch xuất kho gắn với phiếu đó. Phiếu ở trạng thái khác thì bị từ chối | US-03 | MUST |
| **FR-04** | Chặn xuất vượt tồn kho | Khi số lượng xuất > số tồn hiện tại, hệ thống từ chối, thông báo nêu số lượng xuất và số tồn hiện có; số tồn không đổi và không ghi giao dịch. Xuất đúng bằng số tồn thì được phép | US-04 | MUST |
| **FR-05** | Cảnh báo tồn kho thấp | Hiển thị cảnh báo cho mọi linh kiện có số tồn < ngưỡng tối thiểu, cập nhật sau mỗi giao dịch xuất kho, nhập kho và khi đổi ngưỡng; quản lý lọc được danh sách chỉ gồm linh kiện dưới ngưỡng; cảnh báo mất khi số tồn ≥ ngưỡng | US-05 | SHOULD |
| **FR-06** | Xem lịch sử giao dịch linh kiện | Quản lý trung tâm xem lịch sử giao dịch linh kiện, lọc theo linh kiện, phiếu bảo hành hoặc khoảng ngày; mỗi dòng gồm ngày giờ, loại (nhập kho / xuất kho), số lượng, người thực hiện, phiếu bảo hành (nếu là xuất kho) | US-06, US-07 | SHOULD |
| **FR-07** | Thiết lập ngưỡng tối thiểu | Quản lý trung tâm đặt ngưỡng tối thiểu là số nguyên ≥ 0 cho từng linh kiện tại trung tâm mình; giá trị < 0 bị từ chối; sau khi lưu, cảnh báo cập nhật theo ngưỡng mới | US-08 | COULD |


---

### 3.2. User Story (8 story, chuẩn INVEST & MoSCoW)

#### US-01 · Xem tồn kho linh kiện — **MUST**
> Là **kỹ thuật viên**, tôi muốn **xem số tồn của từng linh kiện tại trung tâm mình** để **biết còn hàng hay không trước khi nhận sửa, tránh phải hẹn lại khách**.

**Tiêu chí chấp nhận (Given – When – Then):**

| # | Given | When | Then |
|---|---|---|---|
| AC-1.1 | Kỹ thuật viên đã đăng nhập, thuộc trung tâm "BH Quận 10" | Mở trang "Tồn kho linh kiện" | Hệ thống hiển thị danh sách linh kiện chỉ của trung tâm "BH Quận 10", mỗi dòng gồm mã, tên, số tồn và ngưỡng tối thiểu |
| AC-1.2 | Trang tồn kho đang mở, danh sách có 180 linh kiện | Nhập từ khóa "LCD" vào ô tìm kiếm | Danh sách chỉ hiển thị các linh kiện có mã hoặc tên chứa "LCD" |
| AC-1.3 | Kỹ thuật viên thuộc trung tâm "BH Cần Thơ 1" | Cố truy cập tồn kho của trung tâm "BH Quận 10" | Hệ thống từ chối và hiển thị thông báo "Bạn không có quyền xem dữ liệu trung tâm khác" |

---

#### US-02 · Ghi nhận nhập kho — **MUST**
> Là **quản lý trung tâm**, tôi muốn **ghi nhận nhập kho linh kiện** để **số tồn được cập nhật ngay, không phải chờ chốt cuối ngày như sổ tay giấy**.

**Tiêu chí chấp nhận (Given – When – Then):**

| # | Given | When | Then |
|---|---|---|---|
| AC-2.1 | Linh kiện "LCD-IP15" đang có tồn = 5 tại "BH Quận 10" | Quản lý nhập kho 10 cái "LCD-IP15" và bấm Lưu | Tồn kho "LCD-IP15" tăng lên 15; một giao dịch nhập kho được ghi nhận với số lượng = 10 |
| AC-2.2 | Quản lý đang ở form nhập kho | Nhập số lượng = 0 hoặc số âm và bấm Lưu | Hệ thống hiển thị lỗi "Số lượng nhập phải lớn hơn 0" và không lưu giao dịch |
| AC-2.3 | Quản lý nhập mã linh kiện không tồn tại "XYZ-999" | Bấm Lưu | Hệ thống hiển thị lỗi "Mã linh kiện không tồn tại trong danh mục" và không lưu giao dịch |

---

#### US-03 · Xuất linh kiện cho phiếu bảo hành — **MUST**
> Là **kỹ thuật viên**, tôi muốn **xuất linh kiện gắn với một phiếu bảo hành cụ thể** để **biết mỗi phiếu đã dùng linh kiện nào và truy vết được khi cần kiểm tra**.

**Tiêu chí chấp nhận (Given – When – Then):**

| # | Given | When | Then |
|---|---|---|---|
| AC-3.1 | Linh kiện "PIN-SS-S24" tồn = 8; phiếu "BH-000456/2026" đang ở trạng thái **Đang xử lý** | Kỹ thuật viên xuất 1 cái "PIN-SS-S24" cho phiếu "BH-000456/2026" | Tồn giảm xuống 7; một giao dịch xuất kho được ghi nhận kèm mã phiếu "BH-000456/2026" |
| AC-3.2 | Phiếu "BH-000789/2026" đang ở trạng thái **Đã đóng** | Kỹ thuật viên cố xuất linh kiện cho phiếu này | Hệ thống từ chối: "Không thể xuất linh kiện cho phiếu đã đóng"; tồn không đổi |
| AC-3.3 | Phiếu "BH-000111/2026" thuộc trung tâm "BH Cần Thơ 1", kỹ thuật viên thuộc "BH Quận 10" | Kỹ thuật viên cố xuất linh kiện cho phiếu này | Hệ thống từ chối: "Phiếu không thuộc trung tâm của bạn" |

---

#### US-04 · Chặn xuất vượt tồn kho — **MUST**
> Là **kỹ thuật viên**, tôi muốn **hệ thống từ chối khi tôi xuất nhiều hơn số tồn hiện có** để **số tồn không bị âm và luôn khớp với thực tế trong kho**.

**Tiêu chí chấp nhận (Given – When – Then):**

| # | Given | When | Then |
|---|---|---|---|
| AC-4.1 | Linh kiện "MH-OP-R5" tồn = 2 tại "BH Cần Thơ 1" | Kỹ thuật viên xuất 3 cái "MH-OP-R5" | Hệ thống từ chối, hiển thị "Số lượng xuất (3) vượt quá tồn kho hiện tại (2). Giao dịch bị hủy."; tồn giữ nguyên = 2 |
| AC-4.2 | Linh kiện "MH-OP-R5" tồn = 2 | Kỹ thuật viên xuất đúng 2 cái | Giao dịch thành công; tồn = 0 |
| AC-4.3 | Linh kiện "MH-OP-R5" tồn = 0 | Kỹ thuật viên cố xuất 1 cái | Hệ thống từ chối: "Linh kiện này đã hết hàng tại trung tâm của bạn" |

---

#### US-05 · Cảnh báo tồn kho thấp — **SHOULD**
> Là **quản lý trung tâm**, tôi muốn **được cảnh báo khi tồn kho của một linh kiện xuống dưới ngưỡng tối thiểu** để **đặt hàng bổ sung kịp trước khi hết, giảm số lần kỹ thuật viên phải hẹn lại khách**.

**Tiêu chí chấp nhận (Given – When – Then):**

| # | Given | When | Then |
|---|---|---|---|
| AC-5.1 | Linh kiện "SAC-IP15" có ngưỡng tối thiểu = 5, tồn hiện tại = 6 | Một giao dịch xuất kho 2 cái làm tồn giảm xuống 4 | Hệ thống hiển thị cảnh báo ngay trên trang tồn kho: "SAC-IP15 – Tồn (4) dưới ngưỡng tối thiểu (5)" |
| AC-5.2 | Danh sách tồn kho đang mở | Quản lý bật bộ lọc "Chỉ hiện linh kiện dưới ngưỡng" | Chỉ hiển thị các linh kiện có số tồn nhỏ hơn ngưỡng tối thiểu của linh kiện đó |
| AC-5.3 | Linh kiện "SAC-IP15" đang hiển thị cảnh báo, tồn = 4, ngưỡng = 5 | Quản lý nhập kho thêm 2 cái, tồn tăng lên 6 | Cảnh báo biến mất khỏi danh sách |

---

#### US-06 · Xem lịch sử giao dịch linh kiện theo linh kiện — **SHOULD**
> Là **quản lý trung tâm**, tôi muốn **xem lịch sử giao dịch linh kiện theo từng linh kiện, có thể lọc theo khoảng ngày** để **đối chiếu với tồn thực tế và phát hiện sai lệch sớm**.

**Tiêu chí chấp nhận (Given – When – Then):**

| # | Given | When | Then |
|---|---|---|---|
| AC-6.1 | Linh kiện "LCD-IP15" có 50 giao dịch trong tháng 9/2026 | Quản lý chọn linh kiện "LCD-IP15" và lọc khoảng thời gian 01/09 – 30/09/2026 | Hiển thị đúng 50 giao dịch; mỗi dòng gồm ngày giờ, loại (nhập kho / xuất kho), số lượng, người thực hiện, phiếu bảo hành liên quan (nếu là xuất kho) |
| AC-6.2 | Quản lý đang xem lịch sử linh kiện "LCD-IP15" | Quản lý thay đổi bộ lọc sang tháng 10/2026 (chưa có giao dịch nào) | Hệ thống hiển thị danh sách rỗng và thông báo "Không có giao dịch nào trong khoảng thời gian này" |

---

#### US-07 · Xem lịch sử giao dịch linh kiện theo phiếu bảo hành — **SHOULD**
> Là **quản lý trung tâm**, tôi muốn **xem tất cả linh kiện đã xuất cho một phiếu bảo hành cụ thể** để **kiểm tra việc dùng linh kiện của từng phiếu và phát hiện bất thường**.

**Tiêu chí chấp nhận (Given – When – Then):**

| # | Given | When | Then |
|---|---|---|---|
| AC-7.1 | Phiếu "BH-000456/2026" đã xuất 2 linh kiện: 1× "LCD-IP15" và 1× "PIN-IP15" | Quản lý mở chi tiết phiếu "BH-000456/2026", chọn tab "Linh kiện đã dùng" | Hiển thị bảng gồm: mã linh kiện, tên, số lượng, người xuất, ngày giờ xuất; tổng số linh kiện đã dùng hiển thị ở cuối bảng |
| AC-7.2 | Phiếu "BH-000999/2026" chưa có linh kiện nào được xuất | Quản lý mở tab "Linh kiện đã dùng" của phiếu này | Hệ thống hiển thị bảng rỗng và thông báo "Phiếu này chưa có linh kiện nào được xuất" |

---

#### US-08 · Thiết lập ngưỡng tối thiểu cho linh kiện — **COULD**
> Là **quản lý trung tâm**, tôi muốn **thiết lập và điều chỉnh ngưỡng tối thiểu cho từng linh kiện tại trung tâm mình** để **cảnh báo tồn kho phù hợp với nhu cầu sử dụng thực tế, tránh thiếu hàng đột ngột**.

**Tiêu chí chấp nhận (Given – When – Then):**

| # | Given | When | Then |
|---|---|---|---|
| AC-8.1 | Linh kiện "LCD-IP15" tại "BH Quận 10" có ngưỡng tối thiểu = 3, tồn hiện tại = 4 | Quản lý sửa ngưỡng tối thiểu thành 5 và bấm Lưu | Ngưỡng mới được lưu; vì tồn (4) < ngưỡng mới (5), cảnh báo xuất hiện ngay trên trang tồn kho |
| AC-8.2 | Quản lý đang chỉnh sửa ngưỡng tối thiểu của một linh kiện | Nhập giá trị ngưỡng = -1 và bấm Lưu | Hệ thống từ chối và hiển thị lỗi "Ngưỡng tối thiểu phải là số nguyên ≥ 0" |

---

### 3.3. Tổng hợp User Story theo MoSCoW

| Mức ưu tiên | User Story | Số lượng | Ghi chú lộ trình hiện thực |
|---|---|:---:|---|
| **MUST** | US-01, US-02, US-03, US-04 | 4 | Bắt buộc hoàn thành và chạy được trong Bài tập 2 (BT2) |
| **SHOULD** | US-05, US-06, US-07 | 3 | Hiện thực trong BT2 nếu kịp tiến độ |
| **COULD** | US-08 | 1 | Tính năng mở rộng cho người dùng cấu hình |
| **WON'T** | Tự đặt hàng NCC, chuyển kho liên chi nhánh | 0 | Không làm trong học phần CĐTN1 |
| **Tổng cộng** | | **8** | Đạt chuẩn ≥ 8 User Story theo yêu cầu Buổi 4; tất cả có mã US, mức MoSCoW và AC (Given–When–Then) |

---




## 4. Yêu cầu phi chức năng (NFR)

| Mã NFR | Phân loại | Yêu cầu kỹ thuật | Ngưỡng đo lường bắt buộc (Metric) |
|---|---|---|---|
| **NFR-01** | **Hiệu năng** *(Performance)* | Trang tồn kho linh kiện, lịch sử giao dịch linh kiện, nhập kho và xuất kho phản hồi trong thời gian cho phép khi nhiều người dùng cùng sử dụng | Thời gian phản hồi **≤ 500 ms** cho 95% request, với 10.000 giao dịch linh kiện và 20 người dùng đồng thời, trên máy 2 CPU, 4 GB RAM |
| **NFR-02** | **Truy cập đồng thời** *(Concurrency)* | Số tồn không bị âm khi nhiều kỹ thuật viên cùng xuất kho một linh kiện | Với 50 yêu cầu xuất kho (mỗi yêu cầu 1 cái) gửi cùng lúc cho linh kiện có tồn = 10: đúng 10 yêu cầu thành công, 40 bị từ chối, tồn cuối = 0, số lần tồn âm = 0 |
| **NFR-03** | **Bảo mật và phân quyền** *(Security)* | Người dùng chỉ truy cập dữ liệu của trung tâm mình và chỉ làm đúng quyền của vai trò | 100% API yêu cầu đăng nhập; truy cập dữ liệu trung tâm khác hoặc thao tác ngoài quyền bị từ chối (HTTP 403) trong 100% của ≥ 10 ca kiểm thử |
| **NFR-04** | **Truy vết và toàn vẹn dữ liệu** *(Traceability & Data Integrity)* | Mọi giao dịch linh kiện truy vết được và không bị xóa; tồn kho luôn khớp với lịch sử giao dịch linh kiện | 100% giao dịch linh kiện có người thực hiện và thời điểm; số lần xóa vật lý = 0; sai lệch giữa tồn kho linh kiện và (tổng nhập kho − tổng xuất kho) = 0 trên 100% linh kiện sau bộ kiểm thử |

---

## 5. Ràng buộc và quy tắc nghiệp vụ

### 5.1. Quy tắc nghiệp vụ
*(QT-xx trích từ Bảng 9.1 của case study; QT-L5-xx do phân tích luồng L5 suy ra)*

| Mã quy tắc | Tên quy tắc | Nội dung chi tiết | Nguồn |
|---|---|---|---|
| **QT-06** | Vòng đời phiếu bảo hành | Phiếu bảo hành chỉ chuyển trạng thái theo đúng vòng đời, không quay lại trạng thái trước. Luồng L5 chỉ đọc trạng thái phiếu bảo hành, không đổi trạng thái. | Case study, Bảng 9.1 |
| **QT-09** | Chặn xuất quá tồn và cảnh báo | Không được xuất kho vượt số tồn hiện tại của trung tâm bảo hành. Khi số tồn xuống dưới ngưỡng tối thiểu, hệ thống phải hiển thị cảnh báo tồn thấp. | Case study, Bảng 9.1 |
| **QT-13** | Không xóa vật lý | Không xóa vật lý dữ liệu, chỉ đánh dấu ngừng sử dụng. Áp dụng cho luồng L5: giao dịch linh kiện đã ghi không được sửa hoặc xóa. | Case study, Bảng 9.1 |
| **QT-14** | Cô lập dữ liệu trung tâm | Nhân viên chỉ xem và thao tác trên dữ liệu của trung tâm bảo hành mình làm việc; quản lý xem toàn bộ đơn vị mình phụ trách. | Case study, Bảng 9.1 |
| **QT-L5-01** | Ràng buộc nhập kho | Mỗi lần nhập kho phải có linh kiện hợp lệ, số lượng nguyên > 0, người thực hiện và thời điểm. | Phân tích từ sổ tay tồn kho |
| **QT-L5-02** | Ràng buộc xuất kho | Mỗi lần xuất kho phải có số lượng nguyên > 0 và gắn với đúng một phiếu bảo hành cùng trung tâm, đang ở trạng thái **Đang xử lý** hoặc **Chờ linh kiện**. | Phân tích quy trình thực tế |
| **QT-L5-03** | Số tồn không âm | Số tồn của mọi linh kiện luôn ≥ 0, kể cả khi nhiều kỹ thuật viên cùng xuất một linh kiện. | Suy ra từ QT-09 và NFR-02 |

### 5.2. Ràng buộc (Constraints)

- **RB-01**: Triển khai độc lập từng luồng nghiệp vụ, không phụ thuộc đồng bộ vào luồng khác (chỉ đạo của Phó Tổng giám đốc: làm từng phần, phần nào xong dùng phần đó).
- **RB-02**: Yêu cầu công nghệ: Java 17, Spring Boot, MySQL 8; kiểu dữ liệu của từ điển tham chiếu (PostgreSQL) được quy đổi sang MySQL 8.
- **RB-03**: Phiếu bảo hành do luồng L2 quản lý; luồng L5 chỉ đọc dữ liệu phiếu bảo hành (với tập dữ liệu mẫu khoảng 400 phiếu).

---

## 6. Bảng truy vết yêu cầu (Traceability Matrix)

Bảng dưới đây ánh xạ **Mã FR ↔ Tên yêu cầu ↔ User Story ↔ Use Case ↔ MoSCoW** (Bảo đảm không có bất kỳ ô trống nào):

| Mã FR | Tên yêu cầu | User Story | Use Case | MoSCoW |
|:---:|---|:---:|---|:---:|
| **FR-01** | Xem tồn kho linh kiện | US-01 | UC-01 – Xem tồn kho linh kiện | **MUST** |
| **FR-02** | Ghi nhận nhập kho | US-02 | UC-02 – Ghi nhận nhập kho | **MUST** |
| **FR-03** | Xuất linh kiện cho phiếu bảo hành | US-03 | UC-03 – Xuất linh kiện cho phiếu bảo hành | **MUST** |
| **FR-04** | Chặn xuất vượt tồn kho | US-04 | UC-04 – Kiểm tra số tồn trước khi xuất | **MUST** |
| **FR-05** | Cảnh báo tồn kho thấp | US-05 | UC-05 – Xem cảnh báo tồn thấp | **SHOULD** |
| **FR-06** | Xem lịch sử giao dịch linh kiện | US-06, US-07 | UC-06 – Xem lịch sử giao dịch linh kiện | **SHOULD** |
| **FR-07** | Thiết lập ngưỡng tối thiểu | US-08 | UC-07 – Thiết lập ngưỡng tối thiểu | **COULD** |

---

*Tài liệu đặc tả yêu cầu phần mềm rút gọn (SRS) – Hoàn thành cho Luồng L5 (Track SE).*
