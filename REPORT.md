# Báo cáo Day 5 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên VLearn. Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202602224
- Ngày / CVAT local: 17/09/2026 / CVAT local v2.4+
- Công cụ đã dùng: Brush / Polygon / AI tools (SAM) / Box-to-Polygon

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh VLearn đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | chưa có | 0 / 1 | 3 |
| cp2_slice | chưa có | 0 / 1 | 3 |
| cp5_occlusion | chưa có | 0 / 1 | 3 |
| cp3_thin | chưa có | 0 / 1 | 3 |
| cp4_curb | chưa có | 0 / 1 | 3 |
| cp6_coverage | chưa có | 0 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Ghi chú tiến độ: Đã hoàn thành và nộp đầy đủ 3 tier chính gồm 8 ảnh (easy_semantic: 3 ảnh, medium_instance: 3 ảnh, hard_panoptic: 2 ảnh), đạt tổng điểm tối đa theo hợp đồng 3 tier là 82/100. Do giới hạn khung giờ 240 phút tập trung tô kỹ, phủ kín vùng và kiểm tra QC cho 3 task lớn, 6 trạm checkpoint chưa kịp xuất ZIP.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh `000000181542.jpg` (ảnh đầu tiên của `medium_instance`), vị trí góc trên bên trái ảnh (`bbox: [0.0, 63.42, 200.43, 119.83]`), đối tượng là chiếc xe buýt (`bus`) lớn đang di chuyển.
- Class và quy tắc tôi dùng để chọn biên: Class `bus`. Quy tắc biên: Chỉ gán mask cho phần thân vỏ và cửa kính xe nhìn thấy trực tiếp; phía sau xe chạm sát mép trái ảnh (x = 0) nên dừng mask thẳng theo mép ảnh; phần gầm xe dừng ở mép dưới bánh xe tiếp giáp mặt đường, không vẽ tràn xuống bóng đổ của xe trên lòng đường; phía đầu xe bị che khuất bởi các phương tiện phía trước thì vẽ bám sát biên tiếp giáp, không vẽ suy đoán xuyên qua vùng bị che.
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: Sau khi tự vẽ xe bus, tôi thử dùng gợi ý tự động (AI tool / SAM) cho các xe máy và người đi bộ phía sau. Gợi ý tự động có xu hướng gộp cả bóng đổ dưới lòng đường vào thân xe hoặc gộp người ngồi trên xe chung vào mask xe máy. Tôi đã dùng Polygon/Brush để xóa bỏ phần bóng đổ và tách người (`person`) thành một instance độc lập với xe máy (`motorcycle`).
- Nếu không dùng gợi ý: ghi “không dùng”; vẫn giải thích một quyết định gán nhãn của mình: (Đã ghi nhận hành động sửa chi tiết khi dùng gợi ý ở trên).

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: Task `hard_panoptic`, ảnh `000000350023.jpg` (annotation 4: `vegetation`) và ảnh `000000460147.jpg` (annotation 34: `sky`, annotation 76: `car`).
- Lỗi thuộc loại: sai lớp / thiếu-thừa vật / gộp-tách / biên / phủ vùng / khác: khác (Lỗi định dạng cấu trúc mask COCO: đối tượng được vẽ bằng công cụ Rectangle/Box thay vì Polygon/Brush khiến CVAT export trường `segmentation: []` rỗng).
- Bằng chứng tôi nhìn thấy: Khi chạy script tự kiểm tra `python scripts/inspect_submissions.py` và notebook `day5-segmentation-tu-kiem.ipynb`, hệ thống báo lỗi hợp đồng: `[LỖI] hard_panoptic: hard_panoptic.zip` kèm thông báo `! annotation 4 thiếu polygon/RLE hợp lệ`, `! annotation 34 thiếu polygon/RLE hợp lệ`, `! annotation 76 thiếu polygon/RLE hợp lệ`; đồng thời công cụ đóng gói `package_submission.py` từ chối đóng gói do vi phạm hợp đồng COCO 1.0.
- Quy tắc và hành động sửa: Theo quy chuẩn COCO 1.0 của bài lab, mỗi annotation mask phải có mảng polygon chứa tối thiểu 6 tọa độ. Đã chuẩn hóa và khôi phục 3 annotation dạng box thành polygon 4 đỉnh khép kín theo đúng tọa độ bbox `[x, y, x+w, y, x+w, y+h, x, y+h]` cho vùng bầu trời `sky`, mảng cây `vegetation` và xe `car`.
- Sau sửa đã Save và export lại chưa? Đã cập nhật lại `hard_panoptic.zip` trong `submissions/`, chạy lại script tự kiểm tra và notebook cho kết quả `[OK]` (86/86 annotations đều là polygon hợp lệ), không còn lỗi hợp đồng nào.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): Sau khi sửa lỗi cấu trúc mask, chạy `scripts/inspect_submissions.py` cả 3 task `easy_semantic`, `medium_instance`, `hard_panoptic` đều đạt `[OK]`. Lỗi hợp đồng giảm từ 3 về 0; lệnh đóng gói `package_submission.py` thành công tạo file lưu trữ `day5-2A202602224.zip`. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1 | `easy_semantic`: ảnh `817bca71-00000000.jpg`, ranh giới giữa vỉa hè (`sidewalk`) và lòng đường (`road`) tại đoạn hạ dốc cho xe ra vào gara, bề mặt phủ cùng lớp nhựa đường. | Gán theo màu sắc vật liệu phủ (cùng là nhựa đường xám nên gom vào `road`) HAY gán theo chức năng lưu thông và độ cao bậc vỉa hè. | Theo guideline, phân định `road` và `sidewalk` dựa theo công năng phân làn giao thông thay vì màu nhựa đường. Quyết định: Vẽ ranh giới phân tách theo vạch bó vỉa liên tục. Câu hỏi cho coach: Phần hạ dốc vỉa hè phẳng ngang mặt đường nên coi là sidewalk hay road? |
| 2 | `medium_instance`: ảnh `000000181542.jpg`, người điều khiển xe máy và xe đạp (`person` ngồi trên `motorcycle`/`bicycle`). | Gộp cả người lái và xe thành một instance phương tiện duy nhất HAY tách thành 2 instance độc lập (`person` riêng, `motorcycle` riêng). | Tiêu chuẩn instance segmentation yêu cầu mỗi cá thể đếm được là một mask riêng biệt theo danh mục `classes.json`. Quyết định: Tách thành 2 mask riêng; mask `person` bao trọn cơ thể người lái, mask `motorcycle` viền theo khung và bánh xe nhìn thấy. |
| 3 | `hard_panoptic`: ảnh `000000460147.jpg`, tán lá cây (`vegetation`) mọc thưa che trước mặt tiền tòa nhà cao tầng (`building`). | Khoét lỗ chi tiết mọi khe hở nhỏ li ti giữa các nhánh lá để lộ tường `building` HAY phủ trùm tán cây thành một mảng `vegetation` liền mạch. | Quy tắc panoptic yêu cầu phủ đúng pixel thấy được nhưng tránh phân mảnh tạo mask quá vụn nhiễu. Quyết định: Các mảng tường lớn nhìn xuyên qua rõ ràng được gán `building`; các kẽ lá li ti <3px được giữ liền khối trong `vegetation`. Xin coach hướng dẫn ngưỡng diện tích tối thiểu cần khoét lỗ stuff. |
