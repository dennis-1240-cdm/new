# Menu OCR API (Docker)
API nhận diện menu từ ảnh (đồ ăn / thức uống) sử dụng PaddleOCR + VietOCR.  

---

## 1. Input file*
- Ảnh menu đầu vào (Bắt buộc phải có)
---
## 2. Tham số OCR
```bash
ppocr_lang
```
- Ngôn ngữ cho PPOCR
- Bao gồm : "vi" (mặc định) và "en" 
---

```bash
use_angle_cls
```
- true - Nhận chữ bị xoay
- false - OCR nhanh hơn nhưng dễ sai khi gặp chữ bị xoay
  
---

```bash
use_gpu_ppocr
```
- true - Dùng GPU (Nếu server có GPU)
- false - Dùng CPU (mặc định)
  
---
## 3. VietOCR (Refine tiếng Việt)

```bash
enable_vietocr
```
- true - sẽ check lại hết những chữ đã OCR mà lỗi dấu tiếng Việt để sửa lại dấu (sẽ chạy hơi lâu)
- false - chỉ dùng Paddle OCR thì sẽ có lỗi dấu nếu nhận diện tiếng Việt nhưng chạy nhanh hơn
  
---

```bash
vietocr_device
```
- "cpu"  - chạy CPU
- "cuda:0"- chạy GPU

---
```bash
vietocr_conf_threshold
```
- Ngưỡng confidence (độ chính xác) để quyết định có refine OCR (sửa dấu tiếng Việt)
- OCR conf < threshold thì VietOCR mới được gọi ra để sửa
- Lưu ý: Nên để 0.99 tại vì OCR hiện tại có độ chính xác trên 0.85 có cái lên tới 0.98 nên là để 0.99 (sau khi em fix được bản mới tốt hơn thì em sẽ set sẵn 0.99 luôn ạ)

---
## 4. Lọc nhiễu OCR

```bash
enable_generic_filter
```
- true - bật lọc nhiễu (Header, Banner, Hotline,...)
- false - giữ nguyên toàn bộ text thì sẽ có trường hợp là sẽ bị lỗi về việc sẽ xuất JSON hết mọi thứ
---
## 5. Chế độ menu
```bash
name_price
```
- Nhận diện món ăn có một giá
  
---

```bash
size_prices
```
- Nhận diện món ăn nhiều giá (S / M / L)
  
---
## 6. Xử lý menu nhiều cột

```bash
s_column_index
```
- Xác định côt bắt đầu chưa giá tính từ 0
- mặc định 2
- Sẽ không tinh chỉnh cột này nếu như menu đơn giản
  
---

```bash
max_columns
```
- Số cột tối đa hệ thống tự detect
- áp dụng cho menu nhiều cột
- Lưu ý: Không cần thay đổi tại vì em có cho hệ thống tự detect 
---

## 7. Ràng buộc size (menu nhiều size)

```bash
require_three_prices
```
- true- Chỉ lấy món đủ 3 size
- false - Cho phép 1 hoặc 2 size, có 3 size vẫn lấy

---

## Kiến Trúc Tổng quát

```bash
Client (curl / browser)
        ↓
Docker Container
        ↓
FastAPI (Uvicorn)
        ↓
PaddleOCR + VietOCR
        ↓
JSON Output
```
