# SQLCoder V2 — NL → SQL Analist Ajanı
**Qwen2.5-7B + QLoRA + RAG + Schema Graph + Self-RAG + Güvenlik Doğrulama**

Bu proje, doğal dilde gelen soruları güvenli, şema-uyumlu ve açıklanabilir SQL sorgularına dönüştüren gelişmiş bir NL→SQL analist ajan mimarisi sunar.

---

## 🗂️ İçindekiler
1. Özet  
2. Öne Çıkanlar  
3. Mimari  
4. Dizin Yapısı  
5. Kurulum  
6. Dataset Hazırlığı  
7. Eğitim (Training)  
8. Inference Pipeline  
9. Güvenlik Katmanı  
10. Eval Suite  
11. Troubleshooting  
12. Lisans  

---

## 🔍 Özet
SQLCoder V2, kurumsal çok tablolı veritabanlarında doğru JOIN path, şema farkındalığı ve güvenli SQL üretimi sağlamak için tasarlanmıştır.

---

## 🚀 Mimari

![Architecture](./Untitled%20diagram-2025-11-28-111904.png)

---

## ✨ Öne Çıkanlar
- Schema extraction → tables.json  
- FK graph → otomatik JOIN path üretimi  
- Semantic chunking (table / FK / column level)  
- Schema-injected dataset üretimi  
- RAG (top-k retrieval, cosine similarity, metadata filtering)  
- Specialized SQL embeddings  
- Hierarchical retrieval  
- QLoRA Fine-Tuning (4-bit)  
- Self-RAG reflection  
- AST tabanlı SQL doğrulama  
- Güvenlik filtreleri (DROP/DELETE blokajı, LIMIT enforcement)

---

## 📚 Dizin Yapısı
```
sql_coder_V2/
├─ dataset.jsonl
├─ test_500.jsonl
├─ tables.json
├─ promt_base..txt
├─ 2711deneme.ipynb
├─ Untitled diagram-2025-11-28-111904.png
└─ LICENSE
```

---

## ⚙️ Kurulum
```
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

---

## 📊 Dataset Hazırlığı
- schema-injected NL→SQL pairs  
- alias normalization  
- cutoff_len: 512/1024  
- FK-based chunking  

Dataset formatı:
```
{"instruction": "...", "schema": "...", "sql": "..."}
```

---

## 🧠 Eğitim (Training)
- r = 16–32  
- α = 16–64  
- dropout = 0.05  
- lr = 4e-4 → 3e-4  
- epochs = 3–5  
- bf16 + gradient checkpointing  

---

## 🤖 Inference Pipeline
1. Soru embedding oluşturma  
2. Domain-based logical routing  
3. RAG retrieval  
4. Fine-tuned model → SQL üretimi  
5. AST + FK + table/column existence check  
6. Güvenlik filtresi  
7. Sonuç döndürme  

---

## 🔒 Güvenlik Katmanı
- SELECT-only  
- DROP/DELETE engelleme  
- wildcard temizleme  
- LIMIT zorunluluğu  
- forbidden tables  
- column existence check  
- FK path doğruluğu  

---

## 📈 Eval Suite
```
python eval.py
```
Metrikler: Exec-Accuracy, AST Accuracy, Repair Rate, Reflection Loop Success.

---

## 🩹 Troubleshooting
- 100MB GitHub limiti → LFS veya zip dahil etme  
- CUDA OOM → batch/cutoff düşür  
- Yanlış JOIN path → FK graph güncelle  
- Düşük Retrieval kalite → k artır (k=8)  

---

## 📄 Lisans
MIT License.
