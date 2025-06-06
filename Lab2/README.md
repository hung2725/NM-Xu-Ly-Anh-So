# Chương trình Xử lý Điểm Ảnh

## CÔNG NGHỆ SỬ DỤNG

### Gồm các thư viện:
- **PIL**: Mở và xử lý ảnh
- **numPy**: Xử lý toán học với mảng 
- **Matplotlib.pyplot**: Hiển thị ảnh và biểu đồ
- **ImageIO**: Đọc và ghi ảnh
- **SciPy**: Tính toán khoa học

## THUẬT TOÁN SỬ DỤNG

### 1.1 Nghịch đảo ảnh
Thuật toán nghịch đảo màu đen trắng của ảnh, tạo hiệu ứng âm bản.

### 1.2 Hiệu chỉnh Gamma
Thuật toán điều chỉnh độ sáng dựa trên luật lũy thừa với tham số gamma.

### 1.3. Biến đổi Logarit
Thuật toán biến đổi logarit để tăng cường chi tiết vùng tối.

### 1.4. Cân bằng Histogram
Thuật toán cân bằng phân phối histogram để cải thiện tương phản.

### 1.5. Kéo giãn tương phản
Thuật toán mở rộng dải giá trị pixel để tăng tương phản.

### 1.6.1 Biến đổi Fourier nhanh
Thuật toán chuyển ảnh từ miền không gian sang miền tần số.

### 1.6.2 Bộ lọc Butterworth
Thuật toán lọc tần số cao/thấp trong miền Fourier.

## GIẢI THÍCH HOẠT ĐỘNG

### 1.1 Biến đổi cường độ ảnh (Image Inverse Transformation)

```python
from PIL import Image
import math
import scipy
import numpy as np
import imageio.v2 as iio
import matplotlib.pylab as plt
```
- Import các thư viện cần thiết cho chương trình

```python
img = Image.open('bird.png').convert('L')
im_1 = np.asarray(img)
```
* Mở ảnh 'bird.png' và chuyển sang ảnh đen trắng ở chế độ 'L'
* Chuyển ảnh sang mảng số để dễ xử lý từng pixel

```python
im_2 = 255 - im_1
print(im_2)
new_img = Image.fromarray(im_2)
img.show()
plt.imshow(new_img, cmap = 'gray')
plt.show()
```
* Tính ảnh nghịch đảo: mỗi pixel p biến thành 255 - p (thuật toán âm bản)
* In ra mảng số của ảnh sau khi được nghịch đảo
* Chuyển mảng về lại ảnh để hiển thị
* Hiển thị ảnh gốc và ảnh đã nghịch đảo sử dụng ra màn hình

### 1.2 Thay đổi chất lượng ảnh với Power Law (Gamma Correction)

```python
gamma = 0.5
b1 = im_1.astype(float)
b2 = np.max(b1)
b3 = (b1+1) / b2
```
* Thiết lập tham số gamma = 0.5
* Chuyển matrix từ số nguyên sang số thực để tính toán cho đỡ bị sai 
* Tìm giá trị lớn nhất và chuẩn hóa về khoảng [0,1]

```python
b4 = np.log(b3) * gamma
c = np.exp(b4) * 255.0
c1 = c.astype(np.uint8)
```
* Áp dụng công thức gamma correction: `output = input^gamma`
* Quy về khoảng [0,255] và chuyển về số nguyên

### 1.3 Biến đổi Log Transformation

```python
c = (128.0*np.log(1+b1)) / np.log(1+b2)
c1 = c.astype(np.uint8)
```
* Áp dụng công thức: `c = 128 * log(1+input) / log(1+max)`
* Tăng cường chi tiết vùng tối nén vùng sáng

### 1.4 Histogram Equalization

```python
b1 = im1.flatten()
hist, bins = np.histogram(im1, 256, [0, 255])
cdf = hist.cumsum()
```
* Chuyển ảnh 2D thành mảng 1D
* Tính histogram với 256 bins
* Tính hàm phân phối tích lũy (CDF)

```python
cdf_m = np.ma.masked_equal(cdf, 0)
num_cdf = (cdf_m - cdf_m.min()) * 255
den_cdf = (cdf_m.max() - cdf_m.min())
cdf = num_cdf / den_cdf
```
* Loại bỏ giá trị 0 trong CDF
* Chuẩn hóa CDF về khoảng [0,255]
* Tạo bảng ánh xạ mới cho các pixel

### 1.5 Contrast Stretching

```python
b = im1.max()
a = im1.min()
c = im1.astype(float)
im2 = 255 * (c - a) / (b - a)
```
* Tìm giá trị min và max của ảnh
* Áp dụng công thức kéo giãn: `output = 255 * (input - min) / (max - min)`
* Mở rộng dải động về toàn bộ khoảng [0,255]

### 1.6 Biến đổi Fourier

```python
c = abs(scipy.fftpack.fft2(im1))
d = scipy.fftpack.fftshift(c)
```
* Thực hiện tạo mảng 2 chiều để chuyển sang miền tần số
* Dịch chuyển tần số 0 về trung tâm ảnh

```python
H[i, j] = 1 / (1 + (r / r0)**t2)
con = d * H1
e = abs(scipy.fftpack.ifft2(con))
```
* Tạo bộ lọc Butterworth với bán kính cắt r0
* Nhân phổ tần số với bộ lọc

### 2. BÀI TẬP - Chương trình Menu

```python
def img_inv_func(img):
    return Image.fromarray(255 - np.asarray(img))

def gamma_corr_func(img, gamma=0.5):
    a = np.asarray(img).astype(float)
    m = np.max(a)
    a = (a + 1) / m
    a = np.exp(np.log(a) * gamma) * 255
    return Image.fromarray(a.astype(np.uint8))
```
* Định nghĩa các hàm xử lý ảnh độc lập
* Hàm nhận đầu vào là ảnh PIL và trả về ảnh đã xử lý
* Hàm gamma_corr_func có tham số gamma mặc định = 0.5

```python
def log_transform_func(img):
    a = np.asarray(img).astype(float)
    m = np.max(a)
    a = (128 * np.log(1 + a)) / np.log(1 + m)
    return Image.fromarray(a.astype(np.uint8))

def hist_eq_func(img):
    a = np.asarray(img)
    flat = a.flatten()
    hist, _ = np.histogram(a, 256, [0, 255])
    cdf = hist.cumsum()
    cdf_m = np.ma.masked_equal(cdf, 0)
    cdf = (cdf_m - cdf_m.min()) * 255 / (cdf_m.max() - cdf_m.min())
    cdf = np.ma.filled(cdf, 0).astype('uint8')
    eq = cdf[flat].reshape(a.shape)
    return Image.fromarray(eq)
```
* Hàm log_transform_func thực hiện biến đổi logarit
* Hàm hist_eq_func thực hiện cân bằng histogram

```python
def contrast_stretch_func(img):
    a = np.asarray(img).astype(float)
    stretched = 255 * (a - a.min()) / (a.max() - a.min())
    return Image.fromarray(stretched.astype(np.uint8))

def apply_transformation(func, folder, save_folder, suffix):
    for file in os.listdir(folder):
        if file.lower().endswith(('.png', '.jpg', '.jpeg', '.bmp')):
            path = os.path.join(folder, file)
            img = Image.open(path).convert('L')
            out = func(img)
            plt.imshow(out, cmap='gray')
            plt.title(f"{file} - {suffix}")
            plt.axis('off')
            plt.show()
            name = os.path.splitext(file)[0] + f"_{suffix}.png"
            out.save(os.path.join(save_folder, name))
```
* Hàm contrast_stretch_func thực hiện kéo giãn tương phản
* Hàm apply_transformation xử lý hàng loạt:
Duyệt qua tất cả file ảnh trong thư mục
Áp dụng hàm biến đổi được chọn
Hiển thị kết quả và lưu file với tên mới

```python
os.makedirs("result", exist_ok=True)

transform_map = {
    'I': (img_inv_func, 'inverse'),
    'G': (gamma_corr_func, 'gamma'),
    'L': (log_transform_func, 'log'),
    'H': (hist_eq_func, 'hist'),
    'C': (contrast_stretch_func, 'contrast')
}
```
* Tạo thư mục "result" để lưu nhũng hình sau khi thay đổi
* Tạo menu bằng dictionary tương ứng với những chữ cái đầu của thuật toán
```python
print("I: Image Inverse")
print("G: Gamma Correction")
print("L: Log Transformation")
print("H: Histogram Equalization")
print("C: Contrast Stretching")

choice = input("Nhập lựa chọn (I, G, L, H, C): ").upper()

if choice in transform_map:
    func, name = transform_map[choice]
    apply_transformation(func, "exercise", "result", name)
else:
    print("Lựa chọn không hợp lệ.")
```
* Hiển thị menu cho người dùng chọn 
* Chuyển chữ hoa về chữ thường
* Kiểm tra người dùng nhập đúng với những lựa chọn trong menu chưa
* Gọi hàm apply_transformation với tham số tương ứng
* Xử lý tất cả ảnh trong thư mục "exercise" và lưu vào "result"