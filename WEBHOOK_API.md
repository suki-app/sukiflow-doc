# SukiFlow Webhook API

Webhook API cho phép script, ứng dụng nội bộ và dịch vụ tự động hóa gửi yêu cầu tạo ảnh/video tới SukiFlow Desktop đang chạy trên máy người dùng.

> API hoạt động khi SukiFlow đang mở, Webhook Server đang bật và có Linked Account còn khả dụng cho dịch vụ được chọn. Khả năng tạo media còn phụ thuộc quyền truy cập, hạn mức và trạng thái của nền tảng bên thứ ba.

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
http://127.0.0.1:8766
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
curl http://127.0.0.1:8766/api/health
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
  "reference_images": [],
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
- `aspect_ratio`: `16:9`, `9:16`, `1:1`.
- `style`: `none`, `photography`, `background`, `vector_art`, `sketch`, `watercolor`, `cyberpunk`.
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

- `mode`: `text_to_image` hoặc `image_to_image`.
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
  "save_mode": "job"
}
```

Image → Video dùng ảnh đầu bắt buộc và ảnh cuối tùy chọn:

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

- `model`: `omni_flash`, `veo_3_1_lite`, `veo_3_1_fast`, `veo_3_1_quality`.
- `mode`: `text_to_video` hoặc `image_to_video`; mặc định `text_to_video`.
- `aspect_ratio`: `16:9` hoặc `9:16`.
- `omni_flash`: `video_length` là `4`, `6`, `8` hoặc `10`; `resolution` là `360p` hoặc `720p`.
- `veo_3_1_*`: thời lượng cố định 8 giây; bỏ `resolution` và không đặt `video_length` khác 8.
- `count`: `1–4`.
- `start_image` chỉ dùng với `image_to_video`; `end_image` tùy chọn nhưng cần có `start_image`.
- Ảnh gửi theo cấu trúc `{ "filename": "...png", "data": "data:image/png;base64,..." }`. Hỗ trợ PNG, JPEG, WebP; tổng kích thước ảnh đầu/cuối tối đa 7 MB.
- `clean_watermark`: boolean; mặc định `false`.
- `save_mode`: `job` hoặc `direct`; `base_folder` tùy chọn.

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
- `mode`: `text_to_video` hoặc `image_to_video`.
- `aspect_ratio`: `16:9` hoặc `9:16`.
- `video_length`: số nguyên `3–10` giây.
- `resolution`: `720p` hoặc `1080p`.
- `count`: hiện chỉ nhận `1`.
- `reference_images`: tối đa 3 ảnh; bắt buộc với `image_to_video`, không được gửi cùng `text_to_video`. Một ảnh dùng làm Image → Video; nhiều ảnh được chuyển thành Ingredients → Video.
- `delay`: `0–30` giây; mặc định `3`.
- `clean_watermark`: boolean, mặc định `true`.
- `save_mode`: `job` hoặc `direct`; `base_folder` tùy chọn.

### Muse AI Video

```json
{
  "type": "muse_ai",
  "prompt": "A quiet coastal town at sunrise, gentle camera movement",
  "aspect_ratio": "16:9",
  "video_length": 5,
  "format": "mp4",
  "timeout": 420,
  "save_mode": "job"
}
```

- `aspect_ratio`: `16:9` hoặc `9:16`.
- `video_length`: số nguyên `5–10` giây.
- `format`: `mp4`, `webm`, `mov`.
- `timeout`: `60–900` giây; mặc định `420`.
- Muse AI Image chưa được hỗ trợ qua webhook.

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

Google Flow Image và Google Flow Video cho phép các request riêng ghép vào cùng một batch bằng `batch_id`. Gửi từng prompt thành một request độc lập; giữ nguyên `batch_id`, `batch_size`, `parallel` và các setting chung cho mọi request thuộc batch.

Ví dụ phần field thêm vào từng request:

```json
{
  "batch_id": "campaign-autumn-2026",
  "batch_size": 2,
  "parallel": 1
}
```

- `batch_id`: chuỗi 8–80 ký tự, bắt buộc nếu batch có từ 2 task.
- `batch_size`: số task dự kiến trong batch, `2–4`.
- `parallel`: số task chạy đồng thời, `1–4`, không lớn hơn `batch_size`.
- Với Flow Image, các request trong batch phải dùng chung `base_folder`, `save_mode`, `model_fallback`, `batch_size`, `parallel`.
- Với Flow Video, các request trong batch phải dùng chung cấu hình video và `batch_size`, `parallel`.
- Nếu không gửi đủ request, server tự flush batch sau khoảng 15 giây (Image) hoặc 20 giây (Video).

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

## 10. Bảo mật khi truy cập từ LAN hoặc Internet

### Ứng dụng cùng máy

Dùng `http://127.0.0.1:8766`. Đây là lựa chọn phù hợp cho script và ứng dụng local.

### Thiết bị trong cùng LAN

Trong cài đặt Webhook API, bind server vào `0.0.0.0`, sau đó gọi địa chỉ LAN của máy chạy SukiFlow, ví dụ `http://192.168.1.20:8766`. Chỉ bật trên mạng tin cậy và vẫn phải gửi `X-API-Key`.

### Ứng dụng/web chạy trên Internet

`127.0.0.1` trên trình duyệt online là máy của người truy cập, không phải máy chạy SukiFlow. Muốn kết nối tới SukiFlow local, máy đó cần có đường kết nối tới server, ví dụ VPN hoặc tunnel HTTPS được cấu hình tới `127.0.0.1:8766`.

Không mở cổng HTTP của Webhook API trực tiếp ra Internet. Tunnel chỉ tạo đường mạng; nó không thay thế việc xác thực và bảo vệ API. CORS hiện cho phép request từ browser, vì vậy:

- Không nhúng API key dùng chung vào JavaScript/frontend công khai.
- Dùng backend của website làm proxy, hoặc đặt lớp đăng nhập/Access trước tunnel.
- Dùng HTTPS và giới hạn ai được gọi tunnel; cấp riêng API key/secret nếu kiến trúc hỗ trợ.
- Nếu website server-side gọi tunnel, giữ API key trong secret store của backend.

## 11. Ví dụ nhanh

Đặt base URL và API key trong biến môi trường để tránh ghi key vào source code:

```bash
export SUKIFLOW_URL="http://127.0.0.1:8766"
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
| Google Flow | Có | Có | Image → Image dùng `reference_images`; Flow Video hỗ trợ Text → Video và Start/End Image → Video |
| Google Pics | Có | Không | Ảnh tham chiếu tối đa 3; hỗ trợ `count=1–4` |
| Google Vids | Có | Có | Vids Image hiện chỉ Text → Image; Vids Video hỗ trợ Text/Image/Ingredients → Video qua `mode` và ảnh tham chiếu |
| ChatGPT | Có | Không | Webhook chỉ hỗ trợ ChatGPT Image; `count=1–4` |
| Muse AI | Không | Có | Muse AI Image chưa được hỗ trợ qua webhook |

API key, Linked Account và file kết quả thuộc máy chạy SukiFlow. Provider có thể từ chối tác vụ do hết quota, tài khoản, region hoặc thay đổi giao diện/dịch vụ; hãy luôn kiểm tra trạng thái cuối cùng thay vì chỉ dựa vào HTTP `202`.
