# SukiFlow Webhook API

Webhook API cho phép script, ứng dụng nội bộ và dịch vụ tự động hóa gửi yêu cầu tạo ảnh/video tới SukiFlow Desktop đang chạy trên máy người dùng.

> API hoạt động khi SukiFlow đang mở, Webhook Server đang bật và có Linked Account còn khả dụng cho dịch vụ được chọn. Khả năng tạo media còn phụ thuộc quyền truy cập, hạn mức và trạng thái của nền tảng bên thứ ba.

Đối chiếu gần nhất với mã nguồn và kiểm thử thực tế Flow/Muse: **07/11/2026**.

## Mục lục

- [1. Bật server và chọn địa chỉ](#1-bật-server-và-chọn-địa-chỉ)
- [2. Xác thực và giới hạn request](#2-xác-thực-và-giới-hạn-request)
- [3. Quy trình gửi và nhận kết quả](#3-quy-trình-gửi-và-nhận-kết-quả)
- [4. Endpoint](#4-endpoint)
- [5. Tạo ảnh](#5-tạo-ảnh)
- [6. Tạo video](#6-tạo-video)
- [7. Ảnh tham chiếu](#7-ảnh-tham-chiếu)
- [8. Gom nhiều prompt thành batch](#8-gom-nhiều-prompt-thành-batch)
- [9. Lỗi và trạng thái](#9-lỗi-và-trạng-thái)
- [10. Bảo mật khi truy cập từ LAN hoặc Internet](#10-bảo-mật-khi-truy-cập-từ-lan-hoặc-internet)
- [11. Ví dụ nhanh](#11-ví-dụ-nhanh)
- [12. Khả năng hiện tại](#12-khả-năng-hiện-tại)

## 1. Bật server và chọn địa chỉ

1. Mở SukiFlow Desktop.
2. Vào **Thiết lập → Webhook API**.
3. Chọn cổng server, xem API key và bật server.
4. Giữ SukiFlow mở trong khi client gửi và theo dõi tác vụ.

Địa chỉ mặc định:

```text
http://127.0.0.1:10790
```

Port có thể cấu hình trong khoảng `1024–65535`. Máy chạy SukiFlow phải có Linked Account đã đăng nhập cho đúng provider.

## 2. Xác thực và giới hạn request

Mọi endpoint, ngoại trừ `GET /api/health`, yêu cầu API key trong header:

```http
X-API-Key: YOUR_SUKIFLOW_API_KEY
```

Request body dùng JSON UTF-8. Kích thước tối đa của một request là **10 MB**. Không gửi API key trong URL hoặc ghi key vào log công khai.

## 3. Quy trình gửi và nhận kết quả

1. Gửi `POST` tới endpoint tạo ảnh hoặc video.
2. Khi request hợp lệ, server trả `202 Accepted` cùng `task_id`.
3. Poll `GET /api/status/{task_id}` cho đến khi `status` là `completed`, `failed` hoặc `cancelled`.
4. Khi thành công, tải từng URL trong mảng `results` bằng `GET` và kèm API key.

Các trạng thái task:

| Trạng thái | Ý nghĩa |
|---|---|
| `pending` | Đã nhận và đang chờ worker xử lý |
| `running` | Worker đang thực hiện tác vụ |
| `completed` | Hoàn tất; `results` chứa đường dẫn file |
| `failed` | Thất bại; xem trường `error` |
| `cancelled` | Tác vụ bị hủy bởi worker/provider |

Ví dụ phản hồi khi xếp task:

```json
{
  "task_id": "a1b2c3d4e5f6",
  "status": "pending",
  "message": "Image task queued for processing",
  "poll_url": "/api/status/a1b2c3d4e5f6"
}
```

Ví dụ phản hồi khi hoàn tất:

```json
{
  "task_id": "a1b2c3d4e5f6",
  "type": "image",
  "service_type": "google_flow",
  "status": "completed",
  "results": [
    "/api/files/opaque-file-id-1",
    "/api/files/opaque-file-id-2"
  ],
  "completed_at": 1792820424.21
}
```

`count` là số ảnh/video yêu cầu cho một prompt. Nếu provider trả đủ, mỗi file sẽ có một phần tử trong `results`. Một số provider chỉ cho phép `count=1`.

Task và mã file được giữ trong bộ nhớ của tiến trình SukiFlow. Nếu đóng/restart app hoặc server trước khi tải file, task/file URL cũ có thể không còn tra cứu được. File đã lưu trong thư mục đầu ra vẫn ở trên ổ đĩa.

## 4. Endpoint

| Method | Endpoint | Xác thực | Mô tả |
|---|---|---|---|
| `GET` | `/api/health` | Không | Kiểm tra server có phản hồi |
| `POST` | `/api/image/generate` | Có | Tạo ảnh theo `type` |
| `POST` | `/api/video/generate` | Có | Tạo video theo `type` |
| `GET` | `/api/tasks` | Có | Danh sách tối đa 50 task gần nhất |
| `GET` | `/api/status/{task_id}` | Có | Chi tiết một task |
| `GET` | `/api/files/{file_id}` | Có | Tải ảnh/video kết quả |

Ví dụ health check:

```bash
curl http://127.0.0.1:10790/api/health
```

## 5. Tạo ảnh

Endpoint chung:

```http
POST /api/image/generate
```

`type` chọn provider. Nếu bỏ qua, mặc định là `google_flow`.

### Google Flow Image

```json
{
  "type": "google_flow",
  "prompt": "A paper lantern floating above a quiet river at dusk",
  "model": "nano_banana_2",
  "aspect_ratio": "16:9",
  "image_size": "1K (Tiêu chuẩn)",
  "count": 2,
  "style_prompt": "cinematic lighting",
  "delay": 5,
  "auto_assign": true,
  "clean_watermark": true,
  "save_mode": "job"
}
```

| Field | Bắt buộc | Giá trị / mặc định | Mô tả |
|---|---:|---|---|
| `type` | Không | `google_flow` | Chọn dịch vụ xử lý yêu cầu; nếu bỏ qua, server dùng Google Flow. |
| `prompt` | Có | Tối đa 10.000 ký tự | Nội dung mô tả hình ảnh cần tạo hoặc chỉnh sửa. |
| `model` | Không | `nano_banana_2`, `nano_banana_2_lite`, `nano_banana_pro`; mặc định `nano_banana_2` | Chọn model tạo ảnh của Google Flow. |
| `aspect_ratio` | Không | `16:9`, `9:16`, `1:1`, `4:3`, `3:4`; mặc định `16:9` | Tỉ lệ khung hình của ảnh đầu ra. |
| `image_size` | Không | `1K (Tiêu chuẩn)`, `2K (Sắc nét)`, `4K (Siêu nét)`; mặc định 1K | Mức độ phân giải/kích thước ảnh đầu ra. |
| `count` | Không | Số ảnh `1–4`; mặc định `1` | Số ảnh yêu cầu tạo cho prompt này. |
| `style_prompt` | Không | Chuỗi tối đa 2.000 ký tự | Chỉ dẫn bổ sung về phong cách hoặc cách thể hiện hình ảnh. |
| `delay` | Không | `0`, `5`, `10`, `15`, `30` giây; mặc định `5` | Thời gian chờ giữa các tác vụ trong lượt xử lý. |
| `auto_assign` | Không | Boolean; mặc định `true` | Cho phép worker tự chọn tài khoản Google Flow khả dụng. |
| `clean_watermark` | Không | Boolean; mặc định `true` | Bật hoặc tắt bước xử lý làm sạch watermark sau khi tạo ảnh. |
| `reference_images` | Không | Danh sách ảnh tham chiếu; tối đa 10 ảnh, tổng tối đa 7 MB | Ảnh đầu vào để hướng dẫn hoặc làm cơ sở chỉnh sửa; có ảnh thì worker xử lý theo Image → Image. |
| `model_fallback` | Không | Cấu hình model dự phòng, xem bên dưới | Quy định có chuyển sang model khác khi model chính không dùng được hay không. |
| `save_mode` | Không | `job` hoặc `direct`; mặc định `job` | Chọn cách tổ chức lưu file kết quả: theo thư mục lượt chạy (`job`) hoặc trực tiếp (`direct`). |
| `base_folder` | Không | Thư mục đầu ra tùy chọn; cần có quyền ghi | Thư mục gốc để lưu file; nếu không truyền, dùng thư mục đầu ra cấu hình trong SukiFlow. |
| `batch_id`, `batch_size`, `parallel` | Không | Gom nhiều prompt thành batch; xem [Gom nhiều prompt](#8-gom-nhiều-prompt-thành-batch) | Các field phối hợp nhiều request Flow thành một batch và đặt số lượng task chạy đồng thời. |

Image → Image được xác định bằng `reference_images`: không có ảnh tham chiếu thì là Text → Image; có ảnh tham chiếu thì worker gửi ảnh cùng prompt. **Không truyền field `mode` cho Google Flow Image.**

`model_fallback`:

```json
{
  "enabled": true,
  "all_models": false,
  "model": "Nano Banana 2 Lite"
}
```

Các field hợp lệ: `enabled` và `all_models` là boolean; `model` là một trong `Nano Banana 2`, `Nano Banana 2 Lite`, `Nano Banana Pro`. Khi bật fallback, đặt `all_models: true` hoặc chỉ định `model`.

### Google Pics Image

```json
{
  "type": "google_pics",
  "prompt": "A ceramic vase with fresh flowers on a sunlit table",
  "aspect_ratio": "1:1",
  "export_size": "2k",
  "count": 2,
  "reference_images": [
    {"filename": "reference.png", "data": "data:image/png;base64,<BASE64_DATA>"}
  ],
  "clean_watermark": true,
  "save_mode": "job"
}
```

- `aspect_ratio`: `16:9`, `9:16`, `1:1`, `4:3`, `3:4`.
- `export_size`: `original`, `2k`, `4k`.
- `count`: `1–4`.
- `reference_images`: tối đa 3 ảnh.
- `clean_watermark`: boolean, mặc định `true`.
- `save_mode`: `job` hoặc `direct`; `base_folder` tùy chọn.

### Google Vids Image

```json
{
  "type": "google_vids",
  "mode": "text_to_image",
  "prompt": "A minimalist paper boat floating on a pastel blue background",
  "aspect_ratio": "16:9",
  "style": "watercolor",
  "timeout": 240,
  "delay": 2,
  "save_mode": "job"
}
```

- Hiện chỉ hỗ trợ `mode: "text_to_image"`; mỗi request tạo một ảnh.
- Có thể bỏ `mode`; server mặc định dùng `text_to_image`.
- `aspect_ratio`: `16:9`, `9:16`, `1:1`.
- `style`: `none`, `photography`, `background`, `vector_art`, `sketch`, `watercolor`, `cyberpunk`. Alias cũng được nhận: `vector art`, `no style`, `no_style`.
- `timeout`: `30–900` giây; mặc định `240`.
- `delay`: `0–30` giây; mặc định `2`.
- Không nhận ảnh tham chiếu. `save_mode` là `job` hoặc `direct`; `base_folder` tùy chọn.

### ChatGPT Image

```json
{
  "type": "chatgpt",
  "mode": "text_to_image",
  "prompt": "A small orange cat sleeping in a sunbeam",
  "aspect_ratio": "16:9",
  "quality": "high",
  "count": 2,
  "style_prompt": "soft editorial photography",
  "wait_seconds": 0,
  "save_mode": "job"
}
```

- `mode`: tùy chọn `text_to_image` hoặc `image_to_image`. Nếu bỏ qua, server suy ra từ ảnh tham chiếu: không có ảnh là Text → Image, có ảnh là Image → Image.
- `aspect_ratio`: `16:9`, `1:1`, `3:2`, `2:3`, `9:16`.
- `quality`: `auto`, `high`, `medium`, `low`.
- `count`: `1–4`.
- `wait_seconds`: `0–120`.
- `style_prompt`: tối đa 2.000 ký tự.
- `image_to_image` yêu cầu `reference_images`, tối đa 5 ảnh. `text_to_image` không nhận ảnh tham chiếu.
- `save_mode`: `job` hoặc `direct`; `base_folder` tùy chọn.

Webhook hiện không hỗ trợ tạo video bằng ChatGPT.

## 6. Tạo video

Endpoint chung:

```http
POST /api/video/generate
```

### Google Flow Video

Text → Video:

```json
{
  "type": "google_flow",
  "mode": "text_to_video",
  "prompt": "A red fox running through fresh snow at sunrise, cinematic wide shot",
  "model": "omni_flash",
  "aspect_ratio": "16:9",
  "video_length": 4,
  "resolution": "360p",
  "count": 1,
  "fallback_service": "google_vids",
  "save_mode": "job"
}
```

Image → Video hỗ trợ 2 kiểu:

**Kiểu 1: Khung hình đầu / cuối (Frames mode)** dùng ảnh đầu bắt buộc và ảnh cuối tùy chọn:

```json
{
  "type": "google_flow",
  "mode": "image_to_video",
  "prompt": "The fox walks from the snowy forest into a sunlit meadow",
  "model": "omni_flash",
  "aspect_ratio": "9:16",
  "video_length": 4,
  "resolution": "360p",
  "start_image": {
    "filename": "start.png",
    "data": "data:image/png;base64,REPLACE_WITH_BASE64"
  },
  "end_image": {
    "filename": "end.png",
    "data": "data:image/png;base64,REPLACE_WITH_BASE64"
  }
}
```

**Kiểu 2: Ảnh tham chiếu (Ingredients / Reference-to-Video mode, tối đa 3 ảnh)** để định hình nhân vật, phong cách hoặc bối cảnh:

```json
{
  "type": "google_flow",
  "mode": "image_to_video",
  "prompt": "The character in @Image1 is walking through a neon-lit cyberpunk street at night, cinematic 4k",
  "model": "omni_flash",
  "aspect_ratio": "16:9",
  "video_length": 4,
  "resolution": "360p",
  "count": 1,
  "reference_images": [
    {
      "filename": "character1.png",
      "data": "data:image/png;base64,REPLACE_WITH_BASE64"
    }
  ],
  "clean_watermark": false,
  "fallback_service": "google_vids",
  "save_mode": "job"
}
```

- `model`: `omni_flash`, `veo_3_1_lite`, `veo_3_1_fast`, `veo_3_1_quality`.
- `mode`: tùy chọn `text_to_video` hoặc `image_to_video`. Nếu bỏ qua, có `start_image` hoặc `reference_images` thì suy ra Image → Video; nếu không có thì Text → Video.
- `creation_mode`: tùy chọn `frames` hoặc `ingredients`. Server tự động nhận diện: có `reference_images` là `ingredients`, có `start_image` là `frames`.
- `reference_images`: tối đa 3 ảnh tham chiếu (chỉ dùng cho `image_to_video` kiểu ingredients). Trong prompt có thể gắn tag `@Image1`, `@Image2`, `@Image3` để định danh.
- Không dùng đồng thời `reference_images` và `start_image`/`end_image` trong cùng một request.
- `aspect_ratio`: `16:9` hoặc `9:16`.
- `omni_flash`: `video_length` là `4`, `6`, `8` hoặc `10`; `resolution` là `360p` hoặc `720p`.
- `veo_3_1_*`: thời lượng cố định 8 giây; có thể bỏ `video_length` hoặc đặt `8`. Không gửi `resolution` (field này chỉ dùng cho `omni_flash`).
- `count`: `1–4`.
- `start_image` chỉ dùng với `image_to_video` kiểu frames; `end_image` tùy chọn nhưng cần có `start_image`.
- Ảnh gửi theo cấu trúc `{ "filename": "...png", "data": "data:image/png;base64,..." }`. Hỗ trợ PNG, JPEG, WebP; tổng dung lượng ảnh tham chiếu hoặc ảnh đầu/cuối tối đa 7 MB.
- `clean_watermark`: boolean; mặc định `false`.
- `save_mode`: `job` hoặc `direct`; `base_folder` tùy chọn.
- `fallback_service`: mặc định `google_vids`; đặt `none` để tắt dự phòng sang Google Vids.
- Fallback chỉ xảy ra khi worker phát `JOB_FAILED` với `fallback_eligible=true` và `count=1`. Các trường hợp được worker đánh dấu gồm lỗi quota/giới hạn tài khoản, tài khoản Agent-only, hết tài khoản Flow, timeout kết nối trước khi submit và trường hợp submit không rõ kết quả sau khi đã đối soát project. Lỗi prompt trống và lỗi không được đánh dấu đủ điều kiện sẽ kết thúc task mà không fallback.
- Google Vids không hỗ trợ `360p`; khi fallback từ Flow `360p`, độ phân giải được nâng lên `720p`. Fallback hiện chỉ áp dụng cho `count=1`.
- Để giữ yêu cầu Flow `360p`, gửi `fallback_service: "none"`. Đã kiểm thử tạo và tải video Omni Flash `360p`, `4` giây ở cả hai hướng ngang/dọc.
- Khi Flow báo tạo xong nhưng thiếu link tải hoặc trả link ảnh poster, worker sẽ thử lấy lại MP4 của chính clip đã gửi. Bước khôi phục này không gửi lại prompt tạo video; nếu vẫn không lấy được file hợp lệ, task sẽ thất bại.
- Khi fallback, task vẫn giữ cùng `task_id`; trường `provider_fallback` trong status cho biết dịch vụ đích, lý do và độ phân giải đã dùng.

**Hook kiểm thử fallback (chỉ dành cho môi trường test):** nếu app khởi chạy với `SUKIFLOW_WEBHOOK_TEST_MODE=1`, có thể gửi `test_force_flow_failure: "quota"` hoặc `"rejected"` để bỏ qua việc gửi request tới Google Flow và phát sinh lỗi giả lập trước submit. Task vẫn đi qua luồng fallback thật sang Google Vids. Hook này chỉ nhận request Flow Video đơn lẻ, `count=1`, `fallback_service="google_vids"`; không dùng trong môi trường production. Nếu biến môi trường không bật, field này bị từ chối.

### Google Vids Video

```json
{
  "type": "google_vids",
  "mode": "text_to_video",
  "prompt": "A paper airplane glides slowly across a quiet blue sky",
  "model": "omni",
  "aspect_ratio": "16:9",
  "video_length": 5,
  "resolution": "720p",
  "count": 1,
  "clean_watermark": true,
  "save_mode": "job"
}
```

- `model`: `omni` (mặc định; alias `google_vids_ai_video` cũng được nhận).
- `mode`: tùy chọn `text_to_video` hoặc `image_to_video`. Nếu bỏ qua, không có `reference_images` là Text → Video; có ảnh là Image/Ingredients → Video.
- `aspect_ratio`: `16:9` hoặc `9:16`.
- `video_length`: số nguyên `3–15` giây.
- `resolution`: `720p` hoặc `1080p`.
- `count`: hiện chỉ nhận `1`.
- `reference_images`: tối đa 3 ảnh. Một ảnh dùng làm Image → Video; nhiều ảnh được chuyển thành Ingredients → Video. Nếu truyền `mode` tường minh, `text_to_video` không nhận ảnh và `image_to_video` yêu cầu ít nhất một ảnh.
- `character_names`: tùy chọn, tối đa 3 tên nhân vật, mỗi tên tối đa 100 ký tự; chỉ định tên nhân vật tương ứng với ảnh tham chiếu đã gửi.
- `delay`: `0–30` giây; mặc định `3`.
- `clean_watermark`: boolean, mặc định `true`.
- `save_mode`: `job` hoặc `direct`; `base_folder` tùy chọn.

### Muse AI Video

```json
{
  "type": "muse_ai",
  "prompt": "A quiet coastal town at sunrise, gentle camera movement",
  "aspect_ratio": "16:9",
  "video_length": 10,
  "format": "mp4",
  "timeout": 420,
  "reference_images": [],
  "save_mode": "job"
}
```

- `aspect_ratio`: `16:9` hoặc `9:16`.
- `video_length`: validator hiện nhận số nguyên `5–10`; nếu bỏ qua, **API vẫn mặc định `5`**, còn giao diện Muse Video mặc định `10`. Nên gửi tường minh `10`.
- Thời lượng được đưa vào prompt, không phải tham số điều khiển trực tiếp model Muse. Kiểm thử thực tế yêu cầu `5` giây vẫn nhận clip `10` giây; các lượt có ảnh tham chiếu cũng nhận clip `10` giây. Không coi `video_length` là cam kết thời lượng file đầu ra.
- `format`: `mp4`, `webm`, `mov`.
- `timeout`: `60–900` giây; mặc định `420`.
- `reference_images`: tùy chọn, tối đa **5 ảnh** cho mỗi video. Gửi theo cấu trúc ở mục [Ảnh tham chiếu](#7-ảnh-tham-chiếu). Không có ảnh thì tạo từ prompt; có ảnh thì Muse dùng chúng làm ảnh tham chiếu cho video.
- Khi có ảnh tham chiếu, Muse có thể giữ tỷ lệ ảnh đầu vào thay vì tỷ lệ yêu cầu. Kiểm thử ảnh ngang trả video `1152×768`; ảnh nhân vật dọc trả `672×1152`. Worker có kiểm tra tỷ lệ với sai số cho phép; lệch lớn có thể khiến task `failed` dù Muse đã tạo và tải được clip.

### Muse AI Image

```json
{
  "type": "muse_ai",
  "prompt": "A small fox in a sunlit forest, natural cinematic composition",
  "aspect_ratio": "16:9",
  "timeout": 420,
  "reference_images": [
    {"filename": "fox-reference.png", "data": "data:image/png;base64,<BASE64_DATA>"}
  ],
  "save_mode": "job"
}
```

- Gửi qua `POST /api/image/generate`; `type` nhận `muse_ai` (cũng chấp nhận alias `muse`, `muse_image`, `muse_ai_image`).
- `aspect_ratio`: `16:9` hoặc `9:16`; mặc định `16:9`.
- `reference_images`: tùy chọn, tối đa **5 ảnh**. Không có ảnh thì tạo từ prompt; có ảnh thì gửi ảnh cùng prompt.
- `timeout`: `60–900` giây; mặc định `420`.
- Mỗi request tạo một ảnh; API không nhận `count` hoặc `parallel` cho Muse.
- Muse Image và Muse Video dùng chung một phiên worker, nên mỗi lần chỉ nhận một task Muse đang chờ/chạy. Gửi task Muse khác trong lúc đó sẽ nhận `409`; chờ task hiện tại kết thúc rồi gửi tiếp.
- Worker loại ảnh ứng viên trùng nội dung file tham chiếu đầu vào để tránh trả nhầm ảnh upload thành ảnh kết quả. Khi kiểm tra tích hợp, cần tải file và kiểm tra nội dung, không chỉ dựa vào trạng thái `completed`.

**Nhân vật `@Tên` trong Muse:** khi không gửi `reference_images`, worker có thể tìm ảnh nhân vật đã lưu trên chính máy SukiFlow từ tên trong prompt. Tên phải tồn tại trong thư viện máy đó. Khi đã gửi ảnh qua `reference_images`, các đường dẫn ảnh tường minh được ưu tiên; không dựa vào việc tự ghép thêm toàn bộ ảnh từ `@Tên`. Để request độc lập với thư viện cục bộ, gửi đủ ảnh cần dùng trong `reference_images`. Dấu `+` là thao tác trên giao diện; API dùng mảng `reference_images` tương ứng.
- Hỗ trợ `save_mode` `job`/`direct` và `base_folder`.

## 7. Ảnh tham chiếu

Gửi ảnh trong JSON dưới dạng Base64 hoặc Data URL Base64:

```json
{
  "filename": "reference.png",
  "data": "data:image/png;base64,iVBORw0KGgo..."
}
```

Các quy tắc chung:

- Định dạng PNG, JPEG hoặc WebP; phần mở rộng tên file (nếu có) phải khớp định dạng thật.
- Mỗi ảnh tối đa 7 MB; tổng `reference_images` tối đa 7 MB.
- Tối đa 10 ảnh ở cấp validator chung, đồng thời tuân theo giới hạn riêng từng provider.
- Body JSON toàn request tối đa 10 MB.

Ví dụ đọc ảnh và gửi bằng Python:

```python
import base64
import mimetypes
from pathlib import Path

def reference_image(path):
    path = Path(path)
    mime = mimetypes.guess_type(path.name)[0] or "image/png"
    encoded = base64.b64encode(path.read_bytes()).decode("ascii")
    return {
        "filename": path.name,
        "data": f"data:{mime};base64,{encoded}",
    }
```

## 8. Gom nhiều prompt thành batch

Google Flow Image và Google Flow Video cho phép gửi từng prompt thành request riêng rồi ghép chúng vào một batch. Worker duy trì cửa sổ chạy liên tục: khi một prompt xong, prompt đang chờ tiếp theo bắt đầu ngay nếu còn slot, không cần chờ cả nhóm hiện tại hoàn tất.

Ví dụ phần field thêm vào từng request:

```json
{
  "batch_id": "campaign-autumn-2026",
  "batch_size": 2,
  "parallel": 1
}
```

- `batch_id`: ID dùng chung. Bắt buộc khi ghép nhiều request; với Flow Video dài `8–80` ký tự, với Flow Image là chuỗi tối đa `100` ký tự.
- `batch_size`: tổng số prompt trong batch. Flow Image nhận `1–100` (nếu là một request đơn, bỏ các field batch); Flow Video nhận `2–100` khi dùng batch. Server giới hạn tối đa 100 task đang chờ/chạy.
- `parallel`: số prompt tối đa được chạy đồng thời, `1–4` và không lớn hơn `batch_size`. Flow Image mặc định `min(batch_size, 4)`; Flow Video yêu cầu truyền field này trong batch.
- Với Flow Image, các request trong batch phải dùng chung `base_folder`, `save_mode`, `model_fallback`, `batch_size`, `parallel`.
- Với Flow Video, các request trong batch phải dùng chung cấu hình video và `batch_size`, `parallel`.
- Mỗi request vẫn có `count` riêng (`1–4` ảnh hoặc video cho prompt đó); `batch_size` là số prompt, không phải số media output.
- Nếu không gửi đủ request, server tự flush các prompt đã nhận sau 120 giây (Image và Video). Các prompt gửi sau thời điểm flush cần dùng `batch_id` mới.

Không dùng batch nếu chỉ tạo một prompt; khi đó bỏ `batch_id`, `batch_size`, `parallel`.

## 9. Lỗi và trạng thái

| HTTP | Trường hợp thường gặp |
|---:|---|
| `200` | GET thành công; health check |
| `202` | Request hợp lệ và task đã được xếp |
| `204` | CORS preflight `OPTIONS` |
| `400` | JSON/field không hợp lệ, thiếu prompt hoặc tham số không được hỗ trợ |
| `401` | Thiếu hoặc sai API key |
| `404` | Endpoint, task ID hoặc file ID không tồn tại |
| `409` | Không có account khả dụng, worker đang bận hoặc đạt giới hạn task hoạt động |
| `413` | JSON body vượt 10 MB |
| `500` | Lỗi nội bộ khi nhận/dispatch request |

Task có thể được nhận `202` nhưng thất bại ở bước worker/provider. Khi đó `GET /api/status/{task_id}` trả `status: "failed"` và thông tin trong `error`. `202` chỉ xác nhận server nhận task, không đảm bảo provider sẽ tạo media thành công.

Giới hạn runtime hiện tại: tối đa 100 task đang `pending`/`running`; danh sách `/api/tasks` trả tối đa 50 task gần nhất.

### Chuyển tài khoản khi gặp lỗi quota

Worker có thể thử Linked Account khả dụng tiếp theo khi nhận diện được lỗi gắn với tài khoản, như hết quota/lượt hoặc lỗi xác thực/quyền. Tài khoản đang tắt, hết hạn hoặc bị đánh dấu tạm giới hạn sẽ bị bỏ qua khi lập danh sách tài khoản chạy.

Failover là best effort: lỗi giao diện, selector, kết nối hoặc timeout không nhất thiết được phân loại là lỗi tài khoản, nên không đảm bảo sẽ chuyển tài khoản trong mọi trường hợp. Nếu task thất bại, xem `error` trong trạng thái task và log Webhook trong app. Trước khi gửi lại yêu cầu tạo mới, hãy kiểm tra task cũ vì provider có thể đã nhận yêu cầu dù client chưa nhận được kết quả.

## 10. Bảo mật khi truy cập từ LAN hoặc Internet

### Ứng dụng cùng máy

Dùng `http://127.0.0.1:10790`. Đây là lựa chọn phù hợp cho script và ứng dụng local.

### Thiết bị trong cùng LAN

Trong cài đặt Webhook API, bind server vào `0.0.0.0`, sau đó gọi địa chỉ LAN của máy chạy SukiFlow, ví dụ `http://192.168.1.20:10790`. Chỉ bật trên mạng tin cậy và vẫn phải gửi `X-API-Key`.

### Ứng dụng/web chạy trên Internet

`127.0.0.1` trên trình duyệt online là máy của người truy cập, không phải máy chạy SukiFlow. Muốn kết nối tới SukiFlow local, máy đó cần có đường kết nối tới server, ví dụ VPN hoặc tunnel HTTPS được cấu hình tới `127.0.0.1:10790`.

Không mở cổng HTTP của Webhook API trực tiếp ra Internet. Tunnel chỉ tạo đường mạng; nó không thay thế việc xác thực và bảo vệ API. CORS hiện cho phép request từ browser, vì vậy:

- Không nhúng API key dùng chung vào JavaScript/frontend công khai.
- Dùng backend của website làm proxy, hoặc đặt lớp đăng nhập/Access trước tunnel.
- Dùng HTTPS và giới hạn ai được gọi tunnel; cấp riêng API key/secret nếu kiến trúc hỗ trợ.
- Nếu website server-side gọi tunnel, giữ API key trong secret store của backend.

## 11. Ví dụ nhanh

Đặt base URL và API key trong biến môi trường để tránh ghi key vào source code:

```bash
export SUKIFLOW_URL="http://127.0.0.1:10790"
export SUKIFLOW_API_KEY="YOUR_SUKIFLOW_API_KEY"
```

Gửi yêu cầu Flow Image:

```bash
curl -X POST "$SUKIFLOW_URL/api/image/generate" \
  -H "Content-Type: application/json" \
  -H "X-API-Key: $SUKIFLOW_API_KEY" \
  -d '{
    "type": "google_flow",
    "prompt": "A red fox sitting beside a lantern in a forest clearing",
    "model": "nano_banana_2",
    "aspect_ratio": "16:9",
    "image_size": "1K (Tiêu chuẩn)",
    "count": 1,
    "save_mode": "job"
  }'
```

Poll task (thay `TASK_ID` bằng ID vừa nhận):

```bash
curl "$SUKIFLOW_URL/api/status/TASK_ID" \
  -H "X-API-Key: $SUKIFLOW_API_KEY"
```

Tải file (thay `FILE_ID` bằng phần cuối của URL trong `results`):

```bash
curl -L "$SUKIFLOW_URL/api/files/FILE_ID" \
  -H "X-API-Key: $SUKIFLOW_API_KEY" \
  -o result.jpg
```

## 12. Khả năng hiện tại

| Provider | Ảnh | Video | Ghi chú |
|---|---|---|---|
| Google Flow | Có | Có | Image → Image dùng `reference_images`; Flow Video suy ra Text → Video hoặc Start/End Image → Video từ ảnh đầu vào |
| Google Pics | Có | Không | Ảnh tham chiếu tối đa 3; hỗ trợ `count=1–4` |
| Google Vids | Có | Có | Vids Image chỉ Text → Image; Vids Video suy ra Text/Image/Ingredients → Video từ `reference_images` |
| ChatGPT | Có | Không | Webhook chỉ hỗ trợ ChatGPT Image; `count=1–4` |
| Muse AI | Có | Có | Cả Muse Image và Video nhận tối đa 5 ảnh tham chiếu; mỗi request trả một kết quả |

API key, Linked Account và file kết quả thuộc máy chạy SukiFlow. Provider có thể từ chối tác vụ do hết quota, tài khoản, region hoặc thay đổi giao diện/dịch vụ; hãy luôn kiểm tra trạng thái cuối cùng thay vì chỉ dựa vào HTTP `202`.
