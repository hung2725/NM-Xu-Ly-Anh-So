# TTHỰC HÀNH 2: ẢNH KỸ THUẬT SỐ VÀ MÀU

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

### 2. BÀI TẬP
#### Bài 1: Viết chương trình tạo menu cho phép người dùng chọn các phương pháp biến đổi ảnh như sau:
- Image inverse transformation
- Gamma-Correction
- Log Transformation
- Histogram equalization
- Contrast Stretching
- Khi người dùng ấn phím I, G, L, H, C thì chương trình sẽ thực hiện hàm tương ứng cho các hình trong thư mục exercise. Lưu và hiển thị các ảnh đã biến đổi.

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

#### Bài 2: Viết chương trình tạo menu cho phép người dùng chọn các phương pháp biến đổi ảnh như sau:
- Fast Fourier
- Butterworth Lowpass Filter
- Butterworth Hightpass Filter
- Khi người dùng ấn phím F, L, H thì chương trình sẽ được thực hiện trong mục exercise. Lưu và hiển thị các ảnh đã biến đổi

### CÔNG NGHỆ SỬ DỤNG
Sử dụng các thư viện sau:
**PIL**: Xử lý ảnh
**numpy**: tính toán ma trận
**scipy**: Biến đổi Fourier nhanh (FFT)
**Matplotlib**: Hiển thị ảnh và kết quả
**os**: Quản lý file và thư mục
**math**: Tính toán cơ bản

### THUẬT TOÁN SỬ DỤNG
##### 1. Fast Fourier Transform (FFT)
- **Biến đổi Fourier 2D**: Chuyển ảnh từ miền không gian sang miền tần số
- **FFT Shift**: Dịch chuyển tần số 0 về trung tâm
- **Magnitude Spectrum**: Tính độ lớn của phổ tần số

##### 2. Butterworth Low-pass Filter
- **D(u,v)**: Khoảng cách từ điểm (u,v) đến trung tâm
- **D₀**: Tần số cắt (cutoff frequency)
- **n**: Bậc của bộ lọc

##### 3. Butterworth High-pass Filter
- **Ngược với Low-pass**: Cho phép tần số cao qua, chặn tần số thấp

### GIẢI THÍCH HOẠT ĐỘNG

```python
def Fast_Fourier(img):
    fft_result = abs(scipy.fftpack.fft2(img))
    chuyen = scipy.fftpack.fftshift(fft_result)
    return chuyen
```
* Thực hiện FFT 2D để chuyển ảnh từ miền không gian sang miền tần số
* Tính magnitude spectrum (độ lớn phổ tần số)
* Dịch chuyển tần số thấp về trung tâm để dễ quan sát
* Trả về phổ tần số của ảnh

```python
def Butterworth_Lowpass(img, r0=30.0, t=1):
    M, N = img.shape
    H = np.ones((M, N))
    center1, center2 = M / 2, N / 2
    for i in range(M):
        for j in range(N):
            r = math.sqrt((i - center1) ** 2 + (j - center2) ** 2)
            if r > 0:
                H[i, j] = 1 / (1 + (r / r0) ** (2 * t))
    return apply_filter(img, H)
```
* Khởi tạo bộ lọc Butterworth thông thấp với kích thước bằng ảnh
* Tính toán tâm của ảnh để làm gốc tọa độ
* Duyệt qua từng pixel để tính khoảng cách đến tâm
* Áp dụng công thức Butterworth để tạo độ mượt cho bộ lọc
* Giá trị bộ lọc gần tâm = 1 (cho qua), xa tâm = 0 (chặn)
* Gọi hàm apply_filter để áp dụng bộ lọc lên ảnh

```python
def Butterworth_Highpass(img, r0=30.0, t=1):
    M, N = img.shape
    H = np.zeros((M, N))
    center1, center2 = M / 2, N / 2
    for i in range(M):
        for j in range(N):
            r = math.sqrt((i - center1) ** 2 + (j - center2) ** 2)
            if r > 0:
                H[i, j] = 1 / (1 + (r0 / r) ** (2 * t))
    return apply_filter(img, H)
```
* Khởi tạo bộ lọc Butterworth thông cao (khởi tạo bằng 0)
* Sử dụng công thức nghịch đảo của bộ lọc thông thấp
* Giá trị bộ lọc gần tâm = 0 (chặn), xa tâm = 1 (cho qua)
* Loại bỏ tần số thấp (nền) và giữ lại tần số cao (chi tiết)

```python
def apply_filter(img, H):
    fft = scipy.fftpack.fft2(img)
    fft_shift = scipy.fftpack.fftshift(fft)
    filtered = fft_shift * H
    inverse_fft = abs(scipy.fftpack.ifft2(filtered))
    return inverse_fft
```
* Thực hiện FFT 2D để chuyển ảnh sang miền tần số
* Dịch chuyển tần số 0 về trung tâm
* Nhân ảnh tần số với bộ lọc H (áp dụng lọc)
* Thực hiện IFFT để chuyển về miền không gian
* Trả về ảnh đã được lọc

```python
transform_map = {
    'F': (Fast_Fourier, 'Fast Fourier'),
    'L': (Butterworth_Lowpass, 'Butterworth Lowpass'),
    'H': (Butterworth_Highpass, 'Butterworth Highpass'),
}
```
* Tạo dictionary ánh xạ ký tự với hàm và tên tương ứng
* F: Fast Fourier Transform
* L: Bộ lọc Butterworth thông thấp
* H: Bộ lọc Butterworth thông cao

```python
print("F: Fast Fourier")
print("L: Butterworth Lowpass")
print("H: Butterworth Highpass")
choice = input("Nhập lựa chọn (F, L, H): ").strip().upper()
```
* Hiển thị menu các lựa chọn cho người dùng
* Nhận input từ người dùng
* Loại bỏ khoảng trắng và chuyển thành chữ hoa

```python
if choice in transform_map:
    func, name = transform_map[choice]
    apply_transformation(func, "exercise", "result", name)
else:
    print("Lựa chọn không hợp lệ.")
```
* Kiểm tra lựa chọn của người dùng có hợp lệ không
* Lấy hàm và tên tương ứng từ dictionary
* Gọi hàm apply_transformation để xử lý tất cả ảnh
* Xử lý ảnh từ thư mục "exercise" và lưu vào "result"
* Thông báo lỗi nếu lựa chọn không hợp lệ


#### BÀi 3: 3. Viết chương trình thay đổi màu thứ tự màu RGB của ảnh trong thư mục excercise và sử dụng ngẫu nhiên một trong các phép biến đổi ảnh trong câu 1. Lưu và hiển thị ảnh đã biến đổi.


## CÔNG NGHỆ SỬ DỤNG
các thư viện sử dụng cũng giống như bài 2 và thêm thư viện sau:
- **Random** - Tạo ngẫu nhiên

## THUẬT TOÁN SỬ DỤNG

### 1. Median Filter
- **Lọc nhiễu**: Áp dụng bộ lọc median 3x3 cho từng kênh RGB
- **Giảm nhiễu**: Loại bỏ nhiễu muối tiêu

### 2. Image Inverse Transformation
- **Công thức**: pixel_mới = 255 - pixel_cũ
- **Hiệu ứng**: Đảo ngược màu sắc (âm bản)

### 3. Gamma Correction
- **Công thức**: pixel_mới = ((pixel + 1) / max)^γ × 255
- **Gamma mặc định**: 0.5 (làm sáng ảnh)

### 4. Log Transformation
- **Công thức**: pixel_mới = (128 × log(1 + pixel)) / log(1 + max)
- **Hiệu ứng**: Tăng cường vùng tối

### 5. Histogram Equalization
- **Cân bằng histogram**: Phân bố lại cường độ pixel
- **CDF**: Sử dụng Cumulative Distribution Function

### 6. Contrast Stretching
- **Công thức**: pixel_mới = 255 × (pixel - min) / (max - min)
- **Hiệu ứng**: Kéo giãn độ tương phản toàn dải

## GIẢI THÍCH HOẠT ĐỘNG

```python
def img_inv(img):
    im1 = np.asarray(img)
    im2 = 255 - im1
    return Image.fromarray(im2.astype(np.uint8))
```
* Chuyển ảnh PIL thành mảng NumPy
* Thực hiện phép đảo ngược: 255 - giá trị pixel
* Chuyển kết quả về kiểu uint8 và trả về ảnh PIL

```python
def gamma_corr(img, gamma=0.5):
    a = np.asarray(img).astype(float)
    m = np.max(a)
    a = (a + 1) / m
    a = np.exp(np.log(a) * gamma) * 255
    return Image.fromarray(a.astype(np.uint8))
```
* Chuyển ảnh thành float để tính toán chính xác
* Chuẩn hóa ảnh về khoảng (0,1) bằng cách chia cho giá trị max
* Áp dụng công thức gamma: a^γ = exp(log(a) × γ)
* Nhân với 255 để trở về khoảng (0,255)

```python
def log_transform(img):
    a = np.asarray(img).astype(float)
    m = np.max(a)
    a = (128 * np.log(1 + a)) / np.log(1 + m)
    return Image.fromarray(a.astype(np.uint8))
```
* Áp dụng biến đổi logarithm với hệ số 128
* Sử dụng log(1 + pixel) để tránh log(0)
* Chuẩn hóa bằng log(1 + max) để giá trị không vượt quá 255

```python
def hist_equa(img):
    a = np.asarray(img)
    flat = a.flatten()
    hist, _ = np.histogram(flat, 256, [0, 256])
    cdf = hist.cumsum()
    cdf_m = np.ma.masked_equal(cdf, 0)
    cdf = (cdf_m - cdf_m.min()) * 255 / (cdf_m.max() - cdf_m.min())
    cdf = np.ma.filled(cdf, 0).astype('uint8')
    eq = cdf[flat].reshape(a.shape)
    return Image.fromarray(eq)
```
* Làm phẳng ảnh thành mảng 1D
* Tính histogram với 256 bins cho các giá trị 0-255
* Tính CDF (Cumulative Distribution Function)
* Loại bỏ các giá trị CDF = 0 bằng mask
* Chuẩn hóa CDF về khoảng 0-255
* Áp dụng CDF làm bảng lookup để cân bằng histogram

```python
def contrast_stretch(img):
    a = np.asarray(img).astype(float)
    stretched = 255 * (a - a.min()) / (a.max() - a.min())
    return Image.fromarray(stretched.astype(np.uint8))
```
* Tính giá trị min và max của ảnh
* Áp dụng công thức kéo giãn tuyến tính
* Kéo giãn từ khoảng [min, max] về [0, 255]

```python
transformations = {
    'I': ('Image Inverse', img_inv),
    'G': ('Gamma Correction', gamma_corr),
    'L': ('Log Transformation', log_transform),
    'H': ('Histogram Equalization', hist_equa),
    'C': ('Contrast Stretching', contrast_stretch)
}
```
* Tạo dictionary ánh xạ ký tự với tên và hàm tương ứng
* Mỗi entry chứa tuple (tên_hiển_thị, hàm_xử_lý)
* Dùng để chọn ngẫu nhiên phép biến đổi

```python
a = np.asarray(img)
r = sn.median_filter(a[:, :, 0], size=(3, 3))
g = sn.median_filter(a[:, :, 1], size=(3, 3))
b = sn.median_filter(a[:, :, 2], size=(3, 3))

channels = [r, g, b]
random.shuffle(channels)
rgb_random_img = np.stack(channels, axis=2).astype(np.uint8)
```
* Tách ảnh RGB thành 3 kênh màu riêng biệt
* Áp dụng median filter 3x3 cho từng kênh để giảm nhiễu
* Trộn ngẫu nhiên thứ tự các kênh màu (R-G-B → ngẫu nhiên)
* Ghép lại thành ảnh RGB với thứ tự kênh đã thay đổi

```python
key = random.choice(list(transformations.keys()))
name, transform = transformations[key]
transformed_img = transform(rgb_random_pil)
```
* Chọn ngẫu nhiên một key từ dictionary transformations
* Lấy tên và hàm biến đổi tương ứng
* Áp dụng phép biến đổi lên ảnh RGB đã trộn kênh

```python
base_name = path.stem
output_path = input_folder / f'{base_name}_rgb_random_{key}.jpg'
transformed_img.save(str(output_path))
```
* Lấy tên file gốc không có extension
* Tạo tên file mới với định dạng: tên_gốc_rgb_random_ký_tự.jpg
* Lưu ảnh đã xử lý vào cùng thư mục với tên mới

```python
for path in files:
    process_image(path)
```
* Duyệt qua tất cả file ảnh trong thư mục exercise
* Xử lý từng ảnh một cách tự động
* Mỗi ảnh sẽ được áp dụng ngẫu nhiên một phép biến đổi khác nhau


#### Bài 4:  Viết chương trình thay đổi thứ tự màu RGB của ảnh trong thư mục exercise và sử dụng ngẫu nhiên một trong các phép biến đổi ảnh trong câu 2. Nếu ngẫu nhiên là phép Butterworth Lowpass thì chọn thêm Min Filter để lọc ảnh. Nếu ngẫu nhiên là phép Butterworth Highpass thì chọn thêm Max Filter để lọc ảnh. Lưu và hiển thị ảnh đã biến đổi.

## CÔNG NGHỆ SỬ DỤNG
Các thư viện dùng giống như bài 2 và bài 3
## THUẬT TOÁN SỬ DỤNG

### 1. Random Channel Shuffle
- **Trộn kênh màu**: Shuffle ngẫu nhiên thứ tự các kênh R, G, B
- **Hiệu ứng**: Tạo màu sắc bất thường, nghệ thuật

### 2. Fast Fourier Transform (FFT)
- **Biến đổi Fourier**: Chuyển ảnh từ miền không gian sang miền tần số
- **FFT Shift**: Dịch chuyển tần số 0 về tâm

### 3. Butterworth Filter
- **Tần số cắt**: D0 = 30.0
- **Bậc filter**: n = 1

### 4. Morphological Filter
- **Minimum Filter**: Áp dụng cho Low-pass (làm mờ thêm)
- **Maximum Filter**: Áp dụng cho High-pass (tăng cường cạnh)

## GIẢI THÍCH HOẠT ĐỘNG

```python
img_rgb = Image.open(path).convert('RGB')
r, g, b = img_rgb.split()
channels = [r, g, b]
random.shuffle(channels)
shuffled_img = Image.merge('RGB', channels)
```
* Mở ảnh và chuyển về RGB
* Tách ảnh thành 3 kênh màu riêng biệt (R, G, B)
* Shuffle ngẫu nhiên thứ tự các kênh trong list
* Ghép lại thành ảnh RGB với thứ tự kênh đã thay đổi

```python
img = shuffled_img.convert('L')
im1 = np.asarray(img)
c = abs(scipy.fft.fft2(im1))
d = scipy.fft.fftshift(c)
```
* Chuyển ảnh RGB đã shuffle thành grayscale
* Convert ảnh PIL thành mảng NumPy
* Thực hiện FFT 2D để chuyển sang miền tần số
* Dịch chuyển tần số 0 về tâm ảnh bằng fftshift

```python
M, N = d.shape
H = np.ones((M, N))
center1, center2 = M / 2, N / 2
r0 = 30.0
t = 1
t2 = 2 * t
choice = random.choice(['low', 'high'])
```
* Lấy kích thước ảnh tần số
* Tạo mask filter ban đầu với giá trị 1
* Xác định tâm ảnh tần số
* Thiết lập tham số: tần số cắt r0=30, bậc t=1
* Chọn ngẫu nhiên loại filter (low-pass hoặc high-pass)

```python
for i in range(M):
    for j in range(N):
        r1 = (i - center1) ** 2 + (j - center2) ** 2
        r = math.sqrt(r1)
        if r == 0:
            H[i, j] = 1 if choice == 'low' else 0 
        else:
            if choice == 'low':
                H[i, j] = 1 / (1 + (r / r0) ** t2)
            elif choice == 'high':
                H[i, j] = 1 / (1 + (r0 / r) ** t2)
```
* Duyệt qua từng pixel trong mask filter
* Tính khoảng cách Euclidean từ tâm đến pixel hiện tại
* Xử lý trường hợp đặc biệt tại tâm (r=0)
* Áp dụng công thức Butterworth cho low-pass hoặc high-pass
* Low-pass: giá trị giảm khi khoảng cách tăng
* High-pass: giá trị tăng khi khoảng cách tăng

```python
H1 = H.astype(float)
con = d * H1
e = abs(scipy.fft.ifft2(con))
```
* Chuyển mask về kiểu float để tính toán
* Nhân ảnh tần số với mask filter (convolution trong miền tần số)
* Thực hiện IFFT để chuyển về miền không gian
* Lấy giá trị tuyệt đối để có ảnh thực

```python
if choice == 'low':
    e = sn.minimum_filter(e, size=3)
elif choice == 'high':
    e = sn.maximum_filter(e, size=3)
```
* Áp dụng morphological filter bổ sung
* Low-pass: dùng minimum filter để làm mờ thêm
* High-pass: dùng maximum filter để tăng cường cạnh
* Kernel 3x3 cho cả hai loại filter

```python
e = np.clip(e, 0, 255)
e = e.astype(np.uint8)
im3 = Image.fromarray(e)
```
* Giới hạn giá trị pixel trong khoảng [0, 255]
* Chuyển về kiểu uint8 để lưu ảnh
* Convert mảng NumPy về ảnh PIL

```python
plt.imshow(im3, cmap='gray')
plt.title(f"{filename} - Butterworth {choice.capitalize()}pass + {'Min' if choice=='low' else 'Max'} Filter")
plt.savefig(os.path.join(output_folder, f"{os.path.splitext(filename)[0]}_plot.jpg"))
plt.show()
```
* Hiển thị ảnh kết quả với colormap grayscale
* Tạo tiêu đề mô tả loại filter đã áp dụng
* Lưu biểu đồ dưới dạng file JPG
* Hiển thị ảnh trên màn hình

```python
output_name = f"{os.path.splitext(filename)[0]}_Butterworth_{choice}.jpg"
im3.save(os.path.join(output_folder, output_name))
```
* Tạo tên file đầu ra với định dạng: tên_gốc_Butterworth_loại.jpg
* Lưu ảnh đã xử lý vào thư mục result
* File được lưu ở định dạng JPG