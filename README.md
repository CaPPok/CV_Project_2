# HCMUT - Xử lý ảnh số và Thị giác máy tính

Project minh họa các kỹ thuật **Affine Transformation**, **Projective Transformation** và **Image Blending** trong xử lý ảnh bằng OpenCV.

Các module được viết dưới dạng notebook độc lập để dễ quan sát từng bước xử lý.

---

# Cách sử dụng

Tất cả module đều sử dụng cùng một cách load ảnh.

Chỉ cần chỉnh các biến sau:

```python
DATA_ROOT = "../Images/"
IMAGE_NAME = "sontung.jpg"
BACKGROUND_NAME = "bg1.jpg"
```

- `IMAGE_NAME` : ảnh nguồn (object/source image)
- `BACKGROUND_NAME` : ảnh nền (background image)


Để chạy với ảnh khác, chỉ cần thay đổi:

```python
IMAGE_NAME = "your_image.jpg"
BACKGROUND_NAME = "your_background.jpg"
```
## 1. Clone repository

Sử dụng git để tải project về máy:

```bash
git clone https://github.com/CaPPok/CV_Project_2.git
```

Di chuyển vào thư mục project:

```bash
cd CV_Project_2
```

---

## 2. Tạo môi trường Python

Tạo virtual environment:

```bash
python -m venv venv
```

Kích hoạt môi trường:

### Windows
```bash
venv\Scripts\activate
```

### Linux / MacOS
```bash
source venv/bin/activate
```

---

## 3. Cài đặt các thư viện cần thiết

Cài đặt các thư viện sử dụng trong project:

```bash
pip install opencv-python numpy matplotlib scipy
```

Các thư viện chính:

- `opencv-python` – xử lý ảnh
- `numpy` – tính toán ma trận
- `matplotlib` – hiển thị ảnh
- `scipy` – hỗ trợ một số phép toán số

---

## 4. Cấu trúc thư mục

```
project/
│
├── SourceCode/
│   ├── module1.ipynb
│   ├── module2.ipynb
│   └── module3.ipynb
│
└── Images/
    ├── sontung.jpg
    ├── bg1.jpg
    └── ...
```

Tất cả ảnh cần đặt trong thư mục `Images`.

---

# Module 1 – Gradient Domain Editing

Module này minh họa và so sánh nhiều kỹ thuật **gradient domain editing** để ghép một ảnh source vào background một cách tự nhiên.  
Các phương pháp blending được triển khai gồm:

- Naive Blending
- Poisson Blending
- Mixed Gradient Blending
- Laplacian Pyramid Blending

Mở notebook và chỉnh các tham số ở **cell cuối cùng** trước khi chạy:

```python
DATA_ROOT = '../Images/'      # Đường dẫn đến thư mục Images
IMAGE_NAME = "01.jpg"         # Tên file ảnh (01.jpg / 02.jpg / 03.jpg)

BLEND_TYPE = 2                # 0: Naive | 1: Laplacian | 2: Poisson
GRAD_MIX = False              # True: dùng gradient mixing (chỉ tác dụng khi BLEND_TYPE=2)

NUM_LV = 3                    # Số levels pyramid (chỉ tác dụng khi BLEND_TYPE=1)

USE_MANUAL_OFFSET = True      # True: chọn vị trí thủ công | False: dùng target_configs
```

---

### Chọn vị trí thủ công
Khi `USE_MANUAL_OFFSET = True`, một cửa sổ OpenCV sẽ mở ra.

| Thao tác | Tác dụng |
|--------|--------|
| Di chuột / Click trái | Chọn vị trí đặt source lên target |
| Scroll lên / phím `+` | Phóng to source (+5% mỗi bước) |
| Scroll xuống / phím `-` | Thu nhỏ source (-5% mỗi bước) |
| `Enter` | Xác nhận và tiếp tục |

---

### Cấu hình vị trí tự động

Khi `USE_MANUAL_OFFSET = False`, chỉnh vị trí trong `target_configs`:

```python
target_configs = {
    "01.jpg": [H0, W0, scale],   # H0, W0: offset góc trên-trái; scale: tỉ lệ resize
    "02.jpg": [H0, W0, scale],
    "03.jpg": [H0, W0, scale],
}
```

---

### Kết quả

Kết quả được **lưu tự động** vào thư mục:

```
Images/result/
```

# Module 2 – Biến đổi hình học (Transformation)

Module này minh họa các phép **biến đổi hình học cơ bản** trong xử lý ảnh:

- **Affine Transformation**
- **Projective Transformation (Homography)**

Các phép biến đổi được áp dụng để thay đổi vị trí, góc nhìn và hình dạng của ảnh.

### Nội dung chính

- Thực hiện các phép biến đổi affine (rotation, scaling, shearing, translation).
- Thực hiện projective transformation để mô phỏng thay đổi phối cảnh.
- Quan sát sự khác nhau giữa affine và projective transformation.

### Chạy với ảnh khác

Chỉ cần thay đổi:

```python
IMAGE_NAME = "new_image.jpg"
```

---

# Module 3 – Thử nghiệm mở rộng

Module này áp dụng **Projective Transformation (Homography)** để đặt một ảnh source lên một mặt phẳng bất kỳ trong ảnh background.

### Quy trình chính

1. Load ảnh source và background.
2. Chọn các điểm tương ứng bằng cách click chuột trên background.
3. Tính ma trận **homography**.
4. Warp ảnh source để phù hợp với phối cảnh của background.

Sau đó ảnh source sẽ được **biến dạng theo phối cảnh và đặt đúng vị trí trên ảnh nền**.

### Chạy với ảnh khác

Chỉ cần thay đổi:

```python
IMAGE_NAME = "new_source.jpg"
BACKGROUND_NAME = "new_background.jpg"
```