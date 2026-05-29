# Anomaly Detection in Mobile Networks
### Reproduction Experiment — CUSUM + LCL + Robust Stat Pipeline

> Tái hiện thực nghiệm từ bài báo:
> **"Advanced Anomaly Detection in Mobile Networks: A Hybrid Approach Based on Statistical and Machine Learning Techniques"**
> Nabil M., Hnida M., Haqiq A., Hilal I. — iJIM Vol.19 No.13 (2025)
> https://doi.org/10.3991/ijim.v19i13.54539

---

## Giới thiệu

Bài báo gốc đề xuất pipeline kết hợp 3 phương pháp thống kê để phát hiện bất thường trong traffic mạng di động:CUSUM → Lower Control Limit (LCL) → Robust Stat Detector
Do bài báo không công bố source code và dataset gốc, thực nghiệm này tự xây dựng lại toàn bộ pipeline dựa trên mô tả trong bài báo và kiểm chứng trên 2 dataset công khai thay thế.

---

## Dataset sử dụng


CESNET-TimeSeries24 nguồn [Zenodo](https://zenodo.org/records/13382427)  283 institutions, traffic theo giờ, 40 tuần | So sánh đóng góp từng bước 
NAB Nguồn [GitHub](https://github.com/numenta/NAB)  58 time series, nhãn thật từ chuyên gia | Đánh giá với ground truth |

---

## Kết quả

### Phần 1: CESNET (Unsupervised)
So sánh hiệu quả của từng bước trong pipeline:

| Phương pháp                     | Bất thường | Bình thường | Tỉ lệ  |
|---------------------------------|------------|-------------|--------|
| Mức 1: CUSUM alone              | 283        |      0      | 100.0% |
| Mức 2: CUSUM + LCL              | 26         |     257     | 9.2%   |
| Mức 3: CUSUM + LCL + RobustStat | 26         |     257     | 9.2%   |

→ LCL giảm false positive hơn 10 lần (từ 100% xuống 9.2%)

### Phần 2: NAB (Supervised)
Đánh giá với nhãn thật từ chuyên gia Numenta:

| Chỉ số | Bài báo gốc | Thực nghiệm |
|----------|-----|--------|
| Accuracy | 98% | 90.03% |
| Recall   | 98% | 0.25%  |
| F1 Score |  —  | 0.0048 |

> Kết quả thấp hơn bài báo vì NAB chứa anomaly tức thời (vài phút) trong khi CUSUM được thiết kế cho anomaly kéo dài (nhiều ngày) — đây là hạn chế do không có dataset gốc của bài báo.

---
---

## Cách chạy

### Trên Google Colab
1. Mở file `notebooks/anomaly_detection_experiment.ipynb`
2. Upload lên [Google Colab](https://colab.research.google.com)
3. Chạy theo thứ tự từ CELL 1 đến CELL 15

### Cài đặt local
```bash
pip install -r requirements.txt
jupyter notebook notebooks/anomaly_detection_experiment.ipynb
```

---
