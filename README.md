# HƯỚNG DẪN SỬ DỤNG BỘ BÀI THỰC HÀNH LAB 07 - POSTMAN API TESTING

## 1. Cấu trúc thư mục Lab 07 (`E:\CODE\CNPHANMEM\Lab07`)
```
E:\CODE\CNPHANMEM\Lab07\
├── jsonplaceholder-postman-lab.postman_collection.json   # File Collection Postman v2.1.0
├── jsonplaceholder-environment.postman_environment.json  # File Cấu hình Environment Postman
├── BaoCao_Lab07_Postman.pdf                              # Báo cáo thực hành bản PDF (A4)
├── BaoCao_Lab07_Postman.html                             # Báo cáo thực hành bản HTML
├── BaoCao_Lab07_Postman.md                               # Báo cáo thực hành bản Markdown
└── screenshots/                                          # Thư mục chứa ảnh minh chứng kết quả
    ├── request_1_get_posts.png                           # Minh chứng Bài 1 (GET /posts)
    ├── request_2_get_single_post.png                     # Minh chứng Bài 2 (GET /posts/1)
    ├── request_3_get_comments_by_postid.png              # Minh chứng Bài 3 (GET /comments?postId=1)
    ├── request_4_post_create_new_post.png                # Minh chứng Bài 4 (POST /posts)
    └── runner_summary.png                                # Minh chứng Collection Runner (Mở rộng)
```

## 2. Cách Import vào Postman
1. Mở ứng dụng **Postman**.
2. Nhấn nút **Import** (ở góc trên bên trái màn hình Postman).
3. Kéo thả file `jsonplaceholder-postman-lab.postman_collection.json` vào cửa sổ Import hoặc nhấn **Files** rồi chọn file.
4. (Tùy chọn) Import tiếp file `jsonplaceholder-environment.postman_environment.json` vào tab **Environments**.
5. Nhấn **Import** để hoàn tất.

## 3. Cách chạy kiểm thử
- **Chạy từng request lẻ:** Chọn một request trong danh sách (`GET Posts List`, `GET Single Post`, v.v.) rồi bấm nút **Send**. Kiểm tra kết quả phản hồi ở khung bên phải và tab **Test Results**.
- **Chạy tự động toàn bộ Collection (Runner):**
  1. Click vào tên Collection `jsonplaceholder-postman-lab`.
  2. Bấm nút **Run** ở góc trên bên phải.
  3. Chọn số lượt lặp (Iterations: ví dụ 5) rồi bấm **Run jsonplaceholder-postman-lab**.
  4. Postman sẽ tự động thực thi toàn bộ kịch bản và báo cáo số lượng Test Passed/Failed.
