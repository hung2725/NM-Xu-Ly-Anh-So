# Nhập Môn Xử Lý Ảnh Số - Lab 5

## XÁC ĐỊNH ĐỐI TƯỢNG TRONG ẢNH
- **Sinh Viên Thực Hiên:** Phạm Thế Hùng
- **MSSV:** 2374802010164
- **Môn Học:** Nhập Môn Xử Lý Ảnh Số
- **Giảng viên:** Đỗ Hữu Quân

## 1. VIẾT CHƯƠNG TRÌNH GÁN NHÃN ẢNH

## Công nghệ sử dụng
- **Python:** Ngôn ngữ chính
- **numpy:** Xử lý ảnh dưới dạng mảng số học
- **scipy:**  giải quyết các bài toán kỹ thuật và toán học phức tạp
- **skimage:** xử lý và phân tích hình ảnh
- **imageio:** Đọc file ảnh với định dạng hiện đại
- **matplotlib:** Hiển thị ảnh trực quan

### Gán nhãn ảnh
**Mục đích:**  
- ĐƯợc sử dụng để đào tạo AI và các mô hình học máy để xác định các đối tượng từ hình ảnh và video:
- VD: ứng dụng vào mô hình xe tự lái giúp xe phân tích về đường, biển báo và chướng ngoại vật để có hướng đi an toàn

**Nguyên lý:** Gán nhãn các đối tượng trong ảnh dựa vào vị trí bàng cách đóng khung,vẽ đa giác hay phân doạn gán từng px đối tượng sau đó phân loại và gán nhãn cho từng đối tượng có trong ảnh

**Code chính**
```python
fig, ax = plt.subplots(ncols=1, nrows=1, figsize=(6, 6))
ax.imshow(c, cmap='YlOrRd')
for i in d:
    lr, lc, ur, uc = i['BoundingBox']
    rec_width = uc - lc
    rec_height = ur - lr
    rect = mpatches.Rectangle((lc, lr), rec_width, rec_height, fill=False,
                              edgecolor='black', linewidth=2)
    ax.add_patch(rect)
plt.show()
```

### Dò tìm cạnh theo chiều dọc
**Mục đích:** Phát hiện các biên hoặc sự thay đổi độ sáng rõ rệt trong ảnh theo chiều ngang thì sẽ dò ra được các cạnh dọc của ảnh

**Nguyên lý:** 
- Chuyển ảnh về ảnh màu xám và chỉ giữ lại cường độ sáng của ảnh
- trừ giá trị của mổi điểm ảnh với giá trị điểm ảnh bên cạnh
- Lấy giá trị tuyệt đối đảm bảo cho sự chuyển từ sáng sang tối và từ tối sang sáng thì nó sẽ ghi nhận đó là 1 cạnh 

**Code chính**
```python
data = Image.open("geometric.png").convert("L")
bmg = abs(data - nd.shift(data, (0,1), order = 0))
```

### Dò tìm cạnh với Sobel Filter
**Mục đích:**  phát hiện các biên trong ảnh theo các hướng cụ thể từ đó làm nổi bật hình dạng đối tượng. phương pháp này thường được sử dụng rộng rãi trong  xử lý ảnh để phân đoạn và nhận dạng đối tượng

**Nguyên lý:** tính toán độ chênh lệch độ sáng giữa các điểm ảnh liền kề bằng đạo hàm gần đúng theo chiều ngang và dọc để phát hiện các biên

**Công thức toán học**

- G = sqrt(Gx² + Gy²) hoặc G = |Gx| + |Gy|


Trong đó: 
- `G`: là biên tổn hợp
- `Gx`: gradient theo trục hoành 
- `Gy`: gradient theo trục tung

**Code chính**
```python
data = Image.open("geometric.png")
a = nd.sobel(data, axis = 0)
b = nd.sobel(data, axis = 1)
bmg = abs (a) + abs(b)
```

### Xác định góc của đối tượng
**Mục đích:** Xác định hướng xoay của một đối tượng trong ảnh để hiểu hướng nằm, góc nghiêng, hoặc tư thế tương đối của nó so với trục ngang

**Nguyên lý:** phân tích hình dạng và sự phân bố các điểm ảnh của đối tượng phương pháp phổ biến như phân tích momen hình học hoặc phân tích thành phần chính được sử dụng để tìm ra trục chính của đối tượng

**Code chính**
```python
def Harris(indata, alpha = 0.2):
    x = nd.sobel(indata, 0)
    y = nd.sobel(indata, 1)
    x1 = x ** 2
    y1 = y ** 2
    xy = abs(x * y)
    x1 = nd.gaussian_filter(x1,3)
    y1 = nd.gaussian_filter(y1, 3)
    xy =nd.gaussian_filter(xy, 3)

    detC = x1 * y1 - 2 *xy
    trC = x1 + y1
    R = detC - alpha *trC ** 2
    return R

data = Image.open("geometric.png")
bmg = Harris(data)
```
### Dò tìm đường thẳng trong ảnh
**Mục đích:** phát hiện đường thẳng trong xử lý ảnh được ứng dụng trong những lĩnh vực: nhận diện làn đường, đọc văn bản , nhận diện biểu mẫu, kiểm tra chất lượng công nghiệp

**Nguyên lý:** tìm đường thẳng từ không gian ảnh Cartesian x, y sang không gian tham số. Trong không gian tham số này, tất cả các điểm nằm trên cùng một đường thẳng trong ảnh gốc sẽ hội tụ tại một điểm duy nhất

**Code chính**
```python
def LineHough(data, gamma):
    V, H = data.shape
    R = int(np.sqrt(V * V + H * H))
    ho = np.zeros((R, 90), float)  # Hough space
    w = data + 0
    ok = 1
    theta = np.arange(90) / 180.0 * np.pi
    tp = np.arange(90).astype(float)
    while ok:
        mx = w.max()
        if mx < gamma:
            ok = 0
        else:
            v, h = divmod(w.argmax(), H)
            y = V - v
            x = h
            rh = x * np.cos(theta) + y * np.sin(theta)
            for i in range(len(rh)):
                if 0 <= rh[i] < R and 0 <= tp[i] < 90:
                    ho[int(rh[i]), int(tp[i])] += mx
            w[v, h] = 0
    return ho

data = np.zeros((256, 256))
data[128, 128] = 1
bmg = LineHough(data, 0.5)
```

### Dò tìm đường tròn trong ảnh
**Mục đích:** Phát hiện và xác định vị trí, kích thước của các đường tròn hoặc hình tròn trong ảnh thường được xử dụng trong việc nhận dạng đồng xu, logo,mắt, phát hiện đèn giao thông, các lỗ trên bề mặt 

**Nguyên lý:** Dùng Hough Circle Transform để tìm tâm và bán kính của đường tròn mỗi pixel biên sẽ đề xuất các đường tròn đi qua nó

**Code chính**
```python
data = iio.imread('bird.png')
image_gray = rgb2gray(data)
coordinate = corner_harris(image_gray, k=0.001)
```


## CÀI ĐẶT THƯ VIỆN
```python
pip install pillow numpy matplotlib imageio opencv-python
```

## TÀI LIỆU THAM KHẢO
- [Gán nhãn ảnh - Shaip.com](https://vi.shaip.com/blog/image-annotation-for-computer-vision/)

- [Phát hiện cạnh trong hình ảnh - Najam R. Syed](https://nrsyed.com/2018/02/18/edge-detection-in-images-how-to-derive-the-sobel-operator/)

- [Object Detection là gì? - Mỹ Y](https://interdata.vn/blog/object-detection-la-gi/)

- [Phát hiện đường thẳng với Hough Transform - Viet Anh](https://www.vietanh.dev/blog/2019-10-24-hough-transform-phat-hien-duong-thang)

- [Hough Circle Transform](https://docs.opencv.org/3.4/d4/d70/tutorial_hough_circle.html)