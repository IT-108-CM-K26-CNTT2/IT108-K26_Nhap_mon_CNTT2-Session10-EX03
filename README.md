# Session10_Ex03 – VLOOKUP tra cứu liên bảng

**Mục tiêu:** Tự động điền Tên linh kiện và Đơn giá từ sheet `Danh_Muc` vào sheet `Don_Hang` (100 mã), thay cho làm thủ công.

## 1. Phân tích 4 tham số

`=VLOOKUP(lookup_value, table_array, col_index_num, [range_lookup])`

| Tham số | Trong bài toán | Giá trị |
|---|---|---|
| `lookup_value` | Mã linh kiện cần dò (sheet Don_Hang) | `$A2` |
| `table_array` | Bảng tra ở sheet Danh_Muc, cột đầu phải là mã | `Danh_Muc!$A$2:$C$101` |
| `col_index_num` | Cột cần lấy về: Tên = 2, Đơn giá = 3 | `2` / `3` |
| `range_lookup` | Tìm chính xác (mã là văn bản, không dùng gần đúng) | `FALSE` |

## 2. Công thức hoàn chỉnh (kèm IFERROR)

| Ô | Công thức |
|---|---|
| **B2** (Tên LK) | `=IFERROR(VLOOKUP(TRIM($A2), Danh_Muc!$A$2:$C$101, 2, FALSE), "Không tìm thấy mã")` |
| **C2** (Đơn giá) | `=IFERROR(VLOOKUP(TRIM($A2), Danh_Muc!$A$2:$C$101, 3, FALSE), 0)` |

Nhập ở dòng 2, chọn B2:C2 rồi kéo xuống dòng 101.

## 3. Xử lý 2 bẫy dữ liệu

### Bẫy 1: Quên dấu `$` → #N/A hàng loạt
- **Nguyên nhân:** `A2:C101` (không có `$`) bị dịch xuống khi kéo công thức (dòng 3 thành `A3:C102`, dòng 50 thành `A50:C149`...), bảng tra mất dần dữ liệu.
- **Cách sửa:** cố định bằng `$A$2:$C$101` (nhấn **F4**).
- `$A2` ở lookup_value: cố định cột A, dòng vẫn thay đổi khi kéo xuống.

### Bẫy 2: Mã trong bảng tra có khoảng trắng thừa ở cuối
- **Nguyên nhân:** `"LK05 "` khác `"LK05"` nên VLOOKUP chính xác trả về #N/A.
- **Phát hiện:** `=LEN(A2)<>LEN(TRIM(A2))` (TRUE là có khoảng trắng thừa).
- **Cách sửa (khuyến nghị):** thêm cột phụ `=TRIM(A2)` trong Danh_Muc, copy → Paste Special → Values đè lại cột A, xóa cột phụ.
- **Lưu ý:** `TRIM($A2)` trong công thức chỉ làm sạch mã bên Don_Hang, **không** sửa được khoảng trắng bên Danh_Muc.

## 4. Checklist

- [ ] `table_array` đủ `$`: `$A$2:$C$101`
- [ ] Tham số cuối là `FALSE`
- [ ] Kéo đủ 100 dòng, không còn #N/A
- [ ] Đã làm sạch khoảng trắng thừa
- [ ] Mã không tồn tại hiện thông báo thân thiện nhờ IFERROR

## 5. Minh chứng

![Trước khi làm](screenshots/01_truoc_khi_lam.png)
![Công thức](screenshots/02_cong_thuc_vlookup.png)
![Kết quả](screenshots/03_ket_qua_sau_khi_keo.png)
