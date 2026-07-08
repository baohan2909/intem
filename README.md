# NS TEM — Nón Sơn

> Hệ thống thiết kế & in tem mã vạch nội bộ  
> **© 2026 Công ty TNHH Thời trang Nón Sơn** — All rights reserved

---

## Giới thiệu

**NS TEM** là ứng dụng web (PWA) thay thế phần mềm BarTender, được xây dựng riêng cho vận hành in tem sản phẩm của chuỗi 220 cửa hàng Nón Sơn. Toàn bộ hệ thống hoạt động trên trình duyệt, không phát sinh chi phí bản quyền theo số lượng máy hoặc người dùng, chạy được cả khi mất mạng.

### Tính năng chính

- Thiết kế tem kéo-thả trực quan với đầy đủ 8 điểm resize
- Hỗ trợ 10 chuẩn mã vạch (Code 128, EAN-13, GS1-128, ITF-14, DataMatrix, PDF417…) và mã QR
- Kết nối trực tiếp file Excel — mỗi dòng dữ liệu = 1 tem
- Cột "Số lượng" tự nhân bản tem
- Tự co chữ khi tràn khung (tắt được)
- Thư viện mẫu nhiều template lưu trong máy + xuất/nhập .nstem.json
- Xuất PDF chất lượng cao 1200dpi grayscale antialiased
- In nhiệt tối ưu: render 4× supersampled + nhị phân hoá 300dpi
- Hoạt động offline sau lần mở đầu tiên
- Cài đặt được lên máy như phần mềm độc lập (PWA)

### Mẫu tem có sẵn

1. **TEM_BAO_HIEM** — Tem mũ bảo hiểm 41×20mm, 2 tem/hàng
2. **TEM_NON_VAI** — Tem nón vải 40×18.5mm, 2 tem/hàng

Bản backup của cả 2 mẫu nằm trong `templates/` — dùng khi cần khôi phục hoặc chuyển sang máy khác.

---

## Cấu trúc thư mục

```
NS_TEM/
├── index.html              # Ứng dụng chính (single-file)
├── manifest.json           # Khai báo PWA
├── sw.js                   # Service worker (offline cache)
├── favicon-32.png          # Icon trình duyệt
├── icons/                  # Icon PWA
│   ├── icon-192.png        # Icon 192×192 (Android, Windows)
│   ├── icon-512.png        # Icon 512×512 (splash screen)
│   └── icon-maskable.png   # Icon maskable (Android adaptive)
├── templates/              # Bản backup mẫu tem
│   ├── TEM_BAO_HIEM.nstem.json
│   └── TEM_NON_VAI.nstem.json
├── README.md               # Tài liệu này
└── LICENSE                 # Điều khoản sử dụng
```

---

## Triển khai lên GitHub Pages

1. Tạo repository trên GitHub (ví dụ: `intem`)
2. Upload **toàn bộ nội dung folder này** vào root của repository (giữ nguyên cấu trúc)
3. Vào **Settings → Pages**:
   - **Source**: Deploy from a branch
   - **Branch**: `main` — thư mục `/root`
   - Bấm **Save**
4. Chờ 1–2 phút, GitHub cấp URL dạng `https://<username>.github.io/<repo>/`
5. Mở URL — nếu thấy màn hình splash magenta Nón Sơn là thành công

### Cập nhật phiên bản

Khi có bản mới:
1. Ghi đè các file lên repository
2. Người dùng cần **Ctrl + Shift + R** để trình duyệt bỏ qua cache service worker
3. Hoặc chờ tự động (service worker sẽ tự cập nhật trong lần mở tiếp theo)

---

## Cài đặt lên máy như phần mềm

Sau khi truy cập app trên trình duyệt:

**Windows / macOS (Chrome, Edge):**  
Bấm nút **Cài đặt** (magenta) trên header → xác nhận. Icon NS TEM xuất hiện trên Desktop / Start Menu. Mở lên chạy fullscreen không có thanh URL.

**Android (Chrome):**  
Bấm **Cài đặt** → thêm vào Home screen như app native.

**iPhone / iPad (Safari):**  
Do giới hạn iOS, cần thao tác thủ công:  
Nút Chia sẻ (biểu tượng mũi tên đi lên) → **Thêm vào Màn hình chính** → Thêm.

---

## Sử dụng cơ bản

1. Mở app, chọn mẫu từ dropdown (**TEM_BAO_HIEM** hoặc **TEM_NON_VAI**)
2. Bấm **Nạp Excel**, chọn file dữ liệu — mỗi cột trở thành placeholder `{Tên cột}`
3. Duyệt xem trước bằng nút ‹ › hoặc bấm vào dòng trong bảng
4. Bấm **In tem**:
   - **In thử 1 hàng** để kiểm tra căn lề
   - **Xuất PDF** để lưu / gửi qua Zalo / in ngoài
   - **In toàn bộ** để in loạt

### Thiết lập máy in nhiệt

Trong driver máy in (Godex, Zebra, Citizen…):
- Tạo khổ giấy stock: **rộng = Khổ giấy ngang** trong app (mặc định 82–84mm), **cao = Cao tem** (20mm)
- **Media Type**: Labels With Gaps
- **Gap Height**: theo cuộn thực tế (~3mm)
- Tab **Graphics**: Dithering = **None**
- Tab **Options**: Darkness +2 nấc, Speed 3–4 ips

Trong hộp thoại in Chrome:
- **Máy in đích**: chọn đúng máy
- **Khổ giấy**: stock vừa tạo
- **Lề**: Không có
- **Tỷ lệ**: 100 (Mặc định, không dùng "Vừa với trang in")
- Bỏ tick **Đầu trang và chân trang**

---

## Hỗ trợ máy in

Tương thích mọi máy in tem có driver Windows, đã kiểm nghiệm:
- Citizen CL-S700 (300 dpi)
- Godex EZ530+ / G530
- Zebra ZD series

Máy in nhiệt trực tiếp và nhiệt in chuyển đều dùng được.

---

## Kỹ thuật

- **Zero-cost stack**: GitHub Pages + PWA thuần client-side, không backend
- **Không lệ thuộc CDN khi offline**: tất cả thư viện đã cache qua service worker
- **Thư viện chính**:
  - [Konva.js](https://konvajs.org/) — canvas kéo thả
  - [bwip-js](https://github.com/metafloor/bwip-js) — sinh mã vạch/QR
  - [SheetJS](https://sheetjs.com/) — đọc Excel
  - [jsPDF](https://github.com/parallax/jsPDF) — xuất PDF
- **Lưu trữ dữ liệu**: localStorage trình duyệt (thư viện mẫu tem)
- **Rendering pipeline**:
  - In nhiệt: render 1200dpi (SS ×4) → downsample bicubic → binarize 300dpi
  - PDF: render 1200dpi grayscale antialiased, PNG lossless, nhúng metadata Nón Sơn

---

## Bản quyền

Phần mềm này được bảo hộ theo **Luật Sở hữu trí tuệ Việt Nam** và các điều ước quốc tế mà Việt Nam là thành viên.

- **Chủ sở hữu**: Công ty TNHH Thời trang Nón Sơn
- **Tác giả**: Phòng CNTT Nón Sơn
- **Trạng thái**: Phần mềm nội bộ — không thương mại

Nghiêm cấm mọi hành vi sao chép, phân phối, thương mại hoá, hoặc sử dụng trái phép dưới bất kỳ hình thức nào khi chưa có sự đồng ý bằng văn bản của chủ sở hữu.

Xem chi tiết trong file [LICENSE](LICENSE).

---

**© 2026 Công ty TNHH Thời trang Nón Sơn** — All rights reserved
