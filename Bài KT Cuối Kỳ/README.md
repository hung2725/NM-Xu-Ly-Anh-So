# Nhập Môn Xử Lý Ảnh Số - Bài Kiểm Tra Cuối Kỳ

- **Sinh Viên Thực Hiên:** Phạm Thế Hùng
- **MSSV:** 2374802010164
- **Môn Học:** Nhập Môn Xử Lý Ảnh Số
- **Giảng viên:** Đỗ Hữu Quân

## 1. Bài 1

### Yêu cầu
Cho một ảnh bất kỳ (tên ảnh do sinh viên tự đặt, ví dụ: `my_image.jpg`) và thực hiện các yêu cầu sau:

1. **Bilateral Filter (0.5 điểm):** Làm mịn ảnh
2. **Canny Edge Detection (0.5 điểm):** Xác định biên của hình ảnh
3. **Hoán đổi kênh màu (0.5 điểm):** Đổi màu ảnh bằng cách hoán đổi kênh màu theo thứ tự (ví dụ: BGR → BRG) và lưu thành tên dạng `[ten_anh]_swapped.jpg`
4. **Chuyển đổi YCrCb (0.5 điểm):** Chuyển ảnh sang không gian màu YCrCb và tách riêng 3 kênh Y, Cr, Cb, lưu thành ảnh grayscale tương ứng (`[ten_anh]_Y.jpg`, `[ten_anh]_Cr.jpg`, `[ten_anh]_Cb.jpg`)

## Công nghệ sử dụng
- **Python:** Ngôn ngữ chính
- **OpenCV:** Xử lý ảnh và computer vision
- **NumPy:** Xử lý mảng và tính toán số học
- **Matplotlib:** Hiển thị ảnh và biểu đồ
- **PIL (Pillow):** Xử lý ảnh cơ bản
- **SciPy:** Thư viện khoa học và kỹ thuật

### Bilateral Filter
**Mục đích:**  
- Làm mịn ảnh trong khi vẫn giữ được các cạnh và chi tiết quan trọng
- Loại bỏ nhiễu mà vẫn bảo toàn cấu trúc ảnh

**Code chính**
```python
import cv2
import numpy as np

# Đọc ảnh
img = cv2.imread('my_image.jpg')

# Áp dụng Bilateral Filter
bilateral = cv2.bilateralFilter(img, 9, 75, 75)

# Hiển thị kết quả
cv2.imshow('Original', img)
cv2.imshow('Bilateral Filter', bilateral)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

### Canny Edge Detection
**Mục đích:** Phát hiện biên cạnh trong ảnh một cách chính xác và hiệu quả

**Code chính**
```python
import cv2
import numpy as np

# Đọc ảnh
img = cv2.imread('my_image.jpg', 0)

# Áp dụng Canny Edge Detection
edges = cv2.Canny(img, 100, 200)

# Hiển thị kết quả
cv2.imshow('Original', img)
cv2.imshow('Canny Edges', edges)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

### Hoán đổi kênh màu
**Mục đích:** Thay đổi thứ tự các kênh màu để tạo hiệu ứng màu sắc khác nhau

**Code chính**
```python
import cv2
import numpy as np

# Đọc ảnh
img = cv2.imread('my_image.jpg')

# Hoán đổi kênh màu BGR -> BRG
swapped = img.copy()
swapped[:, :, 1] = img[:, :, 2]  # G = R
swapped[:, :, 2] = img[:, :, 1]  # R = G

# Lưu ảnh
cv2.imwrite('my_image_swapped.jpg', swapped)
```

### Chuyển đổi không gian màu YCrCb
**Mục đích:** Tách riêng các thành phần màu sắc và độ sáng của ảnh

**Code chính**
```python
import cv2
import numpy as np

# Đọc ảnh
img = cv2.imread('my_image.jpg')

# Chuyển sang không gian màu YCrCb
ycrcb = cv2.cvtColor(img, cv2.COLOR_BGR2YCrCb)

# Tách các kênh
y, cr, cb = cv2.split(ycrcb)

# Lưu từng kênh
cv2.imwrite('my_image_Y.jpg', y)
cv2.imwrite('my_image_Cr.jpg', cr)
cv2.imwrite('my_image_Cb.jpg', cb)
```

## 2. CÂU 2

### Yêu cầu
Viết một chương trình Python sử dụng OpenCV để tạo menu tương tác cho phép người dùng chọn các kỹ thuật biến đổi hình học và xử lý ảnh nâng cao từ một danh sách, áp dụng đồng thời cho nhiều ảnh.

### Menu bao gồm:
1. **Biến đổi hình học:**
   - Xoay ảnh (0.5 điểm)
   - Thay đổi kích thước (0.5 điểm)
   - Cắt ảnh (crop ngẫu nhiên vùng giữa ảnh) (0.5 điểm)
   - Lật ảnh (0.5 điểm)
   - Thêm viền (padding màu ngẫu nhiên) (0.5 điểm)

2. **Xử lý ảnh nâng cao:**
   - Làm mịn ảnh (Gaussian, Median, Bilateral)
   - Phát hiện biên (Canny, Sobel, Laplacian)
   - Làm sắc nét ảnh
   - Thay đổi độ tương phản và độ sáng

### Các hàm chính
```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

def zoom_image(img):
    # Thay đổi kích thước ảnh

def rotate_image(img):
    # Xoay ảnh

def lat_ngang(img):
    # Lật ảnh theo chiều ngang

def lat_doc(img):
    # Lật ảnh theo chiều dọc


def crop(img):
    # Cắt ảnh ngẫu nhiên vùng giữa

def padding(img):
    # Thêm viền màu ngẫu nhiên
```

### Lưu kết quả
- Lưu ảnh với tên dạng: `result_[method]_[image_name].jpg`
- Ví dụ: `result_crop_cat.jpg`, `result_rotate_image1.jpg`

## 3. CÂU 3 - XỬ LÝ 3 ẢNH (4 điểm)

### Yêu cầu
Viết một chương trình Python để xử lý 3 ảnh bất kỳ do sinh viên tự chọn với các yêu cầu:

1. **Ảnh thứ nhất:** Thêm viền đen (border) cho ảnh
2. **Ảnh thứ hai:** Chuyển sang ảnh grayscale
3. **Ảnh thứ ba:** Tăng kích thước lên 4 lần và áp dụng Bilateral Filter

### Code chính
```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

# Đọc 3 ảnh
img1 = cv2.imread('image1.jpg')
img2 = cv2.imread('image2.jpg')
img3 = cv2.imread('image3.jpg')

# Xử lý ảnh thứ nhất - thêm viền đen
bordered_img1 = cv2.copyMakeBorder(img1, 20, 20, 20, 20, cv2.BORDER_CONSTANT, (0, 0, 0))

# Xử lý ảnh thứ hai - chuyển sang grayscale
gray2 = cv2.cvtColor(img2, cv2.COLOR_BGR2GRAY)

# Xử lý ảnh thứ ba - tăng kích thước và Bilateral Filter
# Tăng kích thước lên 4 lần
img3 = cv2.resize(img3, None, fx=4, fy=4)
# Áp dụng Bilateral Filter
img3 = cv2.bilateralFilter(img3, 15, 75, 75)

# Hiển thị kết quả
plt.imshow(bordered_img1)
plt.show()

plt.imshow(zoomed2)
plt.title("Xoay ảnh thứ 2 45 độ và phóng to 1.5 lần")
plt.show()

plt.figure()
plt.imshow(img3)
plt.show()

```

## CÀI ĐẶT THƯ VIỆN
```python
pip install opencv-python numpy matplotlib pillow scipy
```

## HƯỚNG DẪN CHẠY
1. **Cài đặt các thư viện cần thiết**
2. **Chuẩn bị ảnh đầu vào:**
   - Câu 1: Đặt tên `my_image.jpg`
   - Câu 2: Chuẩn bị nhiều ảnh để test menu
   - Câu 3: Chuẩn bị 3 ảnh `image1.jpg`, `image2.jpg`, `image3.jpg`
3. **Chạy từng cell trong Jupyter Notebook theo thứ tự:**
   - Câu 1: Xử lý ảnh cơ bản
   - Câu 2: Menu tương tác
   - Câu 3: Xử lý 3 ảnh
4. **Kiểm tra kết quả và ảnh đầu ra**

## TÀI LIỆU THAM KHẢO
- [OpenCV Documentation](https://docs.opencv.org/)
- [Bilateral Filter - OpenCV](https://docs.opencv.org/4.x/d4/d86/group__imgproc__filter.html#ga9d7064d478c95d60003cf839430737a)
- [Canny Edge Detection](https://docs.opencv.org/4.x/da/d22/tutorial_py_canny.html)
- [Color Space Conversions](https://docs.opencv.org/4.x/d8/d01/group__imgproc__color__conversions.html)
- [Geometric Transformations](https://docs.opencv.org/4.x/da/d54/group__imgproc__transform.html)