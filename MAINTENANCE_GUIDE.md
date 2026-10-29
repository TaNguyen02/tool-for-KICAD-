# Kiểm tra chân và khôi phục thư viện / Library maintenance — 4.1.0

## Tiếng Việt

### Vì sao nhiều linh kiện hiện “Chưa xác định”?

Thông báo này nói về **loại điện của chân dùng cho ERC**: Input, Output, Passive, Power input… Nó không có nghĩa là tool làm mất chân vật lý. Một số nguồn CAD cung cấp đủ tên/số chân nhưng bỏ trống toàn bộ thuộc tính điện.

Tool thử đối chiếu mã linh kiện, toàn bộ số/tên chân với symbol KiCad chuẩn hoặc profile đã kiểm tra datasheet. Bản này bổ sung nhiều trường hợp ESP32-S/SL/S2/S3/S3R8, AMS1117-3.3 và ULN2003ADR. Chỉ loại điện còn thiếu được điền; các giá trị đã khai báo, hình vẽ và footprint được giữ nguyên.

Mẫu kiểm tra gồm 16 linh kiện lấy CAD mới từ nguồn: 11 không còn loại điện thiếu, 5 vẫn cần đối chiếu. Bảy trường hợp mới được bổ sung 269 loại chân. Đây là mẫu đại diện, không phải lời cam kết rằng mọi linh kiện đều được tự phân loại. Loại điện đã được nhà cung cấp khai báo cũng có thể cần kiểm tra về ý nghĩa.

### Kiểm tra toàn bộ linh kiện đã tải

1. Mở **Kiểm tra thư viện…** ở cửa sổ chính, hoặc trong Cài đặt.
2. Tool đọc các linh kiện thuộc danh mục Part Finder trên máy; không gọi nguồn Internet để tải lại CAD.
3. Chọn một dòng để xem số chân thiếu, căn cứ đối chiếu và lý do nếu chưa sửa được.
4. Bấm **Bổ sung chân đã đối chiếu** khi có các dòng “Có thể bổ sung”. Tool sao lưu và chỉ ghi những linh kiện được quản lý, chưa chỉnh sửa và đã giải quyết đủ loại chân.
5. Đọc lại kết quả sau khi hoàn tất. Mỗi lần hiện hỗ trợ tối đa 10.000 bản ghi được quản lý.

| Kết quả | Ý nghĩa / việc tiếp theo |
| --- | --- |
| Đủ loại điện | Không còn loại điện trống; không tự thay đổi. Vẫn cần kiểm tra tính đúng với datasheet. |
| Có thể bổ sung | Có căn cứ khớp toàn bộ và bản hiện tại chưa chỉnh sửa; có thể bấm nút bổ sung. |
| Đã bổ sung | Đã lưu thay đổi kèm bản sao lưu. |
| Cần đối chiếu | Chưa đủ nguồn khớp; giữ nguyên để bạn kiểm tra. |
| Giữ nguyên | Tệp thiếu, đã chỉnh sửa, chưa xác minh được sở hữu hoặc có dữ liệu xung đột; không ghi đè. |

Phạm vi này là **linh kiện do Part Finder quản lý**, không phải mọi thư viện toàn cục của KiCad. Bản đã chỉnh tay được bảo vệ. Không nên xóa dấu xác minh hoặc gán hàng loạt mọi chân thành Input/Passive chỉ để vượt qua kiểm tra.

### Cập nhật symbol đang nằm trong schematic

KiCad lưu một bản symbol bên trong schematic. Sửa thư viện không tự đổi bản đó.

Chọn các symbol cần cập nhật rồi dùng **Tools → Update Symbols from Library… → Update selected symbol(s)**. Kiểm tra lại các chân, kết nối và ERC sau khi cập nhật. Tool không tự sửa schematic hoặc đi dây của bạn.

### Khôi phục khi đổi máy hoặc chuyển thư mục

1. Giải nén **toàn bộ ZIP**; giữ `_internal` cạnh `KiCadPartFinder.exe`.
2. Mở **Cài đặt → Khôi phục liên kết thư viện…**.
3. Chọn thư mục hiện có chứa `PartFinder.kicad_sym` và `PartFinder.pretty` của bạn.
4. Bấm **Kiểm tra thư mục** và đọc thông báo. Khi có đủ căn cứ, nút **Khôi phục liên kết** sẽ bật.
5. Bấm **Khôi phục liên kết** để cập nhật đăng ký và nơi lưu trên máy hiện tại. Thay đổi được sao lưu; dữ liệu gốc được giữ nguyên.
6. Đóng rồi mở lại hộp chọn symbol/footprint trong KiCad để đọc thư viện đã liên kết.

Không cần đưa dữ liệu về đường dẫn hoặc máy cũ. Tool kiểm tra nội dung/hash quản lý trước khi đổi liên kết cũ bị thiếu hoặc trỏ tới bản dữ liệu giống nhau. Nếu nickname PartFinder đang trỏ tới **một thư viện khác còn tồn tại và khác nội dung**, tool dừng để bảo vệ nó; không tự thay thế thư viện đó.

Với một bản cài mới chưa có danh mục quản lý, các đường dẫn cũ không còn hợp lệ có thể được thay bằng mặc định của người dùng hiện tại. Khi đã có dữ liệu quản lý, thư mục thay thế phải khớp nội dung được ghi nhận. Tệp thiếu hoặc đã sửa khiến việc xác minh thất bại thì cần kiểm tra bản sao lưu/dữ liệu đang có trước khi tiếp tục.

### Hộp kiểm tra loại chân và tài nguyên tùy chọn

- Nút hành động của hộp **Kiểm tra loại chân** được giữ ở vùng nhìn thấy; danh sách có thể cuộn. Những số chân lặp giống nhau của cùng chân vật lý được xử lý nhất quán; biểu diễn xung đột vẫn cần sửa nguồn.
- Datasheet hỗ trợ kiểm tra vai trò điện. Với trường hợp chưa tự xác định được, chọn loại cho các chân còn thiếu, đối chiếu tài liệu rồi xác nhận. Các loại đã có được giữ nguyên.
- Symbol/footprint đã lưu thành công nhưng PDF, 3D hoặc làm mới KiCad gặp lỗi sẽ được báo riêng. Không cần coi lần tải CAD đó là thất bại; tài nguyên tùy chọn có thể được thử lại.
- Nếu cấu hình Local AI bị hỏng JSON, thao tác **Save settings** sẽ giữ một bản sao tệp hỏng trước khi thay thế. Đóng hộp thoại không tự ghi cấu hình mới. Kiểm tra đường dẫn bản sao lưu trong thông báo.

Chức năng autorouting giữ các giới hạn của 4.0.0; không có tự xếp PCB. Xem [ROUTING_GUIDE.md](ROUTING_GUIDE.md) nếu muốn dùng Freerouting trên PCB đã bố trí.

## English

Missing electrical types are source CAD/ERC attributes, not missing physical pins. Version 4.1.0 broadens exact-reference and reviewed-profile coverage while preserving drawing geometry, labels, footprints and types already declared. A representative fresh-CAD sample covered 16 parts: 11 had no missing types after processing, five still required review, and seven newly supported cases gained 269 additional pin types. This is not universal coverage or proof of electrical correctness.

Use **Kiểm tra thư viện…** to scan every component managed in the local Part Finder catalog without redownloading CAD. Review per-part results, then explicitly choose **Bổ sung chân đã đối chiếu**. Only unchanged, ownership-verified components whose missing roles are fully resolved are written through the backed-up transaction. Missing, edited, ambiguous or unresolved components remain protected. The current per-scan catalog limit is 10,000 records.

Library repair does not update symbol copies embedded in schematics. Select the symbols in KiCad and use **Tools → Update Symbols from Library…**, then check wiring and ERC.

For moved/copied libraries, open **Cài đặt → Khôi phục liên kết thư viện…**, select the current managed-data folder, check it and explicitly restore the links when verification succeeds. The original computer/path is not required. Missing or identical stale registrations can be updated; a different existing foreign library is not overwritten. Fresh installations can discard invalid machine-specific preferences, while existing managed data must prove matching hashes before another location is adopted.

The pin-review footer remains accessible on smaller screens. Optional PDF/3D/refresh failures are separated from a completed CAD save. Explicitly saving Local AI preferences backs up damaged settings before replacement. Existing routing limits and import protections remain; no claim is made that the whole application is free of every possible defect.
