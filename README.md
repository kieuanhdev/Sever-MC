# Máy chủ Minecraft (Paper 1.20.4) - Triển khai bằng Docker

Dự án này chứa cấu hình tự động để chạy một máy chủ Minecraft nhẹ, tối ưu và hỗ trợ người chơi Crack (Offline Mode), quản lý dễ dàng bằng Docker.

## Cấu trúc dự án
- `docker-compose.yml`: File cấu hình chính của máy chủ.
- `.gitignore`: Đã được thiết lập để KHÔNG đẩy thư mục `data/world` lên Git (tránh làm phình to repo), nhưng VẪN cho phép đẩy các plugin trong thư mục `data/plugins/`.

## Hướng dẫn triển khai lên VPS/Server

**Bước 1: Kéo code về Server**
Kết nối SSH vào server của bạn và clone repository này về:
```bash
git clone <đường_dẫn_repo_của_bạn>
cd <tên_thư_mục>
```

**Bước 2: (Tự động) Cài đặt Plugin**
1. Nhờ cấu hình tự động trong `docker-compose.yml`, các plugin thiết yếu (WebAdmin, SkinsRestorer, Essentials, Chunky, BlueMap, GSit, TreeAssist) sẽ được Docker **tự động tải về cài đặt** ở lần chạy đầu tiên. Bạn KHÔNG cần phải copy thủ công!
2. Cổng `8088` đã được mở sẵn cho WebAdmin. Truy cập vào `http://IP_Server:8088` để quản lý. (Lưu ý: Mật khẩu mặc định và user của WebAdmin sẽ hiển thị trong log `docker logs -f paper-mc-server`).
3. Cổng `8100` đã được mở cho Bản đồ BlueMap. Khi nào chơi game và muốn xem bản đồ 3D, hãy truy cập vào `http://IP_Server:8100`.

**Bước 3: Khởi chạy máy chủ**
Đảm bảo máy chủ (VPS) của bạn đã được cài đặt Docker và Docker Compose.
```bash
docker-compose up -d
```
Lần đầu tiên khởi chạy, Docker sẽ mất vài phút để tải image và các file cần thiết của Minecraft.

**Bước 4: Cấu hình tên miền Cloudflare (Để bạn bè kết nối)**
1. Truy cập Cloudflare > Quản lý DNS của tên miền.
2. Tạo bản ghi (Record) loại **A**.
3. Name: `mc` (hoặc tên tùy thích).
4. IPv4 address: **IP của Server/VPS**.
5. **Proxy status**: Chuyển sang biểu tượng Đám mây màu **Xám** (DNS Only) -> BẮT BUỘC.
6. Nhấn Save. Người chơi có thể kết nối bằng địa chỉ: `mc.tenmiencuaban.com`

## Quản lý máy chủ
- **Xem log server:**
  ```bash
  docker logs -f paper-mc-server
  ```
- **Khởi động lại server:**
  ```bash
  docker-compose restart
  ```
- **Tắt server:**
  ```bash
  docker-compose down
  ```
