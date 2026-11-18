# [BTTH] Bài tập thực hành tổng hợp
## Bước 1: Prompt kiểm tra và làm sạch dữ liệu đầu vào (Data Validation & Cleaning)
[Role] Bạn là một bộ lọc dữ liệu backend chuyên nghiệp. 
[Task] Hãy tiếp nhận dữ liệu đơn hàng thô dưới đây và thực hiện bước kiểm tra tính hợp lệ ban đầu:
- Danh sách sản phẩm:
  1. Mã SP: SP01 | Tên: Tai nghe Bluetooth X1 | Số lượng: 2 | Đơn giá: 500.000 VNĐ
  2. Mã SP: SP02 | Tên: Cáp sạc nhanh Type-C | Số lượng: -1 | Đơn giá: 150.000 VNĐ
  3. Mã SP: UNKNOWN | Tên: Sản phẩm không rõ mã | Số lượng: 1 | Đơn giá: 200.000 VNĐ

[Ràng buộc kỹ thuật & Xử lý lỗi biên]:
- Nếu số lượng <= 0, đánh dấu là sản phẩm lỗi (INVALID_INPUT) và chuyển vào danh sách loại bỏ.
- Nếu mã sản phẩm không tồn tại (UNKNOWN), đánh dấu là lỗi (UNKNOWN_PRODUCT).
- Chỉ giữ lại các sản phẩm hợp lệ cho bước xử lý tiếp theo.
## Bước 2: Prompt tính toán logic dòng tiền (Chain-of-Thought Financial Calculation)
[Role] Dựa trên danh sách sản phẩm hợp lệ từ Bước 1, bạn là chuyên gia tính toán tài chính.
[Task] Áp dụng tư duy Chain-of-Thought (từng bước một) để xử lý:
- Bước 1: Tính thành tiền cho từng sản phẩm hợp lệ (Số lượng x Đơn giá).
- Bước 2: Cộng dồn tổng tiền tạm tính (subtotal).
- Bước 3: Áp dụng mã giảm giá (Mã: SALE10 - Giảm 10% tổng đơn hàng).
- Bước 4: Cộng thêm phí vận chuyển cố định 30.000 VNĐ để ra tổng thanh toán cuối cùng (final_total).
## Bước 3: Prompt định dạng kết quả đầu ra chuẩn JSON (JSON Output Generation)
[Role] Bạn là hệ thống chuyển đổi API backend.
[Task] Tổng hợp kết quả từ các bước trước và xuất ra duy nhất định dạng JSON thô (Raw JSON) theo cấu trúc chuẩn:
- order_status: Trạng thái đơn hàng ("PROCESSED_WITH_WARNINGS")
- customer_info: Thông tin khách hàng
- validated_items: Danh sách sản phẩm hợp lệ kèm thành tiền
- rejected_items: Danh sách sản phẩm bị loại kèm lý do lỗi
- financial_calculation: Chi tiết subtotal, discount, shipping_fee, final_total
- reasoning_steps: Nhật ký các bước suy luận logic

[Ràng buộc tuyệt đối]: Không kèm theo bất kỳ văn bản chào hỏi hay Markdown code block nào ngoài cấu trúc JSON.
## Phần 3: Kết quả chạy thử thực tế (Simulation Output)
{
  "order_status": "PROCESSED_WITH_WARNINGS",
  "customer_info": {
    "name": "Nguyễn Văn A",
    "phone": "0901234567",
    "address": "123 Lê Lợi, Phường Bến Thành, Quận 1, TP.HCM"
  },
  "validated_items": [
    {
      "product_id": "SP01",
      "product_name": "Tai nghe Bluetooth X1",
      "quantity": 2,
      "unit_price": 500000,
      "line_total": 1000000,
      "status": "VALID"
    }
  ],
  "rejected_items": [
    {
      "product_id": "SP02",
      "product_name": "Cáp sạc nhanh Type-C",
      "quantity": -1,
      "reason": "INVALID_INPUT: Số lượng sản phẩm nhỏ hơn hoặc bằng 0."
    },
    {
      "product_id": "UNKNOWN",
      "product_name": "Sản phẩm không rõ mã",
      "quantity": 1,
      "reason": "UNKNOWN_PRODUCT: Mã sản phẩm không tồn tại trong hệ thống."
    }
  ],
  "financial_calculation": {
    "subtotal": 1000000,
    "discount_code": "SALE10",
    "discount_amount": 100000,
    "shipping_fee": 30000,
    "final_total": 930000
  },
  "reasoning_steps": [
    "Bước 1 (Lọc lỗi): Loại bỏ SP02 do số lượng âm (-1) và UNKNOWN do mã không hợp lệ. Chỉ giữ lại SP01.",
    "Bước 2 & 3 (Tính toán): Tạm tính cho SP01: 2 * 500.000 = 1.000.000 VNĐ.",
    "Bước 4 (Hoàn tất): Áp dụng mã giảm 10% (100.000 VNĐ), cộng phí vận chuyển 30.000 VNĐ. Tổng thanh toán cuối cùng = 930.000 VNĐ."
  ]
}