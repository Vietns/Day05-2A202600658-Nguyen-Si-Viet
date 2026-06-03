# App Teardown — MoMo Moni

## 1. Chọn sản phẩm

- Product: MoMo Moni
- AI feature: trợ lý tài chính, phân tích chi tiêu, gợi ý tiết kiệm
- Cách truy cập: app MoMo, mục Moni / trợ lý tài chính
- Track liên quan: Personal Finance

## 2. Dùng thử: promise vs reality

- Product hứa gì?
  - Giúp user hiểu tình hình chi tiêu, theo dõi tài chính cá nhân và nhận gợi ý tiết kiệm.
- User được hứa sẽ được giúp?
  - Người dùng MoMo có thu nhập cố định, dùng ví để thanh toán hằng ngày và muốn kiểm soát tổng chi tiêu trong tháng.
- Kỳ vọng AI làm được task nào?
  - Cho user đặt mức chi tiêu tối đa theo tháng.
  - Theo dõi tổng tiền đã chi so với ngân sách.
  - Cảnh báo khi user sắp vượt hoặc đã vượt ngân sách.
  - Dự báo cuối tháng user có thể tiêu bao nhiêu nếu giữ tốc độ hiện tại.
  - Gợi ý mức chi còn lại mỗi ngày/tuần để quay về kế hoạch.
- Dùng thật, điểm gãy xuất hiện ở đâu?
  - Moni có thể trả lời về chi tiêu ở mức tổng quan, nhưng chưa thể hiện rõ flow đặt ngân sách tối đa theo tháng.
  - Khi user hỏi tình huống "ngân sách tháng là 10 triệu, sau 2 tuần đã tiêu hơn 10 triệu", Moni chưa nhất thiết đưa ra cảnh báo/dự báo có cấu trúc.
  - Gợi ý tiết kiệm thường dễ bị chung chung nếu không gắn với số tiền đã chi, số ngày còn lại và tốc độ chi tiêu hiện tại.
  - Nếu có một khoản chi lớn bất thường như học phí, trả nợ hoặc mua đồ một lần, AI có thể dự báo quá căng nếu không hỏi user xác nhận khoản đó có nên tính vào xu hướng chi tiêu hay không.

### Evidence quan sát

- Screenshot/quote cần đính kèm:
  - Màn hình hỏi Moni về ngân sách tháng hoặc tình trạng chi tiêu.
  - Câu trả lời của Moni khi user đưa tình huống đã chi vượt mức.
- Prompt/input đã thử:
  - "Tôi muốn đặt ngân sách chi tiêu tháng này là 10 triệu."
  - "Tôi đã tiêu 7,2 triệu sau 15 ngày, ngân sách tháng là 10 triệu. Tôi có đang chi quá nhanh không?"
  - "Nếu giữ tốc độ chi tiêu hiện tại, cuối tháng tôi sẽ tiêu khoảng bao nhiêu?"
  - "Tôi đã tiêu hơn 10 triệu sau 2 tuần, hãy cảnh báo và gợi ý tôi nên làm gì."
  - "Khoản học phí 3 triệu là khoản chi bất thường, hãy loại khỏi dự báo tháng sau."
- Hành vi cần quan sát:
  - Moni có hiểu ngân sách 10 triệu là một giới hạn cần theo dõi không?
  - Moni có tính được phần trăm ngân sách đã dùng không?
  - Moni có dự báo tổng chi cuối tháng không?
  - Moni có đưa ra mức chi còn lại mỗi ngày/tuần không?
  - Moni có hỏi lại khi có khoản chi bất thường làm sai dự báo không?

## 3. Vẽ 4 paths

| Path | Tóm tắt |
|---|---|
| Happy | User đặt ngân sách 10 triệu/tháng. Sau 15 ngày mới chi 4,5 triệu. AI báo user đang trong kế hoạch, còn 5,5 triệu cho 15 ngày còn lại và có thể chi khoảng 366.000đ/ngày. |
| Low-confidence | User đã chi 7,2 triệu sau 15 ngày, trong đó có một khoản học phí 3 triệu. AI không chắc đây là chi thường xuyên hay bất thường, nên hỏi user có muốn loại khoản này khỏi dự báo không. |
| Failure | User đã tiêu hơn 10 triệu sau 2 tuần nhưng AI chỉ đưa lời khuyên chung chung kiểu "nên tiết kiệm hơn", không cảnh báo rõ đã vượt ngân sách và không tính dự báo cuối tháng. |
| Correction | User đánh dấu một khoản chi là bất thường hoặc chỉnh lại ngân sách tháng. AI tính lại dự báo, cập nhật số tiền còn nên chi mỗi ngày/tuần và lưu lựa chọn cho lần sau. |

## 4. Finding thành quyết định

Khi user đặt ngân sách chi tiêu tối đa theo tháng và hỏi mình có đang chi quá nhanh không,
AI/product chỉ trả lời tổng quan về chi tiêu hoặc gợi ý tiết kiệm chung chung,
thay vì so sánh chi tiêu thực tế với ngân sách, tính tốc độ chi tiêu và dự báo cuối tháng.
Hậu quả là user chỉ phát hiện mình vượt ngân sách khi đã quá muộn, khó điều chỉnh chi tiêu cho phần còn lại của tháng.

Lỗi thuộc layer Data-tool + Intent + UX recovery.

Nên sửa bằng requirement:

- Cho user nhập ngân sách tháng.
- Tính số tiền đã chi, số ngày đã qua, số ngày còn lại.
- Dự báo tổng chi cuối tháng theo tốc độ hiện tại.
- Cảnh báo khi user sắp vượt hoặc đã vượt ngân sách.
- Gợi ý mức chi còn lại mỗi ngày/tuần.
- Khi có khoản chi lớn bất thường, hỏi user xác nhận có loại khỏi dự báo hay không.

**Finding đổi SPEC:** SPEC cần dịch từ "Moni gợi ý tiết kiệm chung" thành "Moni cảnh báo và dự báo vượt ngân sách dựa trên tốc độ chi tiêu thực tế, nhưng cho user sửa/đánh dấu khoản chi bất thường trước khi chốt dự báo".

## 5. Sketch as-is / to-be

- As-is:
  - User mở Moni.
  - User hỏi: "Tôi đặt ngân sách 10 triệu/tháng, sau 2 tuần đã tiêu 10,8 triệu thì sao?"
  - AI trả lời lời khuyên tài chính chung.
  - User không thấy rõ đã vượt bao nhiêu, cuối tháng có thể vượt bao nhiêu, và cần giảm chi thế nào.

- To-be:
  - User mở Moni.
  - User đặt ngân sách: 10.000.000đ/tháng.
  - AI lấy dữ liệu chi tiêu hiện tại: đã chi 10.800.000đ sau 14 ngày.
  - AI cảnh báo: user đã dùng 108% ngân sách.
  - AI dự báo: nếu giữ tốc độ hiện tại, cuối tháng có thể chi khoảng 23.100.000đ.
  - AI chỉ ra top nhóm/khoản chi làm tăng tốc độ chi.
  - AI hỏi user có khoản chi bất thường nào cần loại khỏi dự báo không.
  - User đánh dấu khoản chi bất thường hoặc giữ nguyên.
  - AI tính lại mức chi gợi ý cho phần còn lại của tháng.

## 6. Check list

- [x] Có ít nhất 1 observation/prompt cụ thể từ workflow Moni.
- [x] Có đủ 4 paths: Happy, Low-confidence, Failure, Correction.
- [x] Finding được viết thành product decision, không chỉ là nhận xét.
- [x] Sketch có as-is và to-be.
- [x] Có một câu nói rõ finding này sẽ đổi gì trong SPEC.
