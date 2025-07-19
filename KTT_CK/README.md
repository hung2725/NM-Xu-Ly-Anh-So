# Nhập Môn Xử Lý Ảnh Số - THI THỬ CUỐI KÌ

## XÁC ĐỊNH ĐỐI TƯỢNG TRONG ẢNH
- **Sinh Viên Thực Hiên:** Phạm Thế Hùng
- **MSSV:** 2374802010164
- **Môn Học:** Nhập Môn Xử Lý Ảnh Số
- **Giảng viên:** Đỗ Hữu Quân

## Câu 1: Cho ảnh có tên là a.jpg và thực hiện các yêu cầu:

## Công nghệ sử dụng
- **Python:** Ngôn ngữ chính
- **numpy:** Xử lý ảnh dưới dạng mảng số học
- **scipy:**  giải quyết các bài toán kỹ thuật và toán học phức tạp
- **skimage:** xử lý và phân tích hình ảnh
- **imageio:** Đọc file ảnh với định dạng hiện đại
- **matplotlib:** Hiển thị ảnh trực quan
- **random:** Tạo ra các giá trị ngẫu nhiên 

### Mean filter
**Mục đích:** Mean Filter là một kỹ thuật lọc ảnh tuyến tính được sử dụng phổ biến trong xử lý ảnh số. Mục tiêu chính của nó là làm mờ ảnh để giảm nhiễu và làm mịn các chi tiết nhỏ. Giảm nhiễu, làm mịn ảnh, làm mờ các chi tiết sắc nét
**Nguyên Lý:** Mean Filter hoạt động bằng cách thay thế giá trị của mỗi điểm ảnh bằng giá trị trung bình của các điểm ảnh xung quanh nó trong một vùng cửa sổ kernel, thường là 3×3, 5×5,...

**Code chính:**
```python
a = iio.imread('a.jpg',  mode = "F")
k = np.ones((5, 5)) / 25
b = sn.convolve(a, k).astype(np.uint8)
```
### Ở đây em dùng sobel filter xác định biên của hình ảnh trên

**Mục đích:** Sobel filter được sử dụng phổ biến trong xử lý ảnh để phát hiện biên. Nó giúp làm nổi bật những vùng có sự thay đổi cường độ sáng mạnh, như đường viền góc cạnh những đặc điểm rất quan trọng trong việc nhận diện đối tượng, phân vùng ảnh hay dẫn hướng trong thị giác máy tính

**Nguyên Lý:** Bộ lọc Sobel dựa trên phương pháp tính đạo hàm của cường độ ảnh theo hai hướng:
- X: phát hiện biên theo chiều dọc.
- Y: phát hiện biên theo chiều ngang.

**Code chính:**
```python
c = filters.sobel(b)
c = (c * 255).astype(np.uint8)

```
### Chuyển đổi BGR sang RGB
**Mục đích:** Trong xử lý ảnh kỹ thuật số, nhiều thư viện như OpenCV sử dụng định dạng màu BGR, trong khi phần lớn các hệ thống hiển thị và xử lý ảnh sử dụng RGB. Việc chuyển đổi là cần thiết để:
- Hiển thị màu sắc chính xác trên các nền tảng khác nhau
- Tránh sai lệch màu khi trích xuất hoặc trực quan hóa ảnh
- Chuẩn hóa dữ liệu màu đầu vào trong các ứng dụng học sâu hoặc xử lý ảnh nâng cao


**Nguyên Lý:** Việc chuyển đổi chỉ đơn giản là hoán đổi vị trí kênh màu, vì cả hai đều dùng hệ thống 3 kênh nhưng theo thứ tự khác:
- BGR có thứ tự: Blue (Xanh dương) – Green (Xanh lá) – Red (Đỏ)
- RGB có thứ tự: Red – Green – Blue


**Code chính:**
```python
data = iio.imread("a.jpg")
kenh_mau = [0, 1, 2]
random.shuffle(kenh_mau)
bdata = data[:, :, kenh_mau]
iio.imsave("a_random_color.jpg", bdata )
```

### Chuyển ảnh sang không gian màu HSV và tách riêng kênh Hue, Saturation, Value

**Mục đích:** Việc chuyển ảnh từ hệ màu RGB/BGR sang HSV là một bước quan trọng trong xử lý ảnh vì HSV tách biệt rõ ràng các yếu tố của màu sắc, cho phép xử lý chúng dễ dàng hơn trong các tác vụ phân tích màu sắc, phân đoạn ảnh, cải thiện tương phản và ánh sáng, phân loại và nhận dạng đối tượng dựa trên sắc độ hoặc độ bão hòa

**Nguyên Lý:** HSV là hệ màu biểu diễn theo trực quan của con người:
- Hue (Sắc độ): góc quay trên vòng màu 0–360 độ thể hiện màu cơ bản
- Saturation (Độ bão hòa): mức độ rực rỡ của màu 0 = màu xám, 255 = màu đặm
- Value (Độ sáng): độ sáng của màu 0 = đen, 255 = sáng nhất

**Code chính:**
```python
rgb = iio.imread("a.jpg")
rgb2hsv = np.vectorize(colorsys.rgb_to_hsv)
h,s,v = rgb2hsv(rgb[:,:,0], rgb[:,:,1], rgb[:,:,2])
h_img = (h * 255).astype(np.uint8)
s_img = (s * 255).astype(np.uint8)
v_img = (v * 255).astype(np.uint8)
iio.imwrite("a_hue.jpg", h_img)
iio.imwrite("a_saturation.jpg", s_img)
iio.imwrite("a_value.jpg", v_img)
```

## Câu 2: Viết một chương trình Python sử dụng OpenCV để tạo menu động cho phép người dùng chọn các phương pháp biến đổi ảnh từ một danh sách mở rộng, áp dụng cho nhiều ảnh cùng lúc, và thực hiện các phân tích bổ sung. Các yêu cầu cụ thể:

Ở bài này thì có 2 yêu cầu:
- 1: là chọn 1 trong 6 phương pháp xử lý ảnh và xử lý cùng lúc 3 ảnh
- 2: cũng như yêu cầu khác hơn chút là gán các lựa chọn bằng các chữ cái đầu của các phương pháp xử lý ảnh và lưu ảnh dưới dạng output_inverse_1.jpg, output_gamma_2.jpg,...
### Image inverse transformation
**Mục đích:** Image Inverse Transformation là một bước khôi phục hoặc đảo ngược quá trình xử lý đã áp dụng lên ảnh. Mục đích chính là khôi phục dữ liệu gốc,hiệu chỉnh sai lệch nếu ảnh bị biến dạng do hệ thống camera hay ánh sáng, biến đổi ngược có thể giúp đưa ảnh về trạng thái cân bằng.

**Nguyên Lý:** Biến đổi ảnh thường dùng ma trận biến đổi tuyến tính như biến đổi affine hoặc đồng nhất. Khi bạn đã áp dụng biến đổi T lên ảnh đầu vào để đảo ngược bạn sẽ dùng ma trận nghịch đảo của T, ký hiệu là T^-1.

**Code chính:**
```python
img = Image.open(name).convert("L")
im_1 = np.asarray(img)
img_2 = 255 - im_1
new_img = Image.fromarray(img_2)
```



### Gamma-Correction
**Mục đích:** Gamma Correction là một kỹ thuật quan trọng trong xử lý ảnh và hiển thị hình ảnh nhằm điều chỉnh độ sáng phi tuyến để phù hợp với cách mắt người cảm nhận ánh sáng

**Nguyên Lý:** Gamma Correction áp dụng hàm phi tuyến dạng mũ để biến đổi cường độ pixel theo công thức:

![alt text](image.png)

**Code chính:**
```python
img = Image.open(name).convert("L")
im_3 = np.asarray(img)
gamma = 0.5
b1 = im_3.astype(float)
b2 = np.max(b1)
b3 = b1/b2
c = (np.exp(b3) * 255.0).astype(np.uint8) 
d = Image.fromarray(c)
```

### Log Transformation
**Mục đích:** Biến đổi log là một kỹ thuật phổ biến trong xử lý ảnh nhằm:
- Tăng cường vùng tối của ảnh làm nổi bật các chi tiết trong vùng có cường độ sáng thấp
- Giảm ảnh hưởng của vùng sáng cao làm mềm lại những vùng quá sáng hoặc có cường độ cao.
- huẩn hóa dải động ảnh đặc biệt hiệu quả với các ảnh có độ tương phản cao như ảnh chụp x-quang

**Nguyên Lý:** Log Transformation áp dụng hàm logarit lên giá trị cường độ pixel để nén giá trị lớn và làm nổi bật giá trị nhỏ. Công thức tổng quát:

                    s = c.log(1 + r)

- `s`: giá trị pixel sau biến đổi (đầu ra).
- `r`: giá trị pixel đầu vào (gốc).
- `c`: hằng số tỉ lệ, dùng để scale giá trị `s` về khoảng [0, 255].

**Code chính:**
```python
img = Image.open(name).convert("L")
im_4 = np.asarray(img)
gamma = 0.5
b = im_4.astype(float)
b_1 = np.max(b)
c1 = (128.0 * np.log(1 + b)) / np.log(1 + b_1)
c2 = np.clip(c1, 0, 255).astype(np.uint8)
d1 = Image.fromarray(c2)
```
### Histogram Equalization

**Mục đích:** Histogram Equalization là một kỹ thuật dùng để nâng cao độ tương phản của ảnh, đặc biệt là những ảnh có độ tương phản thấp hoặc các chi tiết bị chìm trong vùng sáng hoặc tối

**Nguyên Lý:** Histogram Equalization hoạt động dựa trên ý tưởng phân phối lại các giá trị cường độ pixel sao cho biểu đồ histogram của ảnh trải đều hơn trên toàn phạm vi 0–255

**Code chính:**
```python
img = Image.open(name).convert('L')
im1 = np.asarray(img)
b1 = im1.flatten()
hist, bins = np.histogram(im1, 256, [0, 255])
cdf = hist.cumsum()
cdf_m = np.ma.masked_equal(cdf, 0)
num_cdf = (cdf_m - cdf_m.min()) * 255
den_cdf = (cdf_m.max() - cdf_m.min())
cdf = num_cdf / den_cdf
cdf = np.ma.filled(cdf, 0).astype('uint8')
im2 = cdf[b1]
im3 = np.reshape(im2, im1.shape)
im4 = Image.fromarray(im3)
```

### Contrast Stretching
**Mục đích:** Contrast Stretching là kỹ thuật xử lý ảnh nhằm tăng cường độ tương phản tổng thể của hình ảnh, đặc biệt với những ảnh có mức xám tập trung vào vùng trung tính, khiến ảnh trông mờ nhạt hoặc thiếu chi tiết

**Nguyên Lý:** Contrast Stretching kéo giãn giá trị cường độ pixel từ một khoảng hẹp về đầy đủ phạm vi 0–255. Nó thường sử dụng phương pháp tuyến tính hoặc phi tuyến để ánh xạ lại giá trị pixel

**Code chính:**
```python
img = Image.open(name).convert('L')
im1 = np.asarray(img)
b = im1.max()
a = im1.min()
print(a,b)
c = im1.astype(float)
im2 = 255 * (c - a) / (b - a)
im3 = Image.fromarray(im2.astype(np.uint8))
```




## Câu 3: Viết một chương trình Python sử dụng OpenCV để xử lý ba ảnh: colorful-ripe-tropical-fruits.jpg, quang-ninh.jpg, và pagoda.jpg với các phương pháp biến đổi và tiền xử lý nâng cao.


### Tăng kichs thước ảnh

**Thuật toán sử dụng** Ở câu này sử dụng thuật toán nd.zoom để phóng to ảnh có trong thư viện scipy.ndimage để phóng to chiều rộng và chiều cao mỗi chiều thêm 30 px

**Code chính:**
```python
data = iio.imread("colorful-ripe-tropical-fruits.jpg")
print(data.shape)
bdata = nd.zoom(data, (1.0212, 1.0142, 1))
print(bdata.shape)
```

### Xoay ảnh
**Thuật toán sử dụng** Ở bài này sử dụng thuật toán nd.rotate xoay ảnh có trong thư viện scipy.ndimage để xoay ảnh theo chiều kim đồng hồ với góc 45 độ và lật ngang ảnh bằng np.fliplr có trong thư viện numpy

**Code chính:**
```python
data = iio.imread("quang_ninh.jpg", mode = "F")
print(data.shape)
d1 = nd.rotate(data, 45)
d2 = np.fliplr(nd.rotate(data, 45, reshape=False))
```

## phóng to ảnh và áp dụng gaussian blur với kernel 7x7
**Thuật toán sử dụng** Ở câu này sử dụng thuật toán nd.zoom để phóng to ảnh có trong thư viện scipy.ndimage để phóng to ảnh gốc gấp 5 lần và sử dụng gaussian blur với kernel 7x7

**Code chính:**
```python
data = iio.imread("pagoda.jpg")
print("Kích thước Pagoda gốc:", data.shape)
z_data = nd.zoom(data, (5, 5, 1))
z_data = nd.gaussian_filter(z_data, sigma=(1.5, 1.5, 0))
print("Kích thước Pagoda sau khi phóng to:", z_data.shape)
```

## CÀI ĐẶT THƯ VIỆN
```python
pip install pillow numpy matplotlib imageio opencv-python
```

## TÀI LIỆU THAM KHẢO
- [Phép toán với điểm - Điểu chỉnh độ tương phản - viblo.asia](https://viblo.asia/p/tuan-2-phep-toan-voi-diem-dieu-chinh-do-tuong-phan-V3m5WjpblO7)

- [Histogram - Histogram equalization - viblo.asia](https://viblo.asia/p/tuan-3-histogram-histogram-equalization-3P0lPnxmKox)