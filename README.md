<p align="center">
  <img src="docs/images/sukiflow-logo.png" alt="SukiFlow" width="120">
</p>

<h1 align="center">SukiFlow</h1>

<p align="center">
  <strong>Giải pháp tự động hoá tạo ảnh &amp; video AI hàng loạt tối ưu trên các nền tảng hàng đầu.</strong>
</p>

<table align="center">
  <tr>
    <td align="center" width="260">
      <img src="https://img.shields.io/badge/Windows-10%20%2F%2011-0078D4?style=flat-square&logo=windows11&logoColor=white" alt="Windows 10/11"><br>
      <strong>SukiFlow cho Windows</strong><br>
      <sub>64-bit · Bộ cài đặt EXE</sub><br><br>
      <a href="https://github.com/suki-app/sukiflow-doc/releases/download/v2.0.0/SukiFlow_Setup_v2.0.0.exe"><img src="https://img.shields.io/badge/TẢI_BẢN_CÀI-0078D4?style=for-the-badge&logo=windows11&logoColor=white" alt="Tải bản cài SukiFlow cho Windows"></a>
    </td>
    <td align="center" width="260">
      <img src="https://img.shields.io/badge/macOS-Apple%20Silicon-10B981?style=flat-square&logo=apple&logoColor=white" alt="macOS Apple Silicon"><br>
      <strong>SukiFlow cho Mac</strong><br>
      <sub>M1 · M2 · M3 · M4</sub><br><br>
      <a href="https://github.com/suki-app/sukiflow-doc/releases/download/v2.0.0/SukiFlow_macOS_v2.0.0_arm64.dmg"><img src="https://img.shields.io/badge/TẢI_BẢN_CÀI-10B981?style=for-the-badge&logo=apple&logoColor=white" alt="Tải bản cài SukiFlow cho Apple Silicon"></a>
    </td>
    <td align="center" width="260">
      <img src="https://img.shields.io/badge/macOS-Intel-5B5BD6?style=flat-square&logo=intel&logoColor=white" alt="macOS Intel"><br>
      <strong>SukiFlow cho Mac</strong><br>
      <sub>Intel · x86_64</sub><br><br>
      <img src="https://img.shields.io/badge/BẢN_DEMO-SẮP_PHÁT_HÀNH-6B7280?style=for-the-badge&logo=intel&logoColor=white" alt="Bản Intel sắp phát hành">
    </td>
  </tr>
</table>

<p align="center">
  <a href="README_EN.md">English</a>
  ·
  <a href="https://github.com/suki-app/sukiflow-doc/releases/latest">Phiên bản mới nhất</a>
  ·
  <a href="#cài-đặt">Hướng dẫn cài đặt</a>
  ·
  <a href="#quản-lý-proxy--đổi-ip">Quản lý Proxy Hub</a>
  ·
  <a href="#bảng-giá--gói-cước">Gói cước &amp; Bảng giá</a>
  ·
  <a href="#chương-trình-giới-thiệu-nhận-hoa-hồng-affiliate">Giới thiệu nhận hoa hồng</a>
  ·
  <a href="WEBHOOK_API.md">Tài liệu Webhook API</a>
</p>

> Phiên bản hiện tại: **SukiFlow v2.0.0**. SukiFlow là ứng dụng desktop dành cho Windows và macOS. Các tính năng phụ thuộc vào dịch vụ bên thứ ba có thể thay đổi theo tài khoản, khu vực, hạn mức và chính sách của từng nền tảng.

---

## Tổng quan

SukiFlow tập trung nhiều quy trình tạo ảnh và video AI hàng đầu thế giới vào một ứng dụng duy nhất. Bạn có thể nhập prompt hàng loạt, quản lý nhiều tài khoản liên kết, gán proxy / IPv6 riêng biệt cho từng tài khoản, chạy tác vụ song song đa luồng thông minh, lưu phiên làm việc, chuyển kết quả giữa các công cụ mà không phải tổ chức file thủ công, và tích hợp tự động hoá qua máy chủ Webhook API tích hợp sẵn.

### Các nhóm chức năng chính:

- **Google Flow:** Tạo ảnh và video hàng loạt với Flow Image (Nano Banana 2/Pro, 1K/2K/4K, xem 4 ảnh 1 dòng) và Flow Video (Omni Flash 360p/720p, Veo 3.1, chế độ Khung hình &amp; Ảnh tham chiếu Ingredients, tự động fallback sang Google Vids, tự động tắt Flow Agent mode, tự động click nút đăng nhập).
- **Grok AI Studio:** Hỗ trợ tạo ảnh và video AI chân thực bằng mô hình Grok, tùy chỉnh tỉ lệ khung hình, phong cách và không bị kiểm duyệt gò bó.
- **ChatGPT Image:** Tạo ảnh qua ChatGPT với cả 2 chế độ Văn bản → Ảnh (Text-to-Image) và Ảnh → Ảnh (Image-to-Image, tối đa 5 ảnh tham chiếu), tuỳ chỉnh độ nét và phong cách.
- **Muse AI:** Hỗ trợ cả tạo video (16:9, 9:16, 5–10s, MP4/WebM/MOV) và tạo ảnh với tối đa 5 ảnh tham chiếu, kết nối qua proxy độc lập.
- **Google Vids:** Tạo video (3–15s, 16:9/9:16/1:1, tối đa 3 ảnh tham chiếu) và tạo ảnh Vids Image theo nhiều phong cách chuyên nghiệp.
- **Google Pics:** Tạo ảnh hàng loạt chất lượng cao, xuất ảnh gốc, 2K hoặc 4K, hỗ trợ ảnh tham chiếu và nhận diện nhân vật tự động.
- **Auto Flow (Video Builder):** Dựng video hoàn chỉnh từ ảnh, ghép video dài, dựng Stock Video từ kịch bản, Draw Video và Jetlag Video.
- **Quản lý Proxy Hub (IPv6 &amp; Proxy ngoài):** Tự động tạo và gán địa chỉ IPv6 riêng biệt từ pool máy local hoặc gán Proxy ngoài HTTP/SOCKS5 cho từng tài khoản, tránh trùng IP, chống checkpoint an toàn.
- **Gói cước &amp; Nâng cấp Plan:** Bảng so sánh rõ ràng các gói Basic (Miễn phí), Pro và Ultra; hỗ trợ thanh toán tự động qua mã VietQR PayOS kích hoạt tức thì.
- **Quà tặng đặc biệt cho gói ULTRA:** Tặng mã Gift Code bản quyền **SukiMail Free** (trị giá 100.000đ/tháng) giúp nuôi và quản lý hộp thư email tự động.
- **Chương trình Affiliate (Giới thiệu nhận hoa hồng):** Nhận link giới thiệu riêng, hưởng hoa hồng hấp dẫn định kỳ khi bạn bè đăng ký và thanh toán.
- **Linked Accounts:** Quản lý độc lập và tự động luân phiên tài khoản Google Flow, Google Vids, Google Pics, Muse AI, ChatGPT và Grok.
- **Thư viện sáng tạo:** Lưu trữ Character, Visual, Narrative và Voice preset để tái sử dụng xuyên suốt các dự án.
- **Máy chủ Webhook API &amp; Test Playground:** Tích hợp máy chủ REST Webhook API độc lập ngay trong ứng dụng (port 10790) kết hợp trang playground `/webhook-test` để tích hợp với n8n, Make, Zapier và các hệ thống backend.
- **Quản lý tác vụ thông minh:** Chạy ngay, chạy các mục đã chọn, chạy lại mục lỗi, hàng đợi FIFO, cơ chế chống nghẽn và khôi phục phiên cũ.

![Giao diện Flow Image của SukiFlow](docs/images/flow-image.png)

## Mục lục

- [Cài đặt](#cài-đặt)
- [Bắt đầu sử dụng](#bắt-đầu-sử-dụng)
- [Quản lý Proxy &amp; Đổi IP (Proxy Hub)](#quản-lý-proxy--đổi-ip)
- [Bảng giá &amp; Gói cước (Plans)](#bảng-giá--gói-cước)
- [Quà tặng SukiMail bản quyền cho gói ULTRA](#quà-tặng-sukimail-bản-quyền-cho-gói-ultra)
- [Chương trình Giới thiệu nhận hoa hồng (Affiliate)](#chương-trình-giới-thiệu-nhận-hoa-hồng-affiliate)
- [Google Flow](#google-flow)
- [Grok AI Studio](#grok-ai-studio)
- [ChatGPT Image](#chatgpt-image)
- [Muse AI](#muse-ai)
- [Google Vids](#google-vids)
- [Google Pics](#google-pics)
- [Auto Flow (Video Builder)](#auto-flow-video-builder)
- [Thư viện Character, Visual, Narrative và Voice](#thư-viện-character-visual-narrative-và-voice)
- [Máy chủ Webhook API](#máy-chủ-webhook-api)
- [Dữ liệu và quyền riêng tư](#dữ-liệu-và-quyền-riêng-tư)
- [Khắc phục sự cố](#khắc-phục-sự-cố)

---

## Cài đặt

### Yêu cầu hệ thống

| Nền tảng | Phiên bản đề xuất | Gói cài đặt |
|---|---|---|
| Windows | Windows 10/11 64-bit | `SukiFlow_Setup_v2.0.0.exe` |
| macOS Apple Silicon | macOS 12 trở lên | `SukiFlow_macOS_v2.0.0_arm64.dmg` |
| macOS Intel | Đang hoàn thiện bản demo | Sắp phát hành |

Gói cài đặt chính thức đã bao gồm đầy đủ các thành phần cần thiết để chạy ứng dụng. Người dùng cuối không cần tự cài Python hoặc FFmpeg.

### Windows

1. Tải **SukiFlow Setup** bằng nút ở đầu trang.
2. Đóng phiên bản SukiFlow đang chạy trước khi cài hoặc cập nhật.
3. Mở `SukiFlow_Setup_v2.0.0.exe` và hoàn tất trình cài đặt.
4. Khởi chạy SukiFlow từ Start Menu hoặc shortcut trên Desktop.
5. Nếu Windows SmartScreen xuất hiện, chọn **More info** → **Run anyway**. Chỉ tải bộ cài từ trang Releases chính thức này.

### macOS

1. Tải `SukiFlow_macOS_v2.0.0_arm64.dmg`.
2. Mở file DMG và kéo **SukiFlow.app** vào thư mục **Applications**.
3. Ở lần mở đầu tiên, bấm chuột phải vào SukiFlow → **Open** → **Open**.
4. Nếu macOS vẫn chặn ứng dụng, vào **System Settings → Privacy & Security** và chọn **Open Anyway** cho SukiFlow.

Nếu macOS báo ứng dụng bị cách ly và bạn chắc chắn file được tải từ Releases chính thức, có thể chạy lệnh:

```bash
xattr -dr com.apple.quarantine "/Applications/SukiFlow.app"
```

> Bản macOS hiện phát hành cho máy Apple Silicon (`arm64`). Hãy kiểm tra loại chip tại **Apple menu → About This Mac** trước khi tải.

---

## Bắt đầu sử dụng

1. Mở SukiFlow và chọn **LOGIN GOOGLE** để đăng nhập tài khoản SukiFlow của bạn.
2. Vào **Setting → Linked Accounts**.
3. Chọn dịch vụ cần dùng rồi bấm **Thêm tài khoản**:
   - Google Flow
   - Muse AI
   - ChatGPT
   - Grok AI
   - Google Vids
   - Google Pics
4. Cửa sổ trình duyệt tự động mở ra. Đối với Google Flow, SukiFlow sẽ tự động click nút bắt đầu để mở thẳng trang đăng nhập tài khoản Google. Hãy đăng nhập tài khoản của bạn và chờ trong giây lát để hệ thống lưu phiên.
5. *(Tùy chọn)* Bấm nút **🌐 Quản lý Proxy** để gán địa chỉ IPv6 hoặc Proxy ngoài cho từng tài khoản nhằm tránh bị giới hạn IP.
6. Quay lại tab chức năng mong muốn, nhập danh sách prompt và chọn **Chạy ngay**.

> Khi SukiFlow đang tạo ảnh hoặc video, không đóng cửa sổ trình duyệt tự động được mở. Đây là phiên làm việc mà ứng dụng sử dụng để điều khiển tác vụ và nhận kết quả.

---

## Quản lý Proxy & Đổi IP

Khi vận hành nhiều tài khoản AI cùng lúc trên một máy, việc dùng chung một địa chỉ IP công cộng (IPv4/IPv6) dễ dẫn đến tình trạng bị các nền tảng (Google, OpenAI, Muse...) đánh cờ spam, khóa tài khoản hoặc giới hạn tần suất tạo (rate limit).

SukiFlow 2.0 tích hợp trung tâm **Proxy Hub** mạnh mẽ ngay trong giao diện Quản lý tài khoản:

### 1. Gán IPv6 nội bộ tự động (Local IPv6 Pool)
- **Cơ chế hoạt động:** SukiFlow tự động dò quét dải mạng `/64` của nhà mạng trên máy bạn, trích xuất hàng chục địa chỉ IPv6 công cộng khả dụng và khởi chạy một proxy server nội bộ siêu tốc.
- **Gán IP riêng biệt:** Mỗi tài khoản (Google Flow, Muse AI, ChatGPT...) được cấp phát một cổng local riêng (`:28001`, `:28002`, `:28003`...) ứng với một địa chỉ IPv6 thực tế duy nhất ra ngoài Internet.
- **Không tốn chi phí:** Tận dụng 100% đường truyền Internet sẵn có, không cần bỏ tiền mua proxy hàng tháng.
- **Tính năng một chạm:**
  - `⚡ Tự động gán tất cả`: Gán tự động cho mọi tài khoản chưa có IP.
  - `Đổi`: Đổi sang một địa chỉ IPv6 khác trong pool còn trống chỉ trong 1 giây.
  - `Test`: Kiểm tra kết nối và hiển thị địa chỉ IP thực tế mà máy chủ Google/Muse nhìn thấy.
  - `Bỏ`: Hủy gán proxy khi muốn quay về IP mặc định của máy.

### 2. Gán Proxy ngoài (External HTTP / SOCKS5)
- Hỗ trợ thêm Proxy mua ngoài chuẩn `HTTP`, `HTTPS` hoặc `SOCKS5`.
- Hỗ trợ cả 2 định dạng phổ biến:
  - `IP:Port` (Proxy mở hoặc IP tĩnh xác thực theo địa chỉ IP)
  - `IP:Port:Username:Password` (Proxy có tài khoản và mật khẩu bảo mật)
- Gán độc lập cho từng profile tài khoản hoặc nhập theo danh sách để tự động chia đều.
- Trình duyệt tự động gắn cấu hình proxy an toàn, che giấu dấu vân tay tự động hóa (`navigator.webdriver`).

---

## Bảng giá & Gói cước

SukiFlow cung cấp các gói dịch vụ linh hoạt, phù hợp từ người dùng cá nhân mới bắt đầu đến các studio sản xuất nội dung quy mô lớn:

| Tính năng | GÓI BASIC | GÓI PRO 🔥 | GÓI ULTRA 👑 |
| :--- | :---: | :---: | :---: |
| **Giá cước (tháng)** | **0đ** (Miễn phí vĩnh viễn) | **79.000đ** *(gốc 100k)* | **149.000đ** *(gốc 200k)* |
| **Số tài khoản liên kết (Linked Accounts)** | **5** tài khoản (tổng toàn app) | **10** tài khoản | **100** tài khoản |
| **Số luồng chạy đồng thời (Concurrency)** | 1 luồng | **4 luồng song song** | **4 luồng song song** |
| **Giới hạn Prompts** | 5 prompt / lượt chạy | **Không giới hạn** | **Không giới hạn** |
| **Dịch vụ mở khóa** | Google Flow, Muse AI | **Mở khóa tất cả** (Flow, Muse, ChatGPT, Grok, Vids, Pics) | **Mở khóa tất cả** toàn bộ dịch vụ |
| **Độ phân giải xuất ảnh** | 1K | **2K / 4K sắc nét** | **2K / 4K sắc nét** |
| **Xóa Watermark / Logo tự động** | ✕ | **Có** | **Có** |
| **Model Fallback dự phòng** | ✕ | **Có** (Chuyển tự động khi lỗi) | **Có** |
| **Máy chủ Webhook API (HTTP / n8n / Make)** | ✕ | ✕ | **Độc quyền ULTRA** |
| **Quà tặng SukiMail Free** | ✕ | ✕ | **🎁 Tặng mã bản quyền SukiMail (100k/tháng)** |
| **Hỗ trợ kỹ thuật** | Cộng đồng | Ưu tiên | **VIP 1-1 trực tiếp** |

### Ưu đãi chu kỳ thanh toán
- **Thanh toán 6 tháng:** Giảm thêm **10%** tổng hóa đơn.
- **Thanh toán 1 năm:** Tiết kiệm tới **20%** tổng hóa đơn.

### Thanh toán VietQR PayOS tự động
- Tích hợp cổng thanh toán trực tiếp qua VietQR.
- Sau khi chọn gói và kỳ hạn, hệ thống tạo mã QR động. Bạn chỉ cần mở app ngân hàng quét mã, hệ thống sẽ **tự động kích hoạt gói cước trong vòng 3–5 giây** mà không cần chờ nhân viên xác nhận.
- Hỗ trợ cơ chế bảo lưu và quy đổi số dư còn lại (Credit PRO) khi nâng cấp từ PRO lên ULTRA.

---

## Quà tặng SukiMail bản quyền cho gói ULTRA

Khi đăng ký hoặc nâng cấp lên gói **SukiFlow ULTRA**, bạn sẽ nhận được một món quà đặc biệt:

> 🎁 **TẶNG MIỄN PHÍ MÃ BẢN QUYỀN SUKIMAIL (TRỊ GIÁ 100.000đ/THÁNG)**

### SukiMail là gì?
[SukiMail](https://github.com/suki-app) là phần mềm desktop chuyên dụng để quản lý, nuôi và kiểm tra hộp thư email tự động số lượng lớn:
- Tự động kiểm tra trạng thái sống/chết (Live check) hàng ngàn email.
- Tự động nhận mã OTP, link xác thực kích hoạt tài khoản.
- Tự động dọn dẹp thư rác, phân loại hộp thư thông minh.
- Phối hợp hoàn hảo với SukiFlow trong việc tạo và duy trì hàng loạt tài khoản Google Flow, Muse AI, ChatGPT...

### Cách nhận quà:
Sau khi thanh toán thành công gói ULTRA, mã Gift Code SukiMail sẽ xuất hiện ngay trong tab **Cài đặt → Gói tài khoản**. Bạn chỉ cần bấm nút **Sao chép mã** 📋 và nhập vào phần mềm SukiMail để kích hoạt hoàn toàn miễn phí.

---

## Chương trình Giới thiệu nhận hoa hồng (Affiliate)

SukiFlow cung cấp chương trình Đối tác Giới thiệu (Affiliate / Referral Program) giúp bạn tạo thêm nguồn thu nhập thụ động bền vững:

### Quyền lợi đối tác:
- **Mã giới thiệu & Đường link riêng:** Mỗi tài khoản SukiFlow sau khi đăng nhập đều được cấp một mã khách hàng và đường dẫn giới thiệu duy nhất.
- **Hoa hồng định kỳ:** Khi người được giới thiệu thanh toán các gói dịch vụ (PRO, ULTRA), bạn sẽ nhận được phần trăm hoa hồng trực tiếp.
- **Bảng điều khiển trực quan:**
  - Theo dõi tổng số lượt bạn bè đã đăng ký qua link.
  - Xem tổng hoa hồng tích lũy và số dư khả dụng theo thời gian thực.
  - Lịch sử giao dịch chi tiết, minh bạch.
- **Rút tiền nhanh chóng:** Yêu cầu chuyển khoản hoa hồng về tài khoản ngân hàng của bạn dễ dàng ngay trên giao diện.

Để mở bảng điều khiển, bấm vào biểu tượng **Hộp quà** 🎁 cạnh tên gói ở góc trên bên trái ứng dụng hoặc vào mục **Gói tài khoản → Giới thiệu nhận hoa hồng**.

---

## Google Flow

### Flow Image

Flow Image hỗ trợ tạo ảnh hàng loạt với hiệu suất vượt trội:

- Tỷ lệ: `16:9`, `9:16`, `1:1`, `4:3`, `3:4`.
- Model: **Nano Banana 2**, **Nano Banana 2 Lite** và **Nano Banana Pro**.
- Tạo từ 1 đến 4 ảnh cho mỗi prompt.
- Kích thước xuất ảnh: **1K**, **2K** hoặc **4K**.
- Giao diện lưới kết quả trực quan hiển thị **4 ảnh 1 dòng**.
- Chạy tối đa 4 luồng đồng thời, có cơ chế điều chỉnh thời gian chờ giữa các hàng.
- Nhập prompt trực tiếp hoặc import từ file TXT, CSV, XLSX.
- Gắn ảnh nhân vật tự động bằng cú pháp `@TênNhânVật`.
- Tuỳ chọn tự động chuyển sang model dự phòng (Model fallback) khi tài khoản gặp hạn chế.
- Tuỳ chọn làm sạch watermark tự động.
- Chuyển ảnh hoàn thành trực tiếp sang Flow Video hoặc Auto Flow chỉ với 1 cú click.

### Flow Video

Flow Video cung cấp khả năng tạo video mạnh mẽ với 2 chế độ tạo video từ ảnh:

- **Model đa dạng:**
  - **Omni 1.1 Flash:** hỗ trợ độ phân giải **360p** và **720p**, thời lượng linh hoạt **4s, 6s, 8s, 10s**.
  - **Veo 3.1 (Lite, Fast, Quality):** thời lượng 8 giây chuẩn điện ảnh.
- **Tỷ lệ khung hình:** `16:9` (ngang) và `9:16` (dọc).
- **2 chế độ Image-to-Video linh hoạt:**
  1. **Khung hình (Frames mode):** Nối tiếp chuyển động mượt mà từ ảnh đầu (Start frame) đến ảnh cuối (End frame).
  2. **Ảnh tham chiếu (Ingredients / R2V mode):** Tải lên tối đa **3 ảnh tham chiếu** để AI bám sát nhận diện nhân vật (`@Image1`, `@Image2`), phong cách hoặc bối cảnh.
- **Tự động dự phòng (Fallback to Google Vids):** Khi tài khoản Flow hết quota hoặc gặp sự cố kết nối, SukiFlow có thể tự động chuyển tiếp prompt và ảnh tham chiếu sang Google Vids để tiếp tục tạo video mà không làm gián đoạn chuỗi công việc.
- **Tự động hóa thông minh v2.0:**
  - Tự động tắt công tắc `Flow Agent mode` để tránh lỗi prompt khi tài khoản đang bật chế độ Agent.
  - Tự động đóng các dialog cập nhật / carousel giới thiệu tính năng của Google Flow.
  - Tự động click `Create with Google Flow` để mở thẳng màn hình đăng nhập Google khi thêm tài khoản.

---

## Grok AI Studio

Grok AI Studio tích hợp khả năng sinh ảnh và video đỉnh cao từ mô hình Grok:

- Tạo ảnh chân thực với mức độ tự do sáng tạo cao, không bị kiểm duyệt gắt gao.
- Hỗ trợ sinh ảnh từ văn bản hoặc kết hợp ảnh tham chiếu.
- Tự động hóa quá trình nhập prompt, nhận diện kết quả và tải file về máy.
- Hỗ trợ chạy đa luồng và liên kết nhiều tài khoản Grok.

---

## ChatGPT Image

Tích hợp trực tiếp khả năng tạo ảnh từ mô hình ChatGPT:

- **2 chế độ hoạt động:**
  - **Văn bản → Ảnh (Text-to-Image):** Tạo ảnh chi tiết từ mô tả văn bản.
  - **Ảnh → Ảnh (Image-to-Image):** Biến đổi ảnh nguồn dựa trên tối đa **5 ảnh tham chiếu**.
- **Chất lượng ảnh:** Tuỳ chọn `auto`, `high`, `medium`, `low`.
- **Tỷ lệ khung hình:** `16:9`, `1:1`, `3:2`, `2:3`, `9:16`.
- Tạo từ 1 đến 4 ảnh cho mỗi prompt, hỗ trợ thiết lập thời gian chờ (`wait_seconds`) giữa các ảnh để tránh giới hạn tần suất.
- Hỗ trợ thêm phong cách bổ sung qua trường **Style prompt**.

---

## Muse AI

Muse AI hỗ trợ tạo cả Video và Ảnh thông qua phiên trình duyệt tự động hoá:

- **Muse AI Video:** Tạo video với tỷ lệ `16:9` hoặc `9:16`, thời lượng khuyến nghị 10 giây (hỗ trợ điều khiển qua prompt 5s–10s), định dạng xuất linh hoạt **MP4**, **WebM**, **MOV**. Nhận tối đa **5 ảnh tham chiếu**.
- **Muse AI Image:** Tạo ảnh chất lượng cao theo prompt hoặc tối đa **5 ảnh tham chiếu**.
- **Tích hợp Proxy:** Hỗ trợ cấu hình Proxy IPv6 nội bộ hoặc Proxy ngoài cho từng tài khoản Muse để tránh bị chặn IP từ xa.
- Hỗ trợ cấu hình timeout tuỳ chỉnh (60s đến 900s) cho các video phức tạp.

---

## Google Vids

Google Vids tích hợp cả hai chức năng tạo Video và tạo Ảnh trong cùng một giao diện làm việc:

### Vids Video
- Tạo video từ văn bản hoặc từ tối đa **3 ảnh tham chiếu**.
- Thời lượng linh hoạt: **3, 4, 5, 6, 8, 10, 12 hoặc 15 giây** (mặc định 5s).
- Tỷ lệ khung hình: `16:9` (Landscape), `9:16` (Portrait) và `1:1` (Square).
- Độ phân giải: **720p** hoặc **1080p**.
- Tự động nhận diện nhân vật qua cú pháp `@TênNhânVật`.
- Chuyển nhanh video hoàn thành sang Auto Flow để ghép nối thành video dài.

### Vids Image
- Tạo ảnh đơn lẻ hoặc hàng loạt với các preset phong cách phong phú: *Photography, Vector art, Sketch, Watercolor, Cyberpunk, Background, v.v.*
- Tỷ lệ: `16:9`, `9:16`, `1:1`.
- Hỗ trợ ảnh tham chiếu và gán nhân vật `@TênNhânVật`.

---

## Google Pics

Google Pics cung cấp quy trình tạo ảnh hàng loạt độc lập với chất lượng sắc nét:

- Hỗ trợ các tỷ lệ ảnh phổ biến: `16:9`, `9:16`, `1:1`, `4:3`, `3:4`.
- Kích thước xuất ảnh: **Gốc (Original)**, **2K JPEG** hoặc **4K JPEG**.
- Tạo tối đa 4 ảnh trên mỗi prompt.
- Tự động nhận diện nhân vật qua cú pháp `@TênNhânVật`.
- Hỗ trợ tối đa 3 ảnh tham chiếu cho mỗi tác vụ.
- Quản lý hàng đợi, tự động thử lại khi lỗi và mở lại phiên làm việc cũ.

---

## Auto Flow (Video Builder)

Auto Flow là xưởng dựng video hoàn chỉnh, kết hợp các tài nguyên hình ảnh, video clip, giọng đọc AI và nhạc nền thành các tác phẩm video hoàn thiện cho YouTube, TikTok, Facebook Reels.

1. **Ảnh → Video (Montage):** Dựng video hoàn chỉnh từ thư mục ảnh, tạo chuyển động camera, chèn giọng đọc AI và phụ đề tự động.
2. **Videos → Video:** Ghép nối nhiều clip ngắn thành video dài liền mạch, xử lý âm thanh gốc và lồng nhạc nền.
3. **Stock Video:** Tự động sinh kịch bản theo chủ đề, tự tìm và tải video stock bản quyền từ **Pexels** / **Pixabay**, kết hợp tạo phân cảnh AI khi cần.
4. **Draw Video:** Dựng video vẽ hoạt họa (Hand-drawn / Sketch animation) sinh động theo kịch bản.
5. **Jetlag Video:** Module chuyển đổi video đa ngôn ngữ cho thị trường quốc tế.

---

## Thư viện Character, Visual, Narrative và Voice

- **Character (Nhân vật):** Lưu ảnh mẫu của nhân vật. Khi viết prompt chỉ cần gõ `@TênNhânVật`, SukiFlow tự đính kèm ảnh mẫu vào các nền tảng sinh ảnh/video.
- **Visual (Phong cách):** Lưu các preset ánh sáng, bố cục, mỹ thuật để tái sử dụng nhanh chóng.
- **Narrative (Kịch bản):** Lưu văn phong dẫn dắt, DNA câu chuyện hoặc sao chép phong cách từ link YouTube/TikTok.
- **Voice (Giọng đọc):** Quản lý các giọng đọc AI, cao độ, tốc độ cho Auto Flow.

---

## Máy chủ Webhook API

SukiFlow tích hợp sẵn một **máy chủ REST Webhook API độc lập** (cổng mặc định `10790`), cho phép kết nối SukiFlow với các nền tảng tự động hoá như **n8n, Make.com, Zapier**, hoặc script Python / cURL để tạo ảnh và video từ xa:

- **Bảo mật bằng Khóa API (`X-API-Key`):** Mọi request tạo media đều được xác thực an toàn.
- **Điều khiển mạng linh hoạt:** Chạy nội bộ (`127.0.0.1`) hoặc mở rộng cho mạng cục bộ (`0.0.0.0` - LAN).
- **Trang Webhook Playground (`/webhook-test`):** Giao diện thử nghiệm trực quan tích hợp sẵn tại `http://127.0.0.1:10789/webhook-test`.

📖 Xem hướng dẫn chi tiết tại: **[Tài liệu Webhook API đầy đủ (WEBHOOK_API.md)](WEBHOOK_API.md)**.

---

## Dữ liệu và quyền riêng tư

- Toàn bộ file ảnh, video, kịch bản được lưu trực tiếp trên máy tính của bạn theo thư mục đã chọn.
- Phiên đăng nhập được mã hóa và lưu an toàn trong thư mục dữ liệu ứng dụng.
- Đăng xuất tài khoản SukiFlow không làm mất danh sách Linked Accounts hay Proxy đã cấu hình.
- Ứng dụng tuân thủ tiêu chuẩn bảo mật, không gửi cookie hay mật khẩu ra máy chủ bên ngoài.

---

## Khắc phục sự cố

### 1. Báo đạt giới hạn tài khoản (Gói Basic tối đa 5 Linked Accounts)
- Hệ thống tính giới hạn dựa trên **tổng số lượng tài khoản duy nhất của mọi dịch vụ gộp lại** trên máy. Nếu bạn có 2 Flow, 2 Muse, 1 ChatGPT thì tổng đã là 5 tài khoản.
- Để thêm tài khoản mới, bạn có thể xóa bớt tài khoản cũ hoặc nâng cấp lên gói **PRO** (10 tài khoản) hoặc **ULTRA** (100 tài khoản).

### 2. Trình duyệt mở ra ở landing page mà không đăng nhập được
- SukiFlow v2.0 đã có cơ chế tự động click nút "Create with Google Flow" / "Sign in". Hãy để trình duyệt mở tự nhiên trong 3-5 giây để trang tải xong và chuyển hướng sang màn hình đăng nhập Google.

### 3. Tài khoản bị hạn chế tốc độ hoặc chặn mạng
- Mở **Setting → Linked Accounts → Quản lý Proxy**.
- Bấm **⚡ Tự động gán tất cả** để gán địa chỉ IPv6 riêng biệt cho từng tài khoản, hoặc thêm Proxy ngoài (HTTP/SOCKS5).

### 4. Cài đè trên Windows báo “Access is denied”
- Đóng hoàn toàn SukiFlow (kiểm tra Task Manager để chắc chắn không còn tiến trình chạy ngầm), sau đó chạy lại file cài đặt EXE.

---

## Cập nhật

SukiFlow tự động kiểm tra chính sách phiên bản khi khởi động. Luôn tải bản cài đặt chính thức từ:

**[GitHub Releases — SukiFlow](https://github.com/suki-app/sukiflow-doc/releases/latest)**

---

## Lưu ý

SukiFlow là công cụ hỗ trợ tự động hoá quy trình sáng tạo. SukiFlow không thuộc, không được tài trợ và không được xác nhận bởi Google, OpenAI, xAI, Meta, Pexels, Pixabay hoặc các nền tảng bên thứ ba được nhắc đến. Mọi tên thương hiệu thuộc về chủ sở hữu tương ứng.

<p align="center">
  <img src="docs/images/sukiflow-logo.png" alt="SukiFlow" width="72"><br>
  <strong>SukiFlow v2.0.0</strong><br>
  © 2026 SukiFlow
</p>
