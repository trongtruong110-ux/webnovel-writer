---
name: image-prompts
purpose: Quy trình tạo prompt hình ảnh cho từng cảnh (scene) đảm bảo tính nhất quán.
---

# Hướng dẫn Tạo Prompt Hình ảnh (Easy Diffusion)

Tài liệu này hướng dẫn cách tạo prompt hình ảnh cho mỗi cảnh phim/truyện nhằm phục vụ việc minh họa, đảm bảo tính nhất quán về nhân vật và bối cảnh.

## 1. Cấu hình Kỹ thuật (Bắt buộc)

Tất cả các prompt phải tuân thủ cấu hình sau trong Easy Diffusion v3.0.9c:

- **Model chính**: `dreamshaper_8.safetensors`
- **LoRA**: `symaozhan.safetensors` (Sử dụng cú pháp `<lora:symaozhan:0.7>` hoặc theo yêu cầu của giao diện Easy Diffusion).
- **Định dạng Prompt**: Tiếng Anh, mô tả chi tiết, ưu tiên các từ khóa về chất lượng (masterpiece, best quality, cinematic lighting).

## 2. Quy tắc Nhất quán (Consistency)

### 2.1 Nhất quán Nhân vật (Character Consistency)
Để đảm bảo nhân vật trông giống nhau qua các cảnh:
- **Truy vấn index.db**: Luôn đọc `desc` và `current_json` của nhân vật từ `index.db`.
- **Mô tả đặc điểm cố định**: Luôn bao gồm các đặc điểm vật lý cố định (màu tóc, kiểu tóc, màu mắt, vết sẹo, trang phục đặc trưng).
- **Ví dụ**: "A young man with messy silver hair, cold blue eyes, wearing a black leather duster with silver buckles."

### 2.2 Nhất quán Bối cảnh (Background Consistency)
Để đảm bảo địa điểm không bị thay đổi đột ngột:
- **Truy vấn địa điểm**: Sử dụng mô tả địa điểm từ `设定集/世界观.md` hoặc `index.db`.
- **Từ khóa môi trường**: Sử dụng các từ khóa cố định về ánh sáng, kiến trúc và không khí của địa điểm đó.
- **Ví dụ**: "Deep inside a glowing crystal cave, blue bioluminescent plants on the walls, misty atmosphere."

## 3. Cấu trúc một Image Prompt Standard

Mỗi cảnh (scene) sẽ có một `visual_prompt` theo cấu trúc:
`[Subject Description] + [Action/Pose] + [Location/Background Detail] + [Lighting/Mood] + [Art Style Keywords] + <lora:symaozhan:0.7>`

## 4. Ví dụ Thực tế

**Cảnh: Lâm Phong đột phá trong mật thất**
- **Nhân vật**: Lâm Phong (Tóc đen ngắn, áo bào xanh lam, vẻ mặt kiên định).
- **Địa điểm**: Mật thất đá, cổ kính, ánh nến lung linh.
- **Visual Prompt**:
  > Lin Feng, a young man with short black hair and firm expression, wearing a traditional blue cultivation robe, sitting in lotus position, surrounded by swirling blue energy aura, inside an ancient stone chamber with flickering candlelight, cinematic lighting, masterpiece, 8k, highly detailed, <lora:symaozhan:0.8> --model dreamshaper_8.safetensors

## 5. Tích hợp vào Workflow
1. **Data Agent** thực hiện chia cảnh (Step F).
2. Với mỗi cảnh, Data Agent truy vấn thông tin thực thể liên quan.
3. Tạo `visual_prompt` dựa trên tóm tắt cảnh và dữ liệu thực thể.
4. Lưu `visual_prompt` vào metadata của scene trong `index.db`.
