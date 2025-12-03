# Scheduler

Quay lai phiên bản [Tiếng Anh](./README.md)

Đây là một dự án minh họa một ứng dụng lập lịch được xây dựng bằng kiến trúc microservices. Dự án được tổ chức thành nhiều service, mỗi service có một trách nhiệm riêng biệt.

## Kiến trúc

Ứng dụng bao gồm các microservice sau:

-   **`discovery-service`**: Đóng vai trò là một service registry (Eureka) cho tất cả các microservice khác tự đăng ký, cho phép các service tự tìm và giao tiếp với nhau một cách linh động.

-   **`api-gateway`**: Là điểm vào duy nhất cho tất cả các yêu cầu từ client. Nó định tuyến các yêu cầu đến microservice thích hợp và chịu trách nhiệm cho các vấn đề chung như bảo mật và ghi log.

-   **`auth-service`**: Xử lý việc xác thực và phân quyền người dùng, chịu trách nhiệm cấp và xác thực các token bảo mật.

-   **`user-service`**: Quản lý tất cả thông tin và hoạt động liên quan đến người dùng như giáo viên và học sinh.

-   **`school-service`**: Quản lý dữ liệu liên quan đến trường học, có thể bao gồm lịch học, môn học, và lớp học.

## Công nghệ sử dụng

-   **Backend**: Java
-   **Framework**: Spring Boot & Spring Cloud
-   **Công cụ Build**: Apache Maven
-   **Kiến trúc**: Microservices
-   **Cơ sở dữ liệu**: MySQL

## Bắt đầu

Để chạy ứng dụng trên máy local, cần cài đặt Java (JDK) và Maven.

### Clone Repository

Repository này chứa các submodule cho mỗi microservice. Để clone repository và tất cả các submodule:

```bash
git clone --recurse-submodules https://github.com/HLoc26/scheduler.git
```

Nếu đã clone repository mà không khởi tạo các submodule, chạy lệnh sau tại root:

```bash
git submodule update --init --recursive
```

### Cập nhật Submodule

Để lấy những thay đổi mới nhất cho dự án chính và tất cả các submodule:
```bash
git pull --recurse-submodules
```

### Yêu cầu

-   Java JDK 17 trở lên
-   Apache Maven

### Docker Compose
Kafka và MySQL có thể chạy bằng docker-compose:

**Kafka**

- Chạy chế độ KRaft, expose các port:
  - 9092 (PLAINTEXT)
  - 9093 (Controller)

**MySQL**

- Đã có root password:
  - Cổng nội bộ: 3306
  - Expose: 3300

### Build các Service

Mỗi service là một dự án Maven độc lập. Để build tất cả các service, di chuyển đến thư mục gốc của mỗi service và chạy script Maven wrapper.

Đối với mỗi service (`discovery-service`, `api-gateway`, `auth-service`, `user-service`, `school-service`):

```bash
# Di chuyển đến thư mục của service (ví dụ: school-service)
cd ./school-service

# Trên Windows
./mvnw.cmd clean install

# Trên macOS/Linux
./mvnw clean install
```

### Chạy ứng dụng

Các service nên được khởi động theo thứ tự:

1.  **Chuẩn bị biến môi trường**: Tạo một file `.env` với các biến giống trong `.env.example`.
2.  **Khởi chạy Docker**: Chạy lệnh `docker-compose up -d` để chạy Kafka và MySQL.
3.  **Khởi động `discovery-service`**: Đây phải là service được khởi chạy đầu tiên.
4.  **Khởi động các service khác**: Sau khi discovery service đang chạy, khởi chạy `auth-service`, `user-service`, `school-service`.
5.  **Khởi động `api-gateway`**: Gateway nên được khởi động sau khi các service mà nó phụ thuộc đã hoạt động.

Để chạy một service, hãy di chuyển đến thư mục của nó và sử dụng plugin Spring Boot Maven:

```bash
./mvnw.cmd spring-boot:run
```