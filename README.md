# CV Project 2 — Gradient Domain Editing & Geometric Transformations

Bài tập lớn môn Computer Vision, gồm hai phần chính:
- **Module 1**: Ghép ảnh trong miền gradient (Naive Copying, Laplacian Blending, Poisson Blending)
- **Module 2**: Các phép biến đổi hình học (Affine & Projective Transformation)
- **Module 3**: Thử nghiệm mở rộng — dán ảnh lên mặt phẳng nghiêng trong không gian

---

## Cấu trúc thư mục

```
CV_Project_2/
│
├── Images/
│   ├── source/          # Ảnh nguồn chứa đối tượng cần ghép (01.jpg, 02.jpg, 03.jpg)
│   ├── mask/            # Mask xác định vùng đối tượng (01.jpg, 02.jpg, 03.jpg)
│   ├── target/          # Ảnh nền (01.jpg, 02.jpg, 03.jpg)
│   ├── result/          # Ảnh kết quả sau khi ghép (tự động tạo)
│   ├── sontung.jpg      # Ảnh nguồn dùng cho module 2 & 3
│   └── bg1.jpg          # Ảnh nền dùng cho module 3
│
├── module1.ipynb        # Gradient Domain Editing
├── module2.ipynb        # Geometric Transformations
└── module3.ipynb        # Thử nghiệm mở rộng
```

---

## Yêu cầu

```
python >= 3.8
numpy
scipy
opencv-python
matplotlib
```

Cài đặt nhanh:

```bash
pip install numpy scipy opencv-python matplotlib
```

---

## Hướng dẫn sử dụng

### Module 1 — Gradient Domain Editing (`module1.ipynb`)

Mở notebook và chỉnh các tham số ở **cell cuối cùng** trước khi chạy:

```python
DATA_ROOT    = 'đường/dẫn/đến/Images/'   # Đường dẫn tới thư mục Images
IMAGE_NAME   = "01.jpg"                   # Tên file ảnh (01.jpg / 02.jpg / 03.jpg)
BLEND_TYPE   = 2                          # 0: Naive | 1: Laplacian | 2: Poisson
GRAD_MIX     = False                      # True: dùng gradient mixing (chỉ có tác dụng khi BLEND_TYPE=2)
NUM_LV       = 3                          # Số levels pyramid (chỉ có tác dụng khi BLEND_TYPE=1)
USE_MANUAL_OFFSET = True                  # True: chọn vị trí thủ công | False: dùng target_configs
```

**Khi `USE_MANUAL_OFFSET = True`**, một cửa sổ OpenCV sẽ mở ra:

| Thao tác | Tác dụng |
|---|---|
| Di chuột / Click trái | Chọn vị trí đặt source lên target |
| Scroll lên / phím `+` | Phóng to source (+5% mỗi bước) |
| Scroll xuống / phím `-` | Thu nhỏ source (−5% mỗi bước) |
| `Enter` | Xác nhận và tiếp tục |

**Khi `USE_MANUAL_OFFSET = False`**, chỉnh bảng `target_configs` trong cell đó:

```python
target_configs = {
    "01.jpg": [H0, W0, scale],   # H0, W0: offset góc trên-trái; scale: tỉ lệ resize
    "02.jpg": [H0, W0, scale],
    "03.jpg": [H0, W0, scale],
}
```

Kết quả được lưu tự động vào `Images/result/`.

---

### Module 2 — Geometric Transformations (`module2.ipynb`)

Chỉnh đường dẫn ảnh ở **cell đầu tiên**:

```python
img_path = "đường/dẫn/đến/sontung.jpg"
```

Notebook thực hiện lần lượt các phép biến đổi sau với các tham số mặc định:

| Cell | Phép biến đổi | Tham số mặc định |
|---|---|---|
| Translation | Tịnh tiến | `tx=50, ty=50` |
| Rotation | Quay | `θ=15°` |
| Scaling | Co giãn | `sx=0.5, sy=0.5` |
| Shearing | Kéo xiên | `θx=70°, θy=85°` |
| Rotation + Translation | Kết hợp | `θ=15°, tx=50, ty=50` |
| Scaling + Translation | Kết hợp | `sx=0.5, sy=0.8, tx=100, ty=30` |
| Rotation + Scaling | Kết hợp | `θ=15°, sx=0.8, sy=0.8` |
| Shearing + Translation | Kết hợp | `θx=70°, θy=85°, tx=20, ty=30` |
| 4 phép đồng thời | Kết hợp | `θ=30°, sx=1.2, sy=0.8, θx=70°, θy=85°, tx=40, ty=30` |
| Affine từ điểm tương ứng | `cv2.getAffineTransform` | 3 cặp điểm cố định |
| Projective | `cv2.getPerspectiveTransform` | 4 cặp điểm cố định |

---

### Module 3 — Thử nghiệm mở rộng (`module3.ipynb`)

Chỉnh đường dẫn ảnh ở **cell đầu tiên**:

```python
bg_path  = "đường/dẫn/đến/bg1.jpg"       # Ảnh nền
src_path = "đường/dẫn/đến/sontung.jpg"   # Ảnh nguồn cần dán lên nền
```

Chạy các cell theo thứ tự. Khi đến **cell chọn điểm neo**, một cửa sổ OpenCV sẽ mở:

1. **Click chuột 4 lần** lên ảnh nền để chọn 4 điểm neo theo thứ tự: trên-trái → trên-phải → dưới-phải → dưới-trái (hoặc bất kỳ thứ tự nhất quán nào).
2. Nhấn bất kỳ phím nào để đóng cửa sổ và tiếp tục.

Pipeline sau đó tự động tính Homography, warp ảnh nguồn vào vùng đã chọn và blend vào ảnh nền.

---

## Các phương pháp blend (Module 1)

| `BLEND_TYPE` | Phương pháp | Đặc điểm |
|---|---|---|
| `0` | Naive Copying | Nhanh, nhưng tạo viền cứng tại biên |
| `1` | Laplacian Blending | Biên mượt hơn nhờ blend đa tần số, nhưng không xử lý được chênh lệch màu lớn |
| `2` | Poisson Blending | Chất lượng tốt nhất, tự điều chỉnh màu theo điều kiện biên của ảnh nền |

Với `BLEND_TYPE=2`:
- `GRAD_MIX=False` (khuyến nghị): guidance field lấy hoàn toàn từ gradient source
- `GRAD_MIX=True`: guidance field lấy gradient lớn hơn giữa source và target — phù hợp khi ghép vật thể trong suốt, **không khuyến nghị** khi ảnh nền có texture mạnh hơn đối tượng

---

## Lưu ý

- Thư mục `Images/result/` cần tồn tại trước khi chạy module 1, hoặc tạo thủ công:
  ```bash
  mkdir Images/result
  ```
- Đường dẫn trong code dùng dấu `\\` (Windows). Nếu dùng Linux/macOS, thay bằng `/`:
  ```python
  DATA_ROOT = '/đường/dẫn/đến/Images/'
  source = cv2.imread(DATA_ROOT + "source/" + image_name)
  ```
- Module 1 yêu cầu 3 file ảnh có **cùng tên** trong cả 3 thư mục `source/`, `mask/`, `target/`.