Bài 6: CI/CD Pipeline Optimization & Caching

Trong bài này, em tạo một dự án Maven riêng tên Bai6. Dự án có class Calculator và các unit test bằng JUnit Jupiter.

Em cấu hình GitHub Actions trong file .github/workflows/maven-cache-ci.yml. Workflow được kích hoạt khi có push hoặc pull_request vào nhánh main.

Điểm tối ưu chính là trong bước actions/setup-java@v4, em thêm cấu hình:

cache: maven

Cấu hình này cho phép GitHub Actions cache Maven dependencies trong thư mục ~/.m2/repository. Ở lần chạy đầu tiên, workflow cần tải dependency từ Maven Central nên thời gian chạy có thể lâu hơn. Sau lần chạy đầu tiên, cache được lưu lại. Ở lần chạy thứ hai, workflow khôi phục dependency từ cache, giúp giảm thời gian tải dependency và tối ưu CI/CD pipeline.

Lệnh build được sử dụng trong workflow:

mvn -B clean verify

So sánh kết quả:
- Lần chạy 1: thời gian chạy: ... giây
- Lần chạy 2: thời gian chạy: ... giây
- Nhận xét: lần chạy thứ hai nhanh hơn hoặc log cho thấy Maven dependencies được khôi phục từ cache.

Bằng chứng cache:
Trong log của bước "Set up JDK 17 with Maven cache", GitHub Actions hiển thị thông tin cache được khôi phục, ví dụ "Cache restored from key" hoặc "Cache hit occurred on the primary key".