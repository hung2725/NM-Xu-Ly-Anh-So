# Nhập Môn Xử Lý Ảnh Số - Lab 4

## Phân Vùng Ảnh
- **Sinh Viên Thực Hiên:** Phạm Thế Hùng
- **MSSV:** 2374802010164
- **Môn Học:** Nhập Môn Xử Lý Ảnh Số
- **Giảng viên:** Đỗ Hữu Quân

## 1. VIẾT CHƯƠNG TRÌNH PHÂN VÙNG ẢNH

## Công nghệ sử dụng
- **Python:** Ngôn ngữ chính
- **numpy:** Xử lý ảnh dưới dạng mảng số học
- **scipy:**  giải quyết các bài toán kỹ thuật và toán học phức tạp
- **skimage:** xử lý và phân tích hình ảnh
- **ImageIO:** Đọc file ảnh với định dạng hiện đại
- **Matplotlib:** Hiển thị ảnh trực quan
### Phương pháp Otsu
**Mục đích:**  
- Tự động tìm giá trị ngưỡng tối ưu để phân chia ảnh thành hai lớp foreground và background 
- tối đa hóa phương sai giữa các lớp 
- tối thiểu hóa phương sai trong lớp

**Code chính:**  
```python
data = Image.open('moon.jpg').convert('L')
a = np.array(data)
thres = threshold_otsu(a)
b = a > thres
b = Image.fromarray(b)
```
### Phương pháp Adaptive Thesholding
**Mục địch:** phân ngưỡng thích nghi được sử dụng để chuyển ảnh xám thành ảnh nhị phân trong các trường hợp ánh sáng không đồng đều

**Nguyên lý:**chia ảnh thành các vùng nhỏ sau đó tính ngưỡng riêng của mỗi vùng rôi đem so sanh từng điểm ảnh với ngưỡng của vùng nếu mà sáng hơn ngưỡng thì thành trắng nếu không thì thành màu đen

**Code chính**
```python
data = Image.open('moon.jpg').convert('L')
a = np.array(data)
thres = threshold_otsu(a)
b = threshold_local(a, 39, offset=10)
b = Image.fromarray(b)
```

### Phân vùng theo region
**Mục địch:** 
Phân theo vùng chia ảnh thành các khu vực mà các điểm ảnh giống nhau về độ sáng hoặc màu sắc.Trong kiểu phân vùng này, có một số quy tắc được định sẵn mà pixel phải tuân theo để đảm bảo có thể phân loại thành các vùng pixel tương tự phương pháp phân vùng dựa trên khu vực được ưu tiên hơn phương pháp phân vùng dựa trên cạnh trong trường hợp ảnh bị nhiễu

**Nguyên lý:** Chuyển ảnh sang ảnh xám để dễ xử lý ảnh và nhị phân hóa bằng Otsu để tách nền và đối tượng, giảm nhiễu và tách rời các vùng gần nhau khoảng cách của cá điểm ảnh. Tính khoảng cách từ mỗi điểm ảnh nền đến điểm ảnh gần nhất thuộc đối tượng tạo các vùng cho thuật toán phân vùng

**Code chính**
```python
data = cv2.imread('moon.jpg')
a = cv2.cvtColor(data, cv2.COLOR_BGR2GRAY)
thresh, b1 = cv2.threshold(a, 0, 255, cv2.THRESH_BINARY_INV + cv2.THRESH_OTSU)
b2 = cv2.erode(b1, None, iterations = 2)
dist_trans = cv2.distanceTransform(b2, 2, 3)
thresh, dt = cv2.threshold(dist_trans, 1, 255, cv2.THRESH_BINARY)
labelled, ncc = label(dt)
labelled = labelled.astype(np.int32)
cv2.watershed(data, labelled)
b = Image.fromarray(labelled)
```
### Sử dụng binary_dilation
**Mục địch:** Với những hình ảnh bị đứt nét có thể iúp nối liền ảnh lại với những pixel nhiễu xung quanh đối tượng sẽ trở thành viền của đối tượng giúp nổi bật đối tượng trong ảnh hơn

**Nguyên lý:** trong đó một phần tử cấu trúc structuring element được quét qua ảnh. Với mỗi vị trí, nếu có ít nhất một điểm ảnh trong phần tử cấu trúc trùng với điểm ảnh thường là  hoặc 255, thì điểm ảnh trung tâm sẽ được giữ lại


**Coogn thức toán học**

![markdown](https://resources.iostream.co/content/article/5ef6221c24fd2869e91f18ff/resources/res-1598853113-1598853113696.png)

Trong đó: 
- `A` ma trận của điểm ảnh nhị phân
- `B` là phần tử cấu trúc

**Code chính**
```python
data = Image.open('city.gif').convert('L')
b = nd.binary_dilation(data, iterations=50)
c = Image.fromarray(b)
```
### Sử dụng binary_opening

**Mục địch:** đây là thuật toán kết hợp giữa erosion và dilation. Thực hiện phép erosion trước sau đó mới thực hiện phép giãn dilation nhằm loại bỏ nhiễu nhỏ xóa các đối tượng nhỏ hơn phần tử cấu trúc. Làm mịn biên đối tượng giúp các cạnh trở nên gọn gàng hơn.Tách các đối tượng gần nhau nếu chúng chỉ nối với nhau bằng các điểm nhỏ

**Nguyên lý:** Đầu tiên phép erosion được áp dụng lên ảnh A với phần tử cấu trúc B. Bước này sẽ loại bỏ tất cả các đối tượng hoặc phần của đối tượng nhỏ hơn phần tử cấu trúc B. Các đối tượng lớn hơn sẽ bị co lại dilation sau đó,phép dilation được áp dụng lên kết quả của bước erosion với cùng phần tử cấu trúc B lúc này sẽ giãn nở lại các đối tượng đã bị co lại, khôi phục lại kích thước ban đầu của chúng, nhưng các đối tượng nhỏ đã bị loại bỏ ở bước erosion sẽ không được khôi phục

**Công thức toán học**

![markdown](https://resources.iostream.co/content/article/5ef6221c24fd2869e91f18ff/resources/res-1600422830-1600422830140.png)

**Code chính**
```python
data = Image.open('city.gif').convert('L')
s = [[0,1,0],[1,1,1],[0,1,0]]
b = nd.binary_opening(data,  structure=s, iterations=25)

c = Image.fromarray(b)
```
### Sử dụng binary_erosion
**Mục địch:** Loại bỏ những pixel nhiễu cô lập và loại bỏ những pixel nhiễu xung quanh đối tượng giúp cho phần viền của đối tượng trở nên mịn hơn loại bỏ lớp viền của đối tượng giúp đối tượng trở nên nhỏ hơn và đặt những pixel viền đó trở thành lớp nền của đối tượng

**Nguyên lý** Được áp dụng trong những hình ảnh nhị phân tuy nhiên có một số phiên bản sẽ được áp dụng trên những hình ảnh xám tuy nhiên trong phạm vi bài viết của mình hôm nay thì chỉ tập trung vào những hình ảnh nhị phân

**Công thức toán học**

![markdown](https://resources.iostream.co/content/article/5ef6221c24fd2869e91f18ff/resources/res-1600422438-1600422438845.png)

**Code chính**
```python
data = Image.open('city.gif').convert('L')
s = [[0,1,0],[1,1,1],[0,1,0]]
b = nd.binary_erosion(data,  structure=s, iterations=50)
c = Image.fromarray(b)
```
### Sử dụng binary_closing

**Mục địch:** cũng dùng 2 thuật toán như opening nhưng sẽ khác về cách thực hiện phép dilation trước sau đó mới thực hiện erosion. Lấp đầy các lỗ nhỏ hoặc khoảng trống bên trong đối tượng kết nối các vùng gần nhau nếu chúng bị ngắt quãng bởi các khe hẹp hoặc nhiễu nhỏ

**Nguyên lý:** Đầu tiên, phép Dilation được áp dụng lên ảnh A với phần tử cấu trúc B. Bước này sẽ làm giãn nở các đối tượng, lấp đầy các lỗ nhỏ bên trong chúng và kết nối các khoảng trống hẹp giữa các đối tượng sau đó,phép Erosion được áp dụng lên kết quả của bước Dilation với cùng phần tử cấu trúc B lúc này sẽ co lại các đối tượng đã bị giãn nở, khôi phục lại kích thước ban đầu của chúng nhưng các lỗ nhỏ đã được lấp đầy và các khoảng trống đã được kết nối sẽ vẫn được giữ nguyên


**Công thức toán học**
![markdown]( https://resources.iostream.co/content/article/5ef6221c24fd2869e91f18ff/resources/res-1600422817-1600422817634.png)

**Code chính**
```python
data = Image.open('city.gif').convert('L')
s = [[0,1,0],[1,1,1],[0,1,0]]
b = nd.binary_closing(data,  structure=s, iterations=50)
c = Image.fromarray(b)
```
## 2. BÀI TẬP
### 1.Viết chương trình chọn LangBiang trong ảnh Đà Lạt từ thư mục exercise. Tịnh tiến vùng chọn sang phải 100px. Sử dụng phương pháp Otsu để phân vùng LangBiang theo ngưỡng 0.3. Lưu vào máy với tên lang_biang.jpg và hiển thị trên màn hình.

**Giải thích hoạt động:** bài này ta sử dụng kỹ thuật cắt ảnh để cắt đúng vị trí LangBiang cần xử lý ảnh sau sử dụng phép tịnh tiến để đưa ảnh đã được cắt sang bên phải 100px. sau đó tại dòng biến tên thres ta bắt đầu sử dụng kỹ thuật Ostu từ ảnh gốc để tạo ngưỡng nhị phân ta dùng ngưỡng này để phân tách vùng sáng và tối trên phần ảnh đã cắt và tịnh tiến cuối cùng là ta lưu ảnh với tên lang_biang.jpg đồng thời ta ép về kiểu 8 bit

**Code chính**
```python
data = Image.open('exercise/dalat.jpg').convert('L')
a = np.array(data)
bmg = a[100:340, 100:450]
bmg = nd.shift(bmg, (0, 100))
thres = threshold_otsu(a)
d = bmg > thres
iio.imsave('lang_biang.jpg', (d.astype(np.uint8) * 255))
```
### 2. Viết chương trình chọn Hồ Xuân Hương trong ảnh Đà Lạt từ thư mục exercise. Xoay đối tượng vừa chọn 1 góc 45° và dùng phương pháp Adaptive Thresholding với ngưỡng 60 và lưu vào máy với tên là ho_xuan_huong.jpg.

**Giải thích hoạt động:** bài này bước đầu tiên ta vẫn làm như bước trên là cắt chọn Hồ xuân hương trong file ảnh gốc sau đó là dùng nd.rotate để xoay ảnh với góc là 45 độ và những viền xung quanh là viền màu đen biểu thị cho những chỗ đó có giá trị bằng 0 tiếp đó mới đến bước quan trọng của bài tập đó là dùng phương pháp Adaptive Thresholding với ngưỡng 60 ảnh sẽ bị mờ đi đáng kể với lúc ban đâu
và cuối cùng là lưu ảnh vơi tên ho_xuan_huong.jpg

**Code chính**
```python
data = Image.open('exercise/dalat.jpg').convert('L')
a = np.array(data)
bmg = a[40:680, 510:990]
d = nd.rotate(bmg, 45)
a = np.array(d)
b = threshold_local(a, 39, offset=60)
b = Image.fromarray(b)
iio.imsave("ho_xuan_huong.jpg", b.convert('L'))
```
### 3.Viết chương trình chọn Quảng trường Lâm Viên trong ảnh Đà Lạt từ thư mục exercise. Dùng phương pháp Coordinate Mapping và Binary Closing cho vùng vừa chọn. Lưu vào máy với tên là quan_truong_lam_vien.jpg.

**Giải thích hoạt động:** bài này bước đầu cũng như 2 bài trên là cắt chọn ảnh quảng trường lâm viên trong file ảnh gốc sau đó ảnh này được làm biến dạng bằng phương pháp Coordinate Mapping tiếp đó sử dụng phương pháp Binary Closing để lấp những vùng tối của ảnh và nối các vùng sáng lại với nhau  và cuối cùng là lưu ảnh lại với tên quang_truong_lam_vien.jpg

**Code Chính**
```python
data = Image.open('exercise/dalat.jpg').convert('L')
a = np.array(data)
bmg = a[50:330, 1010:1470]
V, H = bmg.shape
M = np.indices((V, H))
d = 5
q = 2 * d * np.random.ranf(M.shape) - d
mp = (M + q).astype(int)
d1 = nd.map_coordinates(bmg, mp)
s = ([[0, 1, 0], [1, 1, 1], [0, 1, 0]])
b = nd.binary_closing(d1, structure=s, iterations=50)
c  = Image.fromarray(b)
iio.imsave("quang_truong_lam_vien.jpg", c.convert('L'))
```
## CÀI ĐẶT THƯ VIỆN
```python
pip install pillow numpy matplotlib imageio opencv-python
```

## TÀI LIỆU THAM KHẢO
- [Xử lý ảnh với opencv các phép toán](https://www.iostream.co/article/xu-ly-anh-voi-opencv-cac-phep-toan-hinh-thai-hoc-Nljcg)

- [TAPIT](https://tapit.vn/xu-ly-anh-hinh-thai-hoc-morphological-image-processing/)

- [Bài giảng xử lý ảnh số - TS. Ngô Quốc Việt](https://tailieu.vn/doc/bai-giang-xu-ly-anh-so-chuong-7-ts-ngo-quoc-viet-1860718.html)