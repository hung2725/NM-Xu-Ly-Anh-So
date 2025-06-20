# THỰC HÀNH 3: BIẾN ĐỔI HÌNH HỌC
## 1. VIẾT CHƯƠNG TRÌNH BIẾN ĐỔI ẢNH
### 1.1. Chọn đối tượng trong ảnh

### CÔNG NGHỆ SỬ DỤNG
* Gồm các thư viện:
- **numpy**: Xử lý toán học với mảng 
- **imageio**: Đọc và ghi ảnh
- **matplotlib.pyplot**: Hiển thị ảnh và biểu đồ
### THUẬT TOÁN SỬ DỤNG
- Ở bài này sử dụng thuật toán cơ bản của xử lý nảnh đó là cắt ảnh thành vùng con trong ảnh gốc 
### GIẢI THÍCH HOẠT ĐỘNG
```python
data = iio.imread("boat.jpg")
bmg = data[800:1200, 570:980]
print(data.shape)

```
- Đọc ảnh từ file fruit.jpg sau đó tìm tọa độ để cắt ảnh trong bài này là tọa độ để cắt lấy quả cam là y: 800,1200 và x: 570:980 và sau đó in ra kích thước ảnh sau khi cắt
```python
iio.imsave("orange.jpg", bmg)
plt.imshow(bmg)
plt.show()

```
- Lưu ảnh đã cắt với tên orange.jpg và hiển thị ảnh lên màn hình

### 1.2 Tịnh tiến đơn
### CÔNG NGHỆ SỬ DỤNG
* Gồm các thư viện:
- **numpy**: Xử lý toán học với mảng 
- **scipy**: Tính toán khoa học
- **imageio**: Đọc và ghi ảnh
- **matplotlib.pyplot**: Hiển thị ảnh và biểu đồ
### THUẬT TOÁN SỬ DỤNG
- Ở bài này sử dụng thuật toán tịnh tiến ảnh và dùng nd.shift để thực hiện điều đó và hàm này nằm trong thư viện cipy.ndimage
### GIẢI THÍCH HOẠT ĐỘNG
```python
data = iio.imread("fruit.jpg", mode = "F")
bdata = nd.shift(data,(100,25))
```
- Đọc ảnh từ file fruit.jpg và chuyển nó sang ảnh xám theo định dạng số thực sau đó dùng nd.shift để tịnh tiến ảnh xuống dưới 100 px và qua phải 25px

#### 1.3. Thay đổi kích thước ảnh
### CÔNG NGHỆ SỬ DỤNG
* Gồm các thư viện:
- **numpy**: Xử lý toán học với mảng 
- **scipy**: Tính toán khoa học
- **imageio**: Đọc và ghi ảnh
- **matplotlib.pyplot**: Hiển thị ảnh và biểu đồ
### THUẬT TOÁN SỬ DỤNG
- Ở bài này sử dụng thuật toán nd.zoom để phóng to thu nhỏ ảnh có trong thư viện scipy.ndimage
### GIẢI THÍCH HOẠT ĐỘNG
```python
data = iio.imread("fruit.jpg")
print(data.shape)
bdata = nd.zoom(data,2)
print(bdata.shape)
bdata2 = nd.zoom(data, (2, 2, 1))
print(bdata2.shape)
data3 = nd.zoom(data, (0.5, 0.9, 1))
```
- Đọc ảnh từ file fruit.jpg và in ra kích thước ảnh gốc sau đó phóng to ảnh gấp 2 lần và in ra kích thước ảnh gấp 2 lần sau đó nữa là cũng phóng to kích thước ảnh gấp 2 lần nhưng lần này là chỉ rõ chiều cao và chiều rộng phóng to bao lần và tham số cuối là kênh màu giữ nguyên và in ra kích thước của ảnh, data3 là thu nhu chiều cao xuống còn 0.5 và chiều rộng còn 0.9 và kênh màu vẫn giữ nguyene

### 1.4. Xoay ảnh
### CÔNG NGHỆ SỬ DỤNG
* Gồm các thư viện:
- **numpy**: Xử lý toán học với mảng 
- **scipy**: Tính toán khoa học
- **imageio**: Đọc và ghi ảnh
- **matplotlib.pyplot**: Hiển thị ảnh và biểu đồ
### THUẬT TOÁN SỬ DỤNG
- Ở bài này sử dụng thuật toán nd.rotate xoay ảnh có trong thư viện scipy.ndimage
### GIẢI THÍCH HOẠT ĐỘNG
``` python
data = iio.imread("boat.jpg", mode = "F")
print(data.shape)
d1 = nd.rotate(data, 20)
plt.imshow(d1)
plt.show()

d2 = nd.rotate(data, 20, reshape=False)
plt.imshow(d2)
plt.show()
```
- Đọc ảnh từ file fruit.jpg và chuyển nó sang ảnh xám theo định dạng số thực và in ra kích thước ảnh gốc, d1 xoay ảnh với 20 độ và thu nhỏ kích thước ảnh cho vừa khung xoay và d2 là cũng xoay với 20 độ nhưng d2 vẫn giữ nguyên kích thước của ảnh gốc với reshape=False
### 1.5. Dilation và Erosion
### CÔNG NGHỆ SỬ DỤNG
* Gồm các thư viện:
- **numpy**: Xử lý toán học với mảng 
- **scipy**: Tính toán khoa học
- **imageio**: Đọc và ghi ảnh
- **matplotlib.pyplot**: Hiển thị ảnh và biểu đồ
### THUẬT TOÁN SỬ DỤNG
- 
### GIẢI THÍCH HOẠT ĐỘNG

```python
data = iio.imread("world_cup.jpg", mode = "L")
print(data.shape)
d1 = nd.binary_dilation(data)
plt.imshow(d1)
plt.show()
d2 = nd.binary_dilation(data, iterations=3)
plt.imshow(d2)
plt.show()
```
- Đọc ảnh từ file world_cup.jpg và chuyển nó sang ảnh xám theo định dạng số thực và in ra kích thước ảnh gốc, d1 là làm giãn ảnh 1 lần còn d2 là giãn ảnh 3 lần 

### 1.6. Coordinate Mapping
### CÔNG NGHỆ SỬ DỤNG
* Gồm các thư viện:
- **numpy**: Xử lý toán học với mảng 
- **scipy**: Tính toán khoa học
- **imageio**: Đọc và ghi ảnh
- **matplotlib.pyplot**: Hiển thị ảnh và biểu đồ
### THUẬT TOÁN SỬ DỤNG
- Ở bài này sử dụng thuật toán biến dạng hình học geometric transformatio để tạo hiệu ứng sóng lượn trên ảnh

### GIẢI THÍCH HOẠT ĐỘNG
```python
def GeoFun(outcoord):
    a = 10 * np.cos(outcoord[0] / 10.0) + outcoord[0]
    b = 10 * np.cos(outcoord[1] / 10.0) + outcoord[1]
    return a, b

data = iio.imread("world_cup.jpg", mode = "F")

d1 = nd.geometric_transform(data, GeoFun)
```
-tạo 1 hàm GeoFun có tham số là outcoord. sau đó thiết lập cột và hàng của pixel. Như trong bài là a và b trong đó a là hàng tính từ outcoord[0] và b là ccootj tính từ outcoord[1]. Sau đó hàm sẽ trả về a và b tương ứng với tọa độ trong ảnh gốc. Đọc ảnh từ file world_cup.jpg và chuyển nó sang ảnh xám theo định dạng số thực, - d1 dùng hàm nd.geometric_transform() áp dụng phép biến đổi hình học tùy chỉnh cho ảnh dựa vào hàm ánh xạ


## 2. BÀI TẬP
### Bài tập 1: Chọn ảnh quả kiwi bất kì .Tịnh tiến quả kiwi 50 pixel sang phải và 30 pixel xuống dưới. Áp dụng hiệu ứng sóng (wave effect) lên quả kiwi bằng cách sử dụng biến đổi tọa độ (map_coordinates) với hàm sin. Lưu ảnh kết quả vào file kiwi_wave.jpg

### CÔNG NGHỆ SỬ DỤNG
* Gồm các thư viện:
- **numpy**: Xử lý toán học với mảng 
- **scipy**: Tính toán khoa học
- **imageio**: Đọc và ghi ảnh
- **matplotlib.pyplot**: Hiển thị ảnh và biểu đồ
### THUẬT TOÁN SỬ DỤNG
- Bài này dung nd.shift() từ thư viện scipy, cho phép dịch chuyển một ảnh theo trục x, y. Với yêu cầu bài ảnh được tịnh tiến quả kiwi 50 pixel sang phải và 30 pixel xuống dưới và sử dụng hàm sin để biến đổi ảnh thành gợn sóng
### GIẢI THÍCH HOẠT ĐỘNG

```python
def GeoFun(outcoord):
    a = 10 * np.sin(outcoord[0] / 10.0) + outcoord[0]
    b = 10 * np.sin(outcoord[1] / 10.0) + outcoord[1]
    return a, b
```
* tạo 1 hàm GeoFun có tham số là outcoord. sau đó thiết lập cột và hàng của pixel. Như trong bài là a và b trong đó a là hàng tính từ outcoord[0] và b là ccootj tính từ outcoord[1]. Sau đó hàm sẽ trả về a và b tương ứng với tọa độ trong ảnh gốc

```python
data = iio.imread("exercise/colorful-ripe-tropical-fruits.jpg")
bmg = data[920:1100, 390:570]
```
* đọc ảnh từ file "colorful-ripe-tropical-fruits.jpg" cắt ảnh từ pixel y = 920,1100 : x = 390,570

```python
bdata = nd.shift(bmg, (30, 50, 0))
```
* Dịch chuyển ảnh xuống dưới 30px theo tung độ (y) và dịch chuyển ảnh sang phải 50px theo hoành độ(x) và giữ nguyên biên độ màu là 0

```python
d1 = np.stack([nd.geometric_transform(bdata[:, :, i], GeoFun) for i in range(3)], axis=2)
```
* dùng list comprehension để lặp qua ba kênh màu RGB, xử lý từng kênh riêng biệt

```python
iio.imsave("kiwi_wave.jpg", d1.astype(np.uint8))
print(data.shape)
iio.imsave("kiwi.jpg", bdata)
```
* chuyển  d1 về kiểu uint8 để lưu ảnh và in kích thước của ảnh sau đó thì đồng thời lưu ảnh trước khi biến đổi thành sóng (hình này đã được tịnh tiến không phải ảnh gốc)

```python
plt.subplot(1, 2, 1)
plt.imshow(bdata)
plt.subplot(1, 2, 2)
plt.imshow(d1)
plt.show()
```
* hình bên trái là ảnh ban đầu sau khi đã được tính tiến, hình bên phải là ảnh đã áp dụng biến dạng kiểu sóng.

### Bài tập 2: Chọn quả đu đủ và dưa hấu từ google. Đổi màu đu đủ thành gradient từ đỏ sang xanh lá, và dưa hấu thành gradient từ vàng sang tím. Ghép hai quả lên một nền trong suốt (alpha channel) và lưu dưới dạng PNG.

### CÔNG NGHỆ SỬ DỤNG
* Gồm các thư viện:
- **numpy**: Xử lý toán học với mảng 
- **scipy**: Tính toán khoa học
- **imageio**: Đọc và ghi ảnh
- **matplotlib.pyplot**: Hiển thị ảnh và biểu đồ
### THUẬT TOÁN SỬ DỤNG
- Bài này sử dụng thuật color gradient mapping theo chiều dọc vag tạo canvas
### GIẢI THÍCH HOẠT ĐỘNG
```python
data = iio.imread("dudu.jpg")
data1 = iio.imread("duahau.jpg")
```
* Đọc hai ảnh gốc. data: ảnh đầu tiên (dudu.jpg). data1: ảnh thứ hai (duahau.jpg) 
```python
def doi_mau(img, mau1, mau2):
    h = img.shape[0]
    ti_le = np.linspace(0, 1, h).reshape(h, 1, 1)
    mau1 = np.array(mau1).reshape(1, 1, 3)
    mau2 = np.array(mau2).reshape(1, 1, 3)
    mau_moi = mau1 + (mau2 - mau1) * ti_le
    return (img / 255 * mau_moi).astype(np.uint8)
```
* tạo hàm doi_mau với các tham số img, mau1, mau2 theo chiều dọc
* h: chiều cao ảnh theo trục Y
* ti_le: tạo mảng theo tỷ lệ từ 0 đến 1 từ trên xuống
* mau1, mau2: 2 dòng này là định dạng màu đầu và màu cuối cho ảnh
* return áp dụng màu mới tạo được cho ảnh và ép kiểu giá trị kiểu giữ liệu về số nguyên 
```python
data_n = doi_mau(data, [255, 0, 0], [0, 255, 0])
data1_n = doi_mau(data1, [255, 255, 0], [128, 0, 128])
```
* data_n: đổi màu của ảnh du đủ từ đỏ xang xanh lá 
* data1_n: đổi màu của ảnh dưa hấu từ vàng sang tím
```python
h = max(data.shape[0], data1.shape[0])
w = data.shape[1] + data1.shape[1]
kq = np.zeros((h, w, 4), dtype=np.uint8)
```
* Tính kích thước canvas để chứa cả hai ảnh. trong đó h là chiều cao, w là độ rộng, còn kq lầ kết quả nền canvas với nền trong

```python
kq[0:data1.shape[0], 0:data1.shape[1], :3] = data1_n
kq[0:data1.shape[0], 0:data1.shape[1], 3] = 255
```
chèn ảnh dưa hấu đã đổi màu thành công vào canvas và được ở vị trí bên trái 

```python
x = data1.shape[1]
kq[0:data.shape[0], x:x+data.shape[1], :3] = data_n
kq[0:data.shape[0], x:x+data.shape[1], 3] = 255
```
* đầu tiên xác định vị trí bắt đầu của ảnh đu đủ trong nền canvas, dòng đầu là gán kênh màu cho ảnh dòng thứ 2 là gán tọa alpha và ảnh đu đủ được nằm ở vị trí bên phải

```python
iio.imwrite("kq.png", kq)
plt.imshow(kq)
plt.axis("off")
plt.show()
```
* lưu ảnh kết quả dưới định dạng PNG và show hình ra màn hình

## Bài 3: Chọn ảnh núi và thuyền . Xoay cả hai đối tượng 45 độ, giữ kích thước ban đầu (reshape=False). Tạo hiệu ứng phản chiếu dọc (vertical mirror) cho cả hai đối tượng sau khi xoay. Ghép cả hai đối tượng lên một canvas trắng và lưu vào mountain_boat_mirror.jpg

### CÔNG NGHỆ SỬ DỤNG
* Gồm các thư viện:
- **numpy**: Xử lý toán học với mảng 
- **scipy**: Tính toán khoa học
- **imageio**: Đọc và ghi ảnh
- **matplotlib.pyplot**: Hiển thị ảnh và biểu đồ
### THUẬT TOÁN SỬ DỤNG

- Xoay góc ảnh không thay đổi kích thước reshape=False va lật ảnh theo trục dọc

### GIẢI THÍCH HOẠT ĐỘNG
```python
data = iio.imread("mountain.jpg")
data1 = iio.imread("boat.jpg")
```
* Đọc hai ảnh gốc. data: ảnh đầu tiên (mountain.jpg). data1: ảnh thứ hai (boat.jpg) 

```python
data = nd.rotate(data, 45, reshape=False)
data1 = nd.rotate(data1, 45, reshape=False)
```
* xoay cả hai bức ảnh đi một góc 45 độ

```python
data = np.flipud(data)
data1 = np.flipud(data1)
```
* dùng flipud trong thư viện scipy.ndimage để lật ảnh

```python
plt.subplot(1, 2, 1)
plt.imshow(data)
plt.axis('off')
plt.subplot(1, 2, 2)
plt.imshow(data1)
plt.axis('off')
plt.savefig("mountain_boat_mirror.jpg")
plt.show()
```
* Lưu toàn bộ figure của cả hai hình xuất ra màn hình thành file JPG

## Bài 4: Chọn ngôi chùa bất kì. Phóng to ngôi chùa lên 5 lần. Áp dụng một biến đổi hình học tùy chỉnh (geometric transform) để tạo hiệu ứng "uốn cong" (warping) ngôi chùa. Lưu ảnh kết quả vào pagoda_warped.jpg.

### CÔNG NGHỆ SỬ DỤNG
* Gồm các thư viện:
- **numpy**: Xử lý toán học với mảng 
- **scipy**: Tính toán khoa học
- **imageio**: Đọc và ghi ảnh
- **matplotlib.pyplot**: Hiển thị ảnh và biểu đồ
### THUẬT TOÁN SỬ DỤNG
- bài này sử dụng nd.zoom để phóng to ảnh của thư viện scipy.ndimage ở bài này yêu cầu phóng to ảnh với kích thước gâp 5 lần và thuật toán Geometric Transformation sử dụng hàm sin làm cho ảnh méo giống kiểu bài 2 và chuyển ảnh sang màu xám để dễ xử lý ảnh
### GIẢI THÍCH HOẠT ĐỘNG
```python
def warp_function(output_coords):
    y, x = output_coords
    new_y = y + 20 * np.sin(x / 50.0)
    new_x = x + 20 * np.sin(y / 50.0)
    return new_y, new_x
```
- tạo hàm warp_function có tham số output_coords. sau đó tạo biến(y, x) để tính vị trí điểm ảnh gốc, biến new_y và new_x để làm tạo hiệu ứng sóng theo hàm sin. Sau đó trả lại giá trị x,y mới cho ảnh gốc

```python
data = iio.imread("pagoda.jpg")
print("Kích thước gốc:", data.shape)
z_data = nd.zoom(data, (5, 5, 1))
print("Kích thước sau khi phóng to:", z_data.shape)
```
- đọc ngôi chùa gốc và in ra kích thước ảnh ban đầu sau đó phóng to ảnh gấp 5 lần với chiều cao và chiều rộng đều gấp 5 riêng và kênh màu giữ nguyên và in ra giá trị của kích thước ảnh đã được tăng 5 lần
```python
gray = np.mean(z_data, axis=2)
warped = nd.geometric_transform(gray, warp_function)
```
- chuyển đổi ảnh thành màu xám để thuận tiện cho việc xử lý ảnh bằng cách lấy trung bình của 3 màu đỏ, lục, lam. sau đó gọi lại áp dụng biến đổi hình bằng nd.geometric_transform trong đó gọi lại biến gray là biến làm ảnh màu xám và gọi lại hàm 
```python
plt.imshow(warped)
plt.axis('off')
plt.show()
```
-hiển thị ảnh ra màn hình 
```python
iio.imsave("pagoda_warped.jpg", warped.astype(np.uint8))
```
- lưu ảnh đã được xử lý với tên pagoda_warped.jpg và ép kiểu giá trị kiểu giữ liệu về số nguyên 

## Bài 5: Tạo một chương trình menu tương tác cho phép người dùng chọn các phép biến đổi sau:
- Tịnh tiến (hỏi số pixel di chuyển theo x và y).
- Xoay (hỏi góc xoay và chọn reshape=True/False).
- Phóng to/thu nhỏ (hỏi hệ số zoom).
- Làm mờ Gaussian (hỏi giá trị sigma).
- Biến đổi sóng (hỏi biên độ sóng).
- Người dùng chọn ảnh từ 3 ảnh bất kì

### CÔNG NGHỆ SỬ DỤNG
* Gồm các thư viện:
- **numpy**: Xử lý toán học với mảng 
- **scipy**: Tính toán khoa học
- **imageio**: Đọc và ghi ảnh
- **matplotlib.pyplot**: Hiển thị ảnh và biểu đồ
### THUẬT TOÁN SỬ DỤNG
- Tịnh tiến ảnh
- Xoay ảnh.
- Phóng to/thu nhỏ.
- Làm mờ ảnh.
- Biến đổi sóng.
### GIẢI THÍCH HOẠT ĐỘNG
```python
def choose_image():
    print("Chọn ảnh để xử lý:")
    print("1. boat.jpg")
    print("2. world_cup.jpg")
    print("3. mountain.jpg")
    choice = input("Nhập lựa chọn (1-3): ")
    if choice == '1':
        return iio.imread("boat.jpg", mode="F"), "boat.jpg"
    elif choice == '2':
        return iio.imread("world_cup.jpg", mode="F"), "world_cup.jpg"
    elif choice == '3':
        return iio.imread("mountain.jpg", mode="F"), "mountain.jpg"
    else:
        print("Lựa chọn không hợp lệ.")
        return None, None
```
- Tạo hàm choose_image để hiển thị menu cho người dùng chọn 1 trong 3 ảnh có sẵn.
```python
def translate_image(data):
    dx = int(input("Nhập số pixel dịch theo trục X: "))
    dy = int(input("Nhập số pixel dịch theo trục Y: "))
    result = nd.shift(data, (dy, dx))
    return result
```
- Tọa hàm translate_image với tham số data để hỏi người dùng muốn dịch ảnh bao nhiêu pixel theo trục và trục y. Sau đó dùng nd.shift để dịch chuyển ảnh theo hướng người dùng đã nhập trước đó

```python
def rotate_image(data):
    angle = float(input("Nhập góc xoay (độ): "))
    reshape = input("reshape = True? (y/n): ").lower() == 'y'
    result = nd.rotate(data, angle, reshape=reshape)
    return result
```
- Tạo hàm rotate_image với tham số data để hỏi người dùng góc xoay ảnh và có muốn thay đổi kích thước ảnh sau khi xoay hay không dùng nd.rotate để xoay ảnh 

```python
def zoom_image(data):
    zx = float(input("Nhập hệ số zoom theo chiều cao: "))
    zy = float(input("Nhập hệ số zoom theo chiều rộng: "))
    if data.ndim == 3:
        result = nd.zoom(data, (zx, zy, 1))
    else:
        result = nd.zoom(data, (zx, zy))
    return result

``` 

-Tạo hàm zoom_image với tham số data để hỏi người dùng muốn phóng to ảnh với kích thước chiều cao và chiều rộng bao nhiêu dùng nd.zoom để phóng to thu nhỏ tương đươcg với giá trị người dùng nhập

```python
def blur_image(data):
    d = float(input("Nhập độ lệch tối đa (d): "))
    if data.ndim != 2:
        print("Ảnh làm mờ yêu cầu ảnh xám (mode='F').")
        return data
    V, H = data.shape
    M = np.indices((V, H))
    q = 2 * d * np.random.rand(*M.shape) - d
    mp = (M + q).astype(int)
    result = nd.map_coordinates(data, mp, order=1, mode='reflect')
    return result
```
- Tạo hàm blur_image với tham số data để hỏi nhận bao nhiêu giá trị độ lệch dịch chuyển ngẫu nhiên nhỏ trên từng pixel để làm mờ ảnh và hàm này chỉ áp dụng cho ảnh màu xám

```python
def wave_image(data):
    def GeoFun(outcoord):
        a = 10 * np.sin(outcoord[0] / 10.0) + outcoord[0]
        b = 10 * np.sin(outcoord[1] / 10.0) + outcoord[1]
        return a, b
    result = nd.geometric_transform(data, GeoFun)
    return result
```
Tạo hàm wave_image với tham số data dder tạo hiệu ứng ảnh sóng bằng cách sử dụng hàm sin để tính độ cong và áp dụng lên toàn bộ ảnh bằng geometric_transform.

```python
def show_image(data, title):
    plt.imshow(data, cmap='gray' if data.ndim == 2 else None)
    plt.show()
```
- hàm này chủ yếu để hiển thị ảnh ra mà hình cũng không còn xa lạ gì nữa chỉ khác biệt ở đây là "if data.ndim == 2 else None" thì đây là nếu mà bức ảnh đã xử lý thành màu xám rồi thì chỉ có chiều cao và chiều rộng thôi

```python
def main():
    while True:
        data, name = choose_image()
        if data is None:
            continue

        print("\nChọn phép biến đổi:")
        print("1. Tịnh tiến")
        print("2. Xoay")
        print("3. Phóng to / Thu nhỏ")
        print("4. Làm mờ")
        print("5. Biến đổi sóng")
        print("0. Thoát")
        choice = input("Nhập lựa chọn: ")

        if choice == '1':
            result = translate_image(data)
            show_image(result, "Tịnh tiến ảnh")
        elif choice == '2':
            result = rotate_image(data)
            show_image(result, "Xoay ảnh")
        elif choice == '3':
            result = zoom_image(data)
            show_image(result, "Phóng to / Thu nhỏ ảnh")
        elif choice == '4':
            result = blur_image(data)
            show_image(result, "Ảnh bị làm mờ")
        elif choice == '5':
            result = wave_image(data)
            show_image(result, "Biến đổi sóng ảnh")
        elif choice == '0':
            print("Thoát chương trình.")
            break
        else:
            print("Lựa chọn không hợp lệ.")

        again = input("\nBạn có muốn xử lý ảnh khác? (y/n): ").lower()
        if again != 'y':
            break

if __name__ == "__main__":
    main()
``` 
- đây là hàm để thực thi chương trình với các lựa chọn tương ứng với các yêu cầu như tịnh tiến, xoay ảnh, phóng to thu nhỏ, làm ảnh mờ, và biến đổi sóng của ảnh

