# Hệ thống Quản lý Khách sạn (Hotel Management System)

## 1. Tổng quan đề tài

**Tên đề tài:** Hệ thống Quản lý Khách sạn (Hotel Management System)

Đề tài được xây dựng dựa trên bốn tiêu chí:

1. Có ít nhất ba loại tác nhân với nhu cầu khác nhau.
2. Có ít nhất một quy trình nhiều bước và có trạng thái.
3. Có ít nhất một ràng buộc nghiệp vụ thực tế.
4. Phạm vi đủ nhỏ để có thể cài đặt phần lõi.

---

## 2. Các tác nhân

| Tác nhân | Chức năng chính |
|---|---|
| **Khách hàng (Guest)** | Tìm phòng, xem phòng trống, đặt phòng, hủy đặt phòng, xem thông tin đặt phòng |
| **Lễ tân (Receptionist)** | Xác nhận đặt phòng, tạo đặt phòng trực tiếp, check-in, check-out, ghi nhận dịch vụ khách sử dụng |
| **Nhân viên buồng phòng (Housekeeping Staff)** | Xem phòng cần dọn, cập nhật trạng thái phòng sau khi dọn |
| **Quản lý khách sạn (Hotel Manager)** | Quản lý phòng, loại phòng, giá phòng, nhân viên, theo dõi doanh thu và tình trạng hoạt động |

Có thể chọn tối thiểu ba tác nhân:

- Khách hàng
- Lễ tân
- Quản lý khách sạn

Tuy nhiên, để thể hiện rõ nghiệp vụ thực tế hơn, nên bổ sung **Nhân viên buồng phòng**.

---

## 3. Quy trình nghiệp vụ chính

### 3.1. Tìm phòng

Khách hàng nhập:

- Ngày nhận phòng
- Ngày trả phòng
- Số lượng người
- Loại phòng mong muốn

Hệ thống kiểm tra các phòng có thể sử dụng trong khoảng thời gian đó.

**Trạng thái phòng:** `Available`

---

### 3.2. Tạo yêu cầu đặt phòng

Khách hàng lựa chọn phòng và cung cấp:

- Họ tên
- Số điện thoại
- Email
- Ngày check-in
- Ngày check-out
- Số lượng khách

Hệ thống tạo booking.

**Trạng thái booking:** `Pending`

---

### 3.3. Xác nhận đặt phòng

Lễ tân kiểm tra:

- Phòng còn khả dụng hay không
- Ngày nhận và trả phòng
- Thông tin khách hàng
- Tiền đặt cọc nếu khách sạn yêu cầu

Nếu hợp lệ:

`Pending → Confirmed`

Nếu không hợp lệ:

`Pending → Rejected`

---

### 3.4. Check-in

Khi khách đến khách sạn, lễ tân kiểm tra booking.

Điều kiện:

- Booking phải ở trạng thái `Confirmed`
- Thời gian check-in hợp lệ
- Phòng phải ở trạng thái sẵn sàng

Sau khi check-in:

`Confirmed → Checked-in`

Trạng thái phòng:

`Available → Occupied`

---

### 3.5. Sử dụng dịch vụ

Trong thời gian lưu trú, khách có thể sử dụng:

- Ăn uống
- Giặt là
- Minibar
- Đưa đón
- Thuê xe
- Các dịch vụ khác của khách sạn

Mỗi lần sử dụng dịch vụ, hệ thống ghi nhận chi phí vào hóa đơn của khách.

Ví dụ:

| Dịch vụ | Số lượng | Giá |
|---|---:|---:|
| Nước uống | 2 | 40.000đ |
| Giặt là | 1 | 100.000đ |
| Ăn sáng | 2 | 200.000đ |

---

## 4. Quy trình check-out

Quy trình:

1. Lễ tân yêu cầu check-out
2. Hệ thống tính tiền phòng
3. Hệ thống cộng tiền dịch vụ
4. Trừ tiền đặt cọc nếu có
5. Khách thanh toán
6. Hệ thống hoàn tất hóa đơn
7. Khách check-out

Trạng thái booking:

`Checked-in → Checked-out → Completed`

Trạng thái phòng:

`Occupied → Needs Cleaning`

---

## 5. Quy trình dọn phòng

Sau khi khách check-out:

`Occupied`

↓

`Needs Cleaning`

↓

Nhân viên nhận phòng cần dọn

↓

`Cleaning`

↓

Nhân viên hoàn thành

↓

`Available`

Phòng không được chuyển ngay sang `Available` sau khi khách check-out mà phải hoàn thành công việc dọn phòng trước.

---

## 6. Các trạng thái của phòng

| Trạng thái | Ý nghĩa |
|---|---|
| `Available` | Phòng có thể sử dụng |
| `Reserved` | Phòng đã được đặt |
| `Occupied` | Đang có khách |
| `Needs Cleaning` | Khách vừa trả phòng, cần dọn |
| `Cleaning` | Nhân viên đang dọn |
| `Maintenance` | Phòng đang bảo trì |

Vòng đời cơ bản:

`Available → Reserved → Occupied → Needs Cleaning → Cleaning → Available`

Nếu phòng cần bảo trì:

`Available → Maintenance → Available`

---

## 7. Các ràng buộc nghiệp vụ

### BR01 — Không được đặt phòng trùng lịch

Một phòng không được có hai booking có thời gian lưu trú giao nhau.

Ví dụ:

- Booking A: `01/10 → 05/10`
- Booking B: `03/10 → 06/10`

Booking B không được tạo cho cùng một phòng.

---

### BR02 — Ngày check-out phải sau ngày check-in

Sai:

- Check-in: `10/10`
- Check-out: `08/10`

Đúng:

- Check-in: `10/10`
- Check-out: `12/10`

---

### BR03 — Không check-in booking chưa xác nhận

Chỉ booking ở trạng thái:

`Confirmed`

mới được chuyển thành:

`Checked-in`

---

### BR04 — Không check-in phòng chưa sẵn sàng

Phòng đang ở một trong các trạng thái sau không được check-in:

- `Cleaning`
- `Needs Cleaning`
- `Maintenance`

---

### BR05 — Không được check-out khi chưa tính hóa đơn

Trước khi hoàn tất check-out phải tính:

**Tiền phòng + tiền dịch vụ + phụ phí − tiền đặt cọc**

---

### BR06 — Phòng đang có khách không được cấp cho khách khác

Nếu:

`RoomStatus = Occupied`

thì lễ tân không được check-in một booking khác vào phòng đó.

---

### BR07 — Phòng bảo trì không được đặt

Nếu:

`RoomStatus = Maintenance`

thì phòng không được xuất hiện trong danh sách phòng có thể đặt.

---

### BR08 — Chỉ booking đang lưu trú mới được thêm dịch vụ

Không được thêm dịch vụ vào booking ở trạng thái:

- `Cancelled`
- `Completed`

---

### BR09 — Số khách không được vượt quá sức chứa phòng

Ví dụ:

| Loại phòng | Sức chứa |
|---|---:|
| Single | 1 |
| Double | 2 |
| Family | 4 |

Phòng Double không thể được đặt cho 5 người.

---

### BR10 — Booking đã hoàn tất không được sửa thông tin lưu trú

Booking ở trạng thái:

`Completed`

không được thay đổi:

- Ngày check-in
- Ngày check-out
- Phòng
- Khách hàng

Nếu cần thay đổi tài chính thì phải sử dụng nghiệp vụ điều chỉnh hóa đơn riêng.

---

## 8. Quy trình tổng thể

```text
Khách tìm phòng
       ↓
Chọn phòng
       ↓
Tạo booking
       ↓
    Pending
       ↓
Lễ tân xác nhận
       ↓
   Confirmed
       ↓
Khách đến khách sạn
       ↓
    Check-in
       ↓
   Checked-in
       ↓
Sử dụng phòng/dịch vụ
       ↓
    Check-out
       ↓
   Thanh toán
       ↓
   Completed
       ↓
Needs Cleaning
       ↓
    Cleaning
       ↓
   Available
```

---

## 9. Các nghiệp vụ chính

| ID | Nghiệp vụ | Tác nhân |
|---|---|---|
| UC01 | Tìm phòng trống | Khách hàng |
| UC02 | Đặt phòng | Khách hàng / Lễ tân |
| UC03 | Xác nhận đặt phòng | Lễ tân |
| UC04 | Hủy đặt phòng | Khách hàng / Lễ tân |
| UC05 | Check-in | Lễ tân |
| UC06 | Ghi nhận dịch vụ sử dụng | Lễ tân |
| UC07 | Check-out và thanh toán | Lễ tân |
| UC08 | Cập nhật trạng thái dọn phòng | Nhân viên buồng phòng |
| UC09 | Quản lý phòng và loại phòng | Quản lý |
| UC10 | Xem báo cáo hoạt động | Quản lý |

---

## 10. Phạm vi phần lõi

Phần lõi của hệ thống là quản lý phòng và toàn bộ vòng đời của một lượt lưu trú:

**Tìm phòng → Đặt phòng → Xác nhận → Check-in → Sử dụng dịch vụ → Check-out → Dọn phòng → Phòng khả dụng trở lại**

Không nhất thiết phải phát triển:

- Tích hợp Booking.com
- Tích hợp Agoda
- Thanh toán ngân hàng thật
- AI gợi ý giá phòng
- Quản lý nhiều chi nhánh
- Hệ thống loyalty phức tạp
- Quản lý kế toán đầy đủ
- Quản lý nhà hàng đầy đủ

---

## 11. Tóm tắt đăng ký đề tài

**Tên đề tài:** Hotel Management System – Hệ thống quản lý khách sạn.

**Tác nhân:**

- Khách hàng
- Lễ tân
- Nhân viên buồng phòng
- Quản lý khách sạn

**Quy trình chính:**

`Tìm phòng → Đặt phòng → Xác nhận → Check-in → Sử dụng dịch vụ → Check-out/Thanh toán → Dọn phòng → Phòng khả dụng`

**Ràng buộc chính:**

- Không được đặt phòng trùng lịch
- Phòng đang có khách, đang bảo trì hoặc chưa dọn không được cấp cho khách khác
- Số khách không vượt sức chứa phòng
- Chỉ booking đã xác nhận mới được check-in
- Check-out phải hoàn tất hóa đơn

Đề tài đáp ứng bốn tiêu chí: có nhiều tác nhân, có quy trình nhiều bước với trạng thái rõ ràng, có ràng buộc nghiệp vụ thực tế và có phạm vi đủ nhỏ để triển khai phần lõi.
