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
# Sử dụng một base image
FROM ubuntu:latest

# Cài đặt SSH server và các công cụ hỗ trợ
RUN apt update && apt install -y openssh-server
RUN mkdir /var/run/sshd

# Cấu hình SSH server
RUN echo 'root:123456' | chpasswd
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# Loại bỏ cảnh báo "Could not load host key"
RUN ssh-keygen -A

# Mở cổng SSH
EXPOSE 22

# Khởi động SSH server khi container được khởi chạy
CMD ["/usr/sbin/sshd", "-D"]
```

1. Build với docker

Sử dụng lệnh trong giao diện cmd tại thư mục chứa Dockerfile

```bash
docker build -t test_vnc .
```

1. Chạy trong Docker Desktop

Tìm image có tên test_vnc và khởi động 

![Untitled](Untitled%20be43c07c4ad54807a1cee2255068b07a/Untitled.png)

1. Cài đặt các package
- Cài đặt Desktop Environment

Sử dụng xfce

```bash
apt install xfce4 xfce4-goodies
```

Chọn lightdm

- Cài đặt VNC server

Sử dụng TigerVNC

```bash
apt install tigervnc-standalone-server
```

Khởi động vncserver:

```bash
vncserver
```

Chọn mật khẩu

```
You will require a password to access your desktops.

Password:
Verify:
```

1. Cấu hình VNC server

Hiện tại có một vnc server đang chạy trên cổng 5901, đầu tiên phải tắt nó .

```bash
vncserver -kill :1
```

Cài đặt trình soạn thảo, có thể sử dụng vim hoặc nano. Trong bài này sử dụng nano

```bash
apt install nano
```

Tạo mới tệp xstartup

```bash
nano ~/.vnc/xstartup
```

Dán đoạn text vào

```
#!/bin/sh

# Start up the standard system desktop
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS

/usr/bin/startxfce4

[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
x-window-manager &
```

Để lưu lại, sử dụng tổ hợp phím Ctrl-O, Enter. Ctrl-X để thoát khỏi trình soạn thảo

Cấp quyền khởi động cho tệp xstartup

```bash
chmod +x ~/.vnc/xstartup
```

Khởi động VNC server

```bash
vncserver -localhost no :1
```

1. Truy cập vào VNC server bằng VNC Viewer

Sử dụng ứng dụng RealVNC

Container hiện tại đang được chạy với ip 172.17.0.3

![Untitled](Untitled%20be43c07c4ad54807a1cee2255068b07a/Untitled%201.png)

Sử dụng RealVNC, điền đường dẫn 172.17.0.3:5901

![Untitled](Untitled%20be43c07c4ad54807a1cee2255068b07a/Untitled%202.png)