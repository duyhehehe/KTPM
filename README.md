Nguyễn Hoàng Duy
21020755

# Untitled

## 1.	Docker, docker-composer là gì?

Docker là một công cụ để tạo, triển khai và chạy ứng dụng dễ dàng hơn với viedjc sủ dụng các containers. Container cho phép đóng gói ứng dụng cùng với các thành phần cần thiết như thư viện, môi trường và gửi chúng đi dưới dạng đóng gói.

Docker-composer là công cụ chạy các ứng dụng Docker sử dụng nhiều container cùng lúc (multi-container)

## 2. Linux, Unix, BSD, *nix, macOS

Linux là hệ điều hành máy tính được phát triển dựa trên hệ điều hành Unix, được viết bằng ngôn ngữ C.

BSD, viết tắt của Berkeley Software Distribution là hệ điều hành dẫn xuất từ Unix.

*nix là tập hợp các hệ hệ điều hành giống Unix như Xenix, Linux, BSD,…

Unix là một hệ điều hành và là một họ hệ điều hành được phát triển bởi AT&T Bell Labs và các tổ chức khác từ năm 1960 đến 1980.

MacOS xuất phát từ Macintosh operating system. Đây là một hệ điều hành được phát triển dựa trên Unix, sử dụng riêng cho các sản phẩm của Apple.

## 3. Alpine và Ubuntu

Alpine Linux và Ubuntu là hai hệ điều hành Linux phổ biến trong cộng đồng phần mềm mã nguồn mở và phát triển phần mềm.

Alpine Linux là một bản phân phối Linux siêu nhẹ và đơn giản, được thiết kế đặc biệt để có hiệu suất tốt trên hệ thống có tài nguyên hạn chế như container và thiết bị nhúng.

Ubuntu là một hệ điều hành Linux phổ biến dựa trên Debian, được phát triển và duy trì bởi Canonical Ltd.

So sánh Alpine và Ubuntu

- Kích thước
    - **Alpine:** Nhỏ hơn nhiều so với Ubuntu. Một cài đặt tối thiểu của Alpine chỉ chiếm vài chục MB, trong khi Ubuntu cần vài GB.
    - **Ubuntu:** Lớn hơn Alpine nhiều. Cài đặt tối thiểu cần vài GB dung lượng.
- Hiệu suất
    - **Alpine:** Nhẹ và nhanh hơn do kích thước nhỏ
    - **Ubuntu:** Nặng hơn và có thể chậm hơn Alpine, đặc biệt trên máy tính cấu hình thấp.
- Khả năng tương thích
    - **Alpine:** Ít tương thích hơn với phần mềm do sử dụng hệ thống quản lý gói riêng (apk).
    - **Ubuntu:** Tương thích tốt hơn với phần mềm do sử dụng hệ thống quản lý gói phổ biến (apt).
- Cộng đồng
    - **Alpine:** Cộng đồng nhỏ hơn Ubuntu nhiều.
    - **Ubuntu:** Cộng đồng rất lớn, hỗ trợ phong phú

## 4. VNC

**Định nghĩa:**

VNC là viết tắt của Virtual Network Computing, là một hệ thống chia sẻ màn hình từ xa. Nó cho phép truy cập và điều khiển máy tính khác từ xa qua mạng.

**Cách thức hoạt động:**

- **Máy chủ VNC:** Chạy trên máy tính mà bạn muốn truy cập (máy tính từ xa).
- **Máy khách VNC:** Chạy trên máy tính mà bạn sử dụng để truy cập máy tính từ xa (máy tính cục bộ).

**Ứng dụng:**

- Hỗ trợ kỹ thuật từ xa.
- Truy cập máy tính từ xa.

**Ưu điểm:**

- Dễ sử dụng: VNC có giao diện đơn giản và dễ sử dụng.
- Miễn phí: VNC là phần mềm miễn phí và mã nguồn mở.

**Nhược điểm:**

- Hiệu suất: VNC phụ thuộc nhiều vào tốc độ mạng
- Bảo mật: VNC có thể bị tấn công nếu không được bảo mật đúng cách.

**Các phần mềm VNC phổ biến:** TigerVNC, RealVNC, x11VNC

## 5. Bài tập thực hành

1. Viết Dockerfile

```docker
# Sử dụng Ubuntu làm image cơ sở
FROM ubuntu:latest

# Cài đặt các gói phần mềm cần thiết
FROM ubuntu:20.04

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && \
    apt-get install -y \
    openssh-server \
    xfce4 \
    xfce4-goodies \
    tightvncserver \
    xfonts-base \
    x11vnc \
    xvfb \
    firefox

RUN echo 'root:123456' | chpasswd && \
    mkdir -p ~/.vnc && \
    x11vnc -storepasswd 123456 ~/.vnc/passwd

RUN echo '#!/bin/bash' >> /startup.sh && \
    echo 'service ssh start' >> /startup.sh && \
    echo 'mkdir -p ~/.vnc' >> /startup.sh && \
    echo 'Xvfb :1 -screen 0 1024x768x16 & DISPLAY=:1.0 startxfce4 &' >> /startup.sh && \
    echo 'x11vnc -display :1 -forever -passwd 123456 &' >> /startup.sh && \
    echo 'sleep infinity' >> /startup.sh && \
    chmod +x /startup.sh

EXPOSE 22
EXPOSE 5900

CMD ["/startup.sh"]

```
Sử dụng X11VNC
1. Build với docker

Sử dụng lệnh trong giao diện cmd tại thư mục chứa Dockerfile

```bash
docker build -t ubuntu-vnc .
```


2. Khởi động contaner

Chạy ở cổng 5900
```bash
docker run -d -p 2222:22 -p 5900:5900 --name ubuntu-vnc-container ubuntu-vnc
```

3. Truy cập vào VNC server bằng VNC Viewer

Sử dụng ứng dụng RealVNC


Sử dụng RealVNC, điền đường dẫn localhost:5900
![image](https://github.com/duyhehehe/KTPM/assets/112858967/17ce3995-b031-48cc-83f1-f091687529f8)

