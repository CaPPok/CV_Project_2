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

```

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

## 2. Tạo môi trường Python (khuyến nghị)

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

# Module 1 – Gradient domain editing

Module này minh họa và so sánh nhiều kỹ thuật **gradient domain editing** để ghép một ảnh source vào background một cách tự nhiên.  
Các phương pháp blending được triển khai gồm:

- Naive Blending
- Poisson Blending
- Mixed Gradient Blending
- Laplacian Pyramid Blending


# Module 2 – Transformation

Module này minh họa **Affine Transformation** và **Projective Transformation**

### Chạy với ảnh khác

Chỉ cần thay đổi:

```python
IMAGE_NAME = "your_image.jpg"
```

---

# Module 2 – Thử nghiệm mở rộng

Module này thực hiện **Projective Transformation (Homography)** để đặt ảnh source lên một mặt phẳng bất kỳ trên background.


```python
IMAGE_NAME = "new_source.jpg"
BACKGROUND_NAME = "new_background.jpg"
```

---

# Cấu trúc thư mục

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

