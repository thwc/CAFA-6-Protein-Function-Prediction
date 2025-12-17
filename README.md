# CAFA 6 Protein Function Prediction

Dự án này chứa mã nguồn cho cuộc thi **CAFA 6 Protein Function Prediction**. Giải pháp được xây dựng dựa trên sự kết hợp đa dạng các phương pháp:
* **Tra cứu dữ liệu:** Exact Match (Khớp chuỗi chính xác).
* **Machine Learning:** kNN (K-Nearest Neighbors).
* **Deep Learning:** MLP, 1D-CNN + SE-ResNet-MLP, Deep Residual MLP.

Tất cả các dự đoán từ các mô hình trên cuối cùng được tổng hợp tối ưu bằng phương pháp **Ensemble** để đưa ra kết quả cuối cùng.

## Yêu cầu hệ thống (Prerequisites)

Để chạy code trên máy cục bộ (Local Machine), bạn cần cài đặt:

* **OS:** Linux (Ubuntu 20.04+) hoặc Windows 10/11 (khuyên dùng WSL2).
* **Python:** 3.8 trở lên.
* **GPU:** Khuyến nghị NVIDIA GPU với VRAM tối thiểu 8GB (để train các model ResNet/CNN). Cần cài đặt CUDA tương thích với phiên bản PyTorch.

## Cài đặt (Installation)

1.  **Clone repository:**
    ```bash
    git clone https://github.com/thwc/CAFA-6-Protein-Function-Prediction.git
    ```

2.  **Tạo môi trường ảo (Virtual Environment):**
    Khuyên dùng Anaconda hoặc venv để quản lý thư viện.
    ```bash
    python -m venv venv
    # Windows:
    .\venv\Scripts\activate
    # Linux/Mac:
    source venv/bin/activate
    ```

3.  **Cài đặt các thư viện cần thiết:**
    ```bash
    pip install -r requirements.txt
    ```

## Cấu trúc thư mục & Dữ liệu (Data Setup)

Do kích thước lớn, thư mục `input` và `output` **không** được bao gồm trong repo này. Bạn cần thiết lập thủ công như sau:

### 1. Tải dữ liệu

#### Dataset chính
- **CAFA 6 Competition dataset**: [Kaggle CAFA 6 Competition](https://www.kaggle.com/competitions/cafa-6-protein-function-prediction)

#### Các dataset bổ sung
- **blast-quick-sprof-zero-pred**: [Link Kaggle](https://www.kaggle.com/code/taylorsamarel/cafa-6-protein-function-starter-eda-model)
- **cafa-5-ems-2-embeddings-numpy**: [Link Kaggle](https://www.kaggle.com/datasets/viktorfairuschin/cafa-5-ems-2-embeddings-numpy)
- **cafa-6-t5-embeddings**: [Link Kaggle](https://www.kaggle.com/datasets/lolik228/cafa-6-t5-embeddings)
- **cafa6-t5-embeddings**: [Link Kaggle](https://www.kaggle.com/datasets/anoreo/cafa6-t5-embeddings)
- **protein-go-annotations**: [Link Kaggle](https://www.kaggle.com/datasets/seddiktrk/protein-go-annotations)
- **train_targets_top500**: [Link Kaggle](https://www.kaggle.com/datasets/siddhvr/train-targets-top500)

### 2.  **Sắp xếp thư mục:** Tạo thư mục `input` tại thư mục gốc và giải nén dữ liệu vào đó.

Cấu trúc thư mục dự kiến:

```text
|   README.md
|   requirements.txt
|   
+---1_train_models
|       01_Basic-MLP.ipynb
|       02_1D-CNN_SE-ResNet-MLP.ipynb
|       03_Plain-ResNet-MLP.ipynb
|       04_KNN_ExactMatch.ipynb
|
+---2_ensemble
|       05_Ensemble_Blending.ipynb
|
+---input
|   |   .gitkeep
|   |   blast-quick-sprof-zero-pred.zip
|   |   cafa-5-ems-2-embeddings-numpy.zip
|   |   cafa-6-t5-embeddings.zip
|   |   cafa6-t5-embeddings.zip
|   |   protein-go-annotations.zip
|   |   train_targets_top500.zip
|   |
|   +---blast-quick-sprof-zero-pred
|   |       submission.tsv
|   |
|   +---cafa-5-ems-2-embeddings-numpy
|   |       test_embeddings.npy
|   |       test_ids.npy
|   |       train_embeddings.npy
|   |       train_ids.npy
|   |
|   +---cafa-6-protein-function-prediction
|   |   |   IA.tsv
|   |   |   sample_submission.tsv
|   |   |
|   |   +---Test
|   |   |       testsuperset-taxon-list.tsv
|   |   |       testsuperset.fasta
|   |   |
|   |   \---Train
|   |           go-basic.obo
|   |           train_sequences.fasta
|   |           train_taxonomy.tsv
|   |           train_terms.tsv
|   |
|   +---cafa-6-t5-embeddings
|   |       test_embeds.npy
|   |       test_ids.npy
|   |       train_embeds.npy
|   |       train_ids.npy
|   |
|   +---cafa6-t5-embeddings
|   |       test_embeddings_esm2.npy
|   |       test_ids_esm2.npy
|   |       train_embeddings_esm2.npy
|   |       train_ids_esm2.npy
|   |       __huggingface_repos__.json
|   |
|   +---protein-go-annotations
|   |       goa_uniprot_all.csv
|   |
|   \---train_targets_top500
|           train_targets_top500.npy
|
+---models
|       cnn_best.pth
|
\---output
        .gitkeep
        submission.tsv
        submission_mlp.tsv
        submission_hybrid.tsv
        submission_resnet.tsv
        submission_exact.tsv
        submission_knn.tsv
```        
## Hướng dẫn chạy

1. **Train models**
   - Chạy lần lượt các notebook trong thư mục `1_train_models`.
   - Mỗi notebook sẽ tạo ra một file `submission_*.tsv`, trừ notebook `04_KNN_ExactMatch.ipynb` tạo 2 file.

2. **Tạo file submission cuối cùng**
   - Sau khi có các file `submission_*.tsv`, chạy notebook `05_Ensemble_Blending.ipynb`.
   - Notebook này sẽ kết hợp các submission trước để tạo ra file `submission.tsv` cuối cùng, sẵn sàng để nộp.
