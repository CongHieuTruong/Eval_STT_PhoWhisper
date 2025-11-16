# Báo cáo đánh giá PhoWhisper

## 1. Thiết lập thí nghiệm
- Mô hình: `vinai/phowhisper-medium`
- Ngữ cảnh: nhận dạng tiếng nói tiếng Việt (ASR) trên 3 đoạn âm thanh có đặc tính khác nhau:
  - Môi trường yên tĩnh
  - Môi trường có tạp âm nền, lời nói tự nhiên, dài
  - Đoạn có nhiều từ ngữ chuyên ngành
- Âm thanh được chuẩn hóa về tần số lấy mẫu 16 kHz và lưu dạng `*_16k.wav`.
- Chỉ số đánh giá:
  - **WER (Word Error Rate)** – độ chính xác nhận dạng lời nói
  - **Độ trễ xử lý 10 giây** – thời gian mô hình xử lý đoạn audio dài 10 giây

## 2. Kết quả chi tiết từng file

### 2.1. File 1 – Môi trường yên tĩnh
- Tên file: `1.moi_truong_yen_tinh_16k.wav`
- Thời lượng: **5.59 s**
- WER: **0.0000** (0%)
- Độ trễ cho đoạn 10 giây: **0.91 s** (xấp xỉ)
- Nhận xét:
  - Mô hình nhận dạng **chính xác tuyệt đối** so với transcript gốc.
  - Trong điều kiện yên tĩnh, hiệu năng rất tốt, không xuất hiện lỗi thay thế/chèn/xóa từ.

### 2.2. File 2 – Môi trường có tạp âm nền, lời nói tự nhiên
- Tên file: `2.moi_truong_tap_am_nen_16k.wav`
- Thời lượng: **13.40 s**
- WER: **0.0536** (~5.36%)
- Độ trễ cho đoạn 10 giây: **1.32 s** (xấp xỉ)
- Đặc điểm nội dung:
  - Lời nói tự nhiên, nhiều từ khẩu ngữ, lặp từ, câu dài.
- Nhận xét:
  - Mô hình vẫn **giữ được độ chính xác cao** với WER ~5%, chấp nhận được cho ứng dụng thực tế.
  - Một số lỗi chủ yếu là:
    - Nhận nhầm/biến thể nhẹ ở các từ không quá quan trọng về mặt nghĩa (ví dụ: "nãy" ↔ "nải").
  - Độ trễ khoảng 1.3 giây cho đoạn chuẩn hóa 10 giây là **tương đối tốt** cho nhiều ứng dụng gần thời gian thực.

### 2.3. File 3 – Từ ngữ chuyên ngành
- Tên file: `3.tu_ngu_chuyen_nganh_16k.wav`
- Thời lượng: **6.69 s**
- WER: **0.0714** (~7.14%)
- Độ trễ cho đoạn 10 giây: **1.52 s** (xấp xỉ)
- Đặc điểm nội dung:
  - Chứa nhiều tên riêng và từ kỹ thuật: "Microsoft", "Thành phố Hồ Chí Minh", "công suất phản kháng", "blockchain", tên chuyên gia.
- Nhận xét:
  - Mô hình vẫn hiểu đúng **ngữ cảnh tổng thể** nhưng có một số lỗi ở từ chuyên ngành:
    - Ví dụ: "blockchain" bị nhận thành cụm từ khác ("blogger trang" trong kết quả).
  - Đây là nhóm lỗi thường gặp khi mô hình gặp từ mới, từ mượn tiếng Anh, hoặc thuật ngữ ít phổ biến trong dữ liệu huấn luyện.

## 3. Tổng quan hiệu năng

### 3.1. Độ chính xác (WER)
- Trung bình trên 3 file thử nghiệm:
  - File 1 (yên tĩnh): **0%**
  - File 2 (tạp âm, khẩu ngữ): **~5.36%**
  - File 3 (chuyên ngành): **~7.14%**
- Nhìn chung:
  - Trong điều kiện **thu âm tốt, ít nhiễu**, mô hình cho **độ chính xác rất cao**.
  - Khi gặp **tạp âm** hoặc **thuật ngữ chuyên ngành**, WER tăng nhưng vẫn trong mức **<10%**, đủ tốt cho nhiều bài toán nhận dạng tiếng nói tiếng Việt.

### 3.2. Độ trễ xử lý 10 giây
- Độ trễ đo được cho đoạn audio chuẩn hóa 10 giây:
  - Từ khoảng **0.9 s** đến **1.5 s**.
- Điều này cho thấy:
  - Mô hình có thể xử lý gần **thời gian thực (real-time)** cho đoạn âm thanh 10 giây.
  - Hoàn toàn khả thi để tích hợp vào các hệ thống trợ lý ảo, nhập liệu bằng giọng nói, hoặc ứng dụng phân tích cuộc gọi.

## 4. Kết luận và gợi ý ứng dụng
- PhoWhisper (phiên bản `medium`) cho thấy:
  - **Độ chính xác cao** trên tiếng Việt trong môi trường yên tĩnh.
  - **Khả năng chịu nhiễu tốt**, WER vẫn thấp trong bối cảnh thực tế có tạp âm.
  - **Độ trễ thấp (~1–1.5 s cho 10 s audio)**, phù hợp cho ứng dụng gần thời gian thực.
- Hạn chế:
  - Vẫn còn lỗi với **thuật ngữ chuyên ngành** và một số tên riêng.
- Gợi ý cải thiện:
  - Bổ sung/cải thiện dữ liệu huấn luyện với **từ vựng chuyên ngành** (ví dụ tài chính, điện lực, blockchain…).
  - Kết hợp với **từ điển tùy chỉnh** hoặc cơ chế hậu xử lý để sửa các từ mượn tiếng Anh, tên riêng.

## 5. Tệp kết quả
- Bảng kết quả chi tiết được lưu trong file `phowhisper_metrics.csv`.
- Các cột chính:
  - `audio_name`: tên file âm thanh sau chuẩn hóa 16 kHz
  - `duration_s`: thời lượng (giây)
  - `wer`: Word Error Rate của từng file
  - `latency_s`: thời gian mô hình xử lý đoạn 10 giây (giây)
  - `reference_text`: transcript gốc
  - `predicted_text`: transcript mô hình dự đoán
