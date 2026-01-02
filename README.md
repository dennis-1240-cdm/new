# Menu OCR API (Docker)
API nhận diện menu từ ảnh (đồ ăn / thức uống) sử dụng PaddleOCR + VietOCR.  

---

## 1. Yêu cầu
- Docker Engine / Docker Desktop
- Windows / macOS / Linux
- Trình duyệt hoặc `curl`
> - Windows / Linux / macOS Intel: chạy tốt image `amd64`  

---
## 2. Cách setup docker bằng lệnh

```bash
gunzip ocr-api_1.1.tar.gz
docker load -i ocr-api_1.1.tar


---

3. Chạy nhanh (Local)

docker run -p 8000:8000 ocr-api:1.1

---
4. Chạy local


http://localhost:8000/docs

---
5. Kiểm tra API


curl http://localhost:8000/health

---
6. Gọi OCR bằng curl
Menu đồ uống (size + giá)


curl -X POST "http://localhost:8000/ocr" \
  -F "file=@menu.jpg" \
  -F "mode=size_prices" \
  -F "use_gpu_ppocr=false"

Menu đồ ăn (tên + giá)


curl -X POST "http://localhost:8000/ocr" \
  -F "file=@menu.jpg" \
  -F "mode=name_price" \
  -F "use_gpu_ppocr=false"

Nếu tên file có khoảng trắng dùng:  -F "file=@/full/path/My Menu Image.png"

---
7. Chạy cache để tránh tải lại model OCR

docker run -p 8000:8000 \
  -v ocr_cache:/root/.cache \
  ocr-api:1.1




---
Kiến Trúc Tổng quát
Client (curl / browser)
        ↓
Docker Container
        ↓
FastAPI (Uvicorn)
        ↓
PaddleOCR + VietOCR
        ↓
JSON Output
