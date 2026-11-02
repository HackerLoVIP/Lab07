# BÁO CÁO THỰC HÀNH LAB 07
## KIỂM THỬ API CƠ BẢN BẰNG POSTMAN

---

### THÔNG TIN SINH VIÊN
- **Họ và tên:** Lê Trọng Hiệu
- **Mã số sinh viên (MSSV):** 231A290152
- **Lớp / Học phần:** Công Nghệ Phần Mềm

---

### 1. MỤC TIÊU BÀI THỰC HÀNH
- Tạo một Collection trong Postman để quản lý các request kiểm thử API của dịch vụ JSONPlaceholder.
- Khai báo và sử dụng biến dùng chung `baseUrl` để không phải gõ lại địa chỉ server nhiều lần.
- Thực hành gửi các request GET và POST, biết cách cấu hình tham số Params, Header và Body JSON.
- Viết các đoạn test script đơn giản bằng JavaScript trong Postman để tự động kiểm tra mã trạng thái status code và dữ liệu JSON trả về.
- Quan sát và ghi nhận các thông tin phản hồi như: Status code, Response time (thời gian phản hồi), dung lượng response và nội dung dữ liệu trả về.

---

### 2. HƯỚNG DẪN TẠO COLLECTION VÀ BIẾN DÙNG CHUNG
- Mở Postman, tạo một Collection mới đặt tên là: `jsonplaceholder-postman-lab`.
- Trong mục Variables của Collection, em tạo một biến:
  - Tên biến: `baseUrl`
  - Giá trị (Initial / Current Value): `https://jsonplaceholder.typicode.com`
- Sau đó, ở các request con bên dưới em chỉ cần gọi theo cú pháp `{{baseUrl}}/endpoint`.
- Thứ tự các request được tạo lần lượt gồm:
  1. `GET Posts List`
  2. `GET Single Post`
  3. `GET Comments By PostId`
  4. `POST Create New Post`
  5. `GET Users List` (bài mở rộng)

---

### 3. QUÁ TRÌNH THỰC HIỆN VÀ KẾT QUẢ TỪNG BÀI

#### 3.1. Bài 1: Kiểm thử API danh sách (GET /posts)
- **Cách cấu hình:**
  - Name: `GET Posts List`
  - Method: `GET`
  - URL: `{{baseUrl}}/posts`
- **Test script viết trong tab Tests:**
  - Kiểm tra status code là 200.
  - Kiểm tra dữ liệu trả về là dạng mảng JSON.
  - Kiểm tra số lượng bài post có đủ 100 bài.
  - Kiểm tra phần tử đầu tiên có trường `id` và `title`.
- **Kết quả khi bấm gửi request 5 lần:**
  - Lần 1: Status 200 OK | Thời gian: 368 ms | Dung lượng: 27.52 KB
  - Lần 2: Status 200 OK | Thời gian: 169 ms | Dung lượng: 27.52 KB
  - Lần 3: Status 200 OK | Thời gian: 102 ms | Dung lượng: 27.52 KB
  - Lần 4: Status 200 OK | Thời gian: 90 ms  | Dung lượng: 27.52 KB
  - Lần 5: Status 200 OK | Thời gian: 106 ms | Dung lượng: 27.52 KB
  - Thời gian phản hồi trung bình: khoảng **167 ms**.
- **Nhận xét (theo mục 6.5 đề bài):**
  - *API có trả lỗi không?* &rarr; Không, cả 5 lần chạy server đều trả về mã 200 OK, không có lỗi.
  - *Thời gian phản hồi giữa các lần gửi có ổn định không?* &rarr; Lần gửi đầu tiên tốn 368 ms vì máy vừa mới kết nối đến server. Từ lần thứ 2 trở đi thì tốc độ nhanh và ổn định hơn hẳn, dao động từ 90 ms đến 169 ms.
  - *Dữ liệu trả về có đúng định dạng mảng JSON không?* &rarr; Đúng, kết quả trả về là một mảng JSON chứa đúng 100 bài viết.
- **Minh chứng kết quả:**
  ![GET Posts List](screenshots/request_1_get_posts.png)

---

#### 3.2. Bài 2: So sánh API danh sách và API chi tiết (GET /posts/1)
- **Cách cấu hình:**
  - Name: `GET Single Post`
  - Method: `GET`
  - URL: `{{baseUrl}}/posts/1`
- **Test script viết trong tab Tests:**
  - Kiểm tra status code là 200.
  - Kiểm tra kết quả trả về là đối tượng JSON (Object).
  - Kiểm tra `id` trả về đúng bằng 1.
  - Kiểm tra có các trường `title` và `body`.
- **Kết quả thực hiện:**
  - Status code: **200 OK**
  - Thời gian phản hồi trung bình: khoảng **68 ms**
  - Dung lượng response: **292 bytes**
- **Nhận xét và so sánh (theo mục 7.5 đề bài):**
  - *So sánh response time và kích thước:* Request `GET /posts/1` chạy rất nhanh, chỉ mất khoảng 68 ms (nhanh hơn nhiều so với `GET /posts` là 167 ms). Dung lượng của `GET /posts/1` cũng rất nhỏ, chỉ có 292 bytes (nhẹ hơn gần 100 lần so với 27.5 KB của `GET /posts`).
  - *Giải thích lý do:* Vì request danh sách `GET /posts` phải lấy toàn bộ 100 bài viết trong cơ sở dữ liệu nên gói dữ liệu gửi về rất lớn và tốn thời gian tải hơn. Còn request chi tiết `GET /posts/1` chỉ cần lấy đúng duy nhất 1 bài viết có id = 1 nên dữ liệu rất ít, nhẹ và tải về cực kỳ nhanh.
- **Minh chứng kết quả:**
  ![GET Single Post](screenshots/request_2_get_single_post.png)

---

#### 3.3. Bài 3: Kiểm thử API có query parameter (GET /comments?postId=1)
- **Cách cấu hình:**
  - Name: `GET Comments By PostId`
  - Method: `GET`
  - URL: `{{baseUrl}}/comments`
  - Trong tab **Params**, em thêm: Key là `postId`, Value là `1`.
- **Test script viết trong tab Tests:**
  - Kiểm tra status code là 200.
  - Kiểm tra dữ liệu trả về là mảng danh sách bình luận.
  - Kiểm tra toàn bộ bình luận trong mảng đều có trường `postId = 1`.
  - Kiểm tra bình luận có các trường `email` và `body`.
- **Kết quả thực hiện:**
  - Status code: **200 OK**
  - Thời gian phản hồi trung bình: khoảng **65 ms**
  - Dung lượng response: **1,510 bytes** (khoảng 1.5 KB), trả về đúng 5 bình luận của bài viết số 1.
- **Nhận xét và so sánh (theo mục 8.5 đề bài):**
  - *So sánh với GET /posts/1:* Cả hai request đều có thời gian phản hồi rất nhanh (~65 ms). Về kích thước thì `GET /comments?postId=1` (1.5 KB) lớn hơn `GET /posts/1` (292 bytes) vì nó trả về danh sách 5 bình luận thay vì 1 bài viết.
  - *Vai trò của query parameter trong kiểm thử API:* Query parameter dùng để truyền điều kiện lọc dữ liệu cho server mà không làm thay đổi đường dẫn URL. Khi kiểm thử API, việc truyền query param giúp kiểm tra xem backend có lọc đúng dữ liệu theo yêu cầu hay không (ở đây kiểm tra thấy đúng là server chỉ trả về các bình luận có `postId == 1`).
- **Minh chứng kết quả:**
  ![GET Comments By PostId](screenshots/request_3_get_comments_by_postid.png)

---

#### 3.4. Bài 4: Kiểm thử API tạo mới với POST (/posts)
- **Cách cấu hình:**
  - Name: `POST Create New Post`
  - Method: `POST`
  - URL: `{{baseUrl}}/posts`
  - Trong tab **Headers**, thêm: `Content-Type: application/json`
  - Trong tab **Body**, chọn **raw** &rarr; định dạng **JSON** và nhập nội dung:
```json
{
  "title": "Postman Lab",
  "body": "Bai thuc hanh kiem thu API bang Postman",
  "userId": 1
}
```
- **Test script viết trong tab Tests:**
  - Kiểm tra status code tạo mới thành công là 201 Created.
  - Kiểm tra server có trả về trường `id` mới là 101.
  - Kiểm tra tiêu đề trả về đúng với tiêu đề đã gửi lên.
  - Kiểm tra thời gian phản hồi dưới 1000 ms.
- **Thực hiện gửi 3 lần với tiêu đề khác nhau (theo mục 9.4 đề bài):**
  - Lần 1 (title: "Postman Lab"): Status 201 Created, thời gian 389 ms, body trả về có `id: 101`.
  - Lần 2 (title: "Test Title 2"): Status 201 Created, thời gian 347 ms, body trả về có `id: 101`.
  - Lần 3 (title: "Test Title 3"): Status 201 Created, thời gian 328 ms, body trả về có `id: 101`.
  - Dung lượng response khoảng 109 - 110 bytes.
- **Nhận xét (theo mục 9.5 đề bài):**
  - *API có trả về dữ liệu JSON sau khi tạo mới không?* &rarr; Có, server trả về dữ liệu đúng định dạng JSON.
  - *Trường id có xuất hiện trong response không?* &rarr; Có, server tự sinh thêm trường `id: 101` cho bài viết mới tạo.
  - *So sánh phản hồi của POST với các request GET trước đó:*
    - Khi dùng GET thì server trả về status `200 OK`, còn khi dùng POST tạo mới thành công thì server trả về status `201 Created`.
    - Thời gian chạy của POST (~350 ms) lâu hơn so với GET đơn lẻ (~65 ms) vì server phải nhận dữ liệu body gửi lên và xử lý lưu vào cơ sở dữ liệu.
    - Request POST có gửi kèm body dữ liệu lên server, còn các request GET thì không có body.
- **Minh chứng kết quả:**
  ![POST Create New Post](screenshots/request_4_post_create_new_post.png)

---

### 4. BẢNG TỔNG HỢP KẾT QUẢ KIỂM THỬ (THEO MẪU MỤC 11)

| Request | Method | Status code | Response time | Response size | Kết quả test script |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **GET /posts** | GET | 200 OK | ~167 ms | 27,520 bytes (27.5 KB) | Pass (4/4 tests) |
| **GET /posts/1** | GET | 200 OK | ~67.6 ms | 292 bytes | Pass (4/4 tests) |
| **GET /comments?postId=1** | GET | 200 OK | ~64.6 ms | 1,510 bytes (1.5 KB) | Pass (4/4 tests) |
| **POST /posts** | POST | 201 Created | ~389.2 ms | 109 bytes | Pass (4/4 tests) |

---

### 5. NHẬN XÉT VỀ TÍNH ĐÚNG ĐẮN CỦA API VÀ SỰ KHÁC NHAU GIỮA CÁC LOẠI REQUEST
- **Về tính đúng đắn của API:**
  - Dịch vụ JSONPlaceholder hoạt động rất tốt và chuẩn: khi lấy dữ liệu bằng GET thì trả về mã 200 OK; khi tạo mới dữ liệu bằng POST thì trả về đúng mã 201 Created kèm theo mã id mới sinh (101).
  - Dữ liệu trả về đúng chuẩn JSON, không có request nào bị lỗi.
- **Sự khác nhau giữa các loại request:**
  - Request danh sách (`GET /posts`) lấy nhiều dữ liệu nên dung lượng nặng (27.5 KB) và mất nhiều thời gian tải hơn.
  - Request chi tiết (`GET /posts/1`) chỉ lấy đúng 1 bản ghi nên dung lượng rất nhẹ (292 bytes) và phản hồi rất nhanh.
  - Request có query param (`GET /comments?postId=1`) giúp lọc đúng 5 bình luận theo bài viết mong muốn.
  - Request POST gửi thêm dữ liệu body lên server và nhận về mã 201 Created, thời gian xử lý lâu hơn các request GET đơn lẻ.

---

### 6. BÀI TẬP MỞ RỘNG (MỤC 12 ĐỀ BÀI)
- Em đã tạo thêm 1 request `GET /users` để luyện tập kiểm tra dữ liệu danh sách người dùng (kết quả trả về mảng 10 user).
- Viết thêm lệnh test script kiểm tra số lượng phần tử `pm.expect(jsonData.length).to.eql(100)`.
- Sử dụng tính năng **Collection Runner** trong Postman để chạy tự động toàn bộ collection với 5 lần lặp (5 iterations). Kết quả chạy 20 lượt request đều thành công, đạt **80/80 tests Passed (100%)**.
- Tạo file cấu hình môi trường `jsonplaceholder-environment.postman_environment.json` để lưu biến `baseUrl`.
- **Minh chứng chạy Collection Runner:**
  ![Collection Runner](screenshots/runner_summary.png)

---

### 7. DANH MỤC CÁC TẬP TIN NỘP BÀI TRONG THƯ MỤC LAB07
1. `jsonplaceholder-postman-lab.postman_collection.json`
2. `jsonplaceholder-environment.postman_environment.json`
3. `BaoCao_Lab07_Postman.pdf`
4. `BaoCao_Lab07_Postman.html`
5. `BaoCao_Lab07_Postman.md`
6. Thư mục `screenshots/` chứa các ảnh chụp màn hình minh chứng kết quả chạy Postman.
