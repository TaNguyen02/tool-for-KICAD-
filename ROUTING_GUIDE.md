# Đi dây tự động PCB / PCB autorouting — 4.0.0

## Tiếng Việt

### Bắt đầu

1. Trong KiCad, cập nhật PCB từ schematic bằng **F8**, tự sắp xếp linh kiện, vẽ đường viền kín trên **Edge.Cuts**, đặt quy tắc trong Board Setup rồi lưu.
2. Trong Part Finder, bấm **Đi dây tự động…**, chọn file `.kicad_pcb` đó. Chức năng tự xếp PCB đã được bỏ.
3. Lần đầu, bấm **Thiết lập Freerouting**. Bản Windows x64 này đã kèm gói CLI chính thức; tool kiểm tra SHA-256 rồi cài riêng trong thư mục dữ liệu, không cần Java hoặc Internet cho bước cài từ gói đi kèm.
4. Bấm **Kiểm tra PCB**. Các mục “Cần sửa” có hướng dẫn cụ thể; sửa và lưu trong KiCad rồi kiểm tra lại.
5. Bấm **Đi dây tự động**. Nhật ký cho biết giai đoạn xuất DSN, chạy Freerouting, nhập SES và kiểm tra DRC. Có thể bấm **Hủy**; PCB gốc được giữ nguyên.
6. Xem hình 2D, số kết nối chưa nối và vi phạm DRC. Mọi kết quả vẫn là **bản nháp**, kể cả khi đã nối hết.
7. Bấm **Lưu PCB mới** để lưu cạnh file nguồn bằng tên không trùng, hoặc **Lưu ở nơi khác…**. Sau đó dùng **Mở PCB trong KiCad** hoặc **Mở thư mục lưu**.

### PCB nào dùng được?

- File KiCad đã lưu, có **2–1000 footprint đã được đặt vị trí**, pad được gán net và ít nhất một net nối từ hai pad trở lên.
- **2–8 lớp đồng**, một đường biên kín duy nhất, chưa có lỗ cắt bên trong.
- **Chưa có track, via hoặc vùng đồng (copper zone)**. Tool không xóa đường đã đi hoặc vùng đồng. Engine hiện có thể bị kẹt khi nhận copper zone, nên tool báo lỗi trước khi xuất DSN. Hãy giữ bản gốc, tự chuẩn bị bản sao chưa đi dây và chưa có vùng đồng; sau khi autorouting, thêm/fill vùng đồng và chạy lại DRC trong KiCad. Vùng cấm (keepout/rule area) vẫn được hỗ trợ.
- Cần cài KiCad 10 đầy đủ Python/pcbnew. Nếu tool báo thiếu, chọn đúng `kicad-cli.exe` trong Cài đặt.
- Nên giữ `.kicad_pro` và `.kicad_dru` cùng tên cạnh PCB để dùng đúng netclass và custom rules. Thiếu project sẽ được báo; không nên mặc định rằng quy tắc mặc định phù hợp với mạch.

Tool dùng footprint và net đã nhúng trong PCB. Khi schematic thay đổi, hãy cập nhật bằng F8 và lưu lại trước. Footprint không được tự di chuyển, xoay hoặc đổi mặt.

### Kết quả và vị trí lưu

Ví dụ đầu vào `controller.kicad_pcb` sẽ có kết quả mới:

| File | Nội dung |
| --- | --- |
| `controller_routed.kicad_pcb` | PCB đã nhập phiên đi dây, chưa bổ sung vùng đồng. |
| `controller_routed.routing.svg` | Bản xem 2D xuất bằng KiCad. |
| `controller_routed.routing.json` | Thông tin nguồn, engine, kết quả kiểm tra và trạng thái bản nháp. |
| `controller_routed.drc.json` | Báo cáo DRC KiCad để đối chiếu. |
| `controller_routed.kicad_pro`, `.kicad_dru` | Bản sao cùng tên của project/custom rules nếu nguồn có các file này. |

Nếu tên PCB hoặc bất kỳ báo cáo/project/rule cùng tên đã có, **Lưu PCB mới** tự chọn hậu tố `_2`, `_3`… Tool không ghi đè đầu vào hoặc kết quả có sẵn. File nguồn thay đổi sau khi xem trước thì phải chạy lại.

Nên lưu cạnh PCB nguồn. Khi lưu ở thư mục khác, đường dẫn tương đối hoặc `${KIPRJMOD}` tới mô hình 3D/tài nguyên có thể cần chỉnh lại. Kết quả là PCB riêng; tool chưa tự thay thế bo chính trong project.

### Đọc trạng thái đúng

- **Đã chạy xong**: tác vụ đã kết thúc, chưa có nghĩa là bo đạt yêu cầu sản xuất.
- **0 kết nối chưa đi dây**: KiCad không liệt kê kết nối còn thiếu trong lần DRC đó. Vẫn cần xem vi phạm khác và yêu cầu riêng của mạch.
- **Còn N kết nối**: bản nháp chưa nối hết; có thể lưu để tiếp tục đi dây thủ công trong KiCad.
- **Vi phạm DRC**: mở báo cáo hoặc PCB để xử lý. Không bỏ qua các lỗi chỉ vì autorouter đã chạy xong.

Kiểm tra lại antenna/RF, cặp vi sai, impedance, dòng tải, vòng dòng chuyển mạch, tản nhiệt và khoảng cách cách điện theo mạch thực tế. Việc giữ/copy quy tắc không có nghĩa mọi ràng buộc chuyên biệt được chuyển đầy đủ qua DSN/SES. Không có bảo đảm cho ra bố trí dây giống Altium hoặc hoàn tất mọi bo.

### Nâng cao và khắc phục lỗi

- Mặc định **20 lượt thử**, giới hạn **300 giây**; có thể chọn 1–1000 lượt và 10–3600 giây. Tăng giới hạn có thể giúp engine thử thêm nhưng không bảo đảm nối hết.
- Nếu bộ đi dây chưa sẵn sàng, bấm **Thiết lập Freerouting**. Khi thiếu gói đi kèm, tool hỗ trợ tải từ nguồn chính thức; nếu không tải được, mở phần nâng cao và chọn **Chọn gói Freerouting ZIP đã tải…**.
- Chỉ dùng `freerouting-cli-2.5.0-windows-x64.zip` chính thức, **20.183.955 byte**, SHA-256 `5750dc0bed261eefae29d79784301b478af978fd472407d77e01e85dbb794857`. Gói sai hoặc bị sửa sẽ bị từ chối. [Nguồn tải](https://github.com/freerouting/freerouting/releases/tag/v2.5.0).
- Nếu PCB bị từ chối, xem dòng “Cần sửa”: thường là PCB trống, chưa gán net, biên chưa kín hoặc đã có track/via/vùng đồng. Chuẩn bị bản sao phù hợp trong KiCad rồi lưu, giữ nguyên bo gốc.
- Nếu không lưu được, chọn tên mới trong thư mục có quyền ghi. Nút **Sao chép lỗi** giữ đầy đủ chi tiết để báo lỗi.

### Engine và dữ liệu

Đây là tích hợp **Freerouting 2.5.0** độc lập, mã nguồn mở GPL-3.0, dùng native DSN/SES của KiCad. Không dùng mã nguồn Altium và không cần cài plugin KiCad.

PCB được xử lý trên máy, không tải lên dịch vụ đi dây. Tác vụ tắt analytics, profile telemetry và API/MCP server. Engine upstream vẫn có bước kiểm tra phiên bản; tool hướng HTTP qua proxy loopback `127.0.0.1:9` trong tác vụ để tránh kết nối ra ngoài. Vì vậy không mô tả engine là “không hề thử kết nối mạng”. Chỉ phương án tải runtime dự phòng mới cần liên hệ nguồn chính thức.

## English

Save an already placed KiCad PCB with assigned nets and a closed Edge.Cuts boundary, then open **Đi dây tự động…**. First use **Thiết lập Freerouting** to verify/install the bundled official native Windows x64 CLI; no Java or Internet download is needed for bundled setup. Check the PCB, autoroute, inspect the native 2D preview and DRC, then save a new board. Cancel is available during setup/routing. Automatic footprint placement has been removed.

The current input limits are 2–1000 footprints, 2–8 copper layers, one closed boundary without internal cutouts, and no existing tracks, vias or copper zones. The current engine can stall on copper zones, so these inputs are rejected before DSN export without deleting anything. Keepout rule areas are supported. Prepare a separate copy without copper zones; add/fill them and rerun DRC in KiCad after autorouting. Matching project/custom-rule files are staged and copied to the new output name; KiCad DRC runs after SES import. Footprint placement and net assignments are checked before saving. Changed inputs and existing output/sidecar names are protected.

Results remain drafts. Unconnected items and DRC violations are reported separately; a completed run is not a production-ready claim. Review RF, differential pairs, impedance, power, thermal and other circuit-specific constraints. DSN/SES does not guarantee transfer of every advanced rule.

Quick Save writes `*_routed.kicad_pcb`, `.routing.svg`, `.routing.json`, `.drc.json` and same-stem project/rule copies when available. Prefer the source folder for relative resource/model paths. Buttons open the saved board or its folder.

The engine is unmodified **Freerouting 2.5.0**, a separate GPL-3.0 program, not Altium code or a KiCad plugin. PCB data stays local. Analytics/profile telemetry/API/MCP are disabled; upstream update-check HTTP is redirected through an unused loopback proxy during the job. Runtime download fallbacks use official sources. See [SOURCE_CODE.md](SOURCE_CODE.md) and [third-party notices](THIRD_PARTY_NOTICES.md).
