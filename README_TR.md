# Sabit Kanatlı İHA Tespiti — Veri Seti ve Model

![Model](https://img.shields.io/badge/model-YOLOv5s-blue)
![Task](https://img.shields.io/badge/g%C3%B6rev-nesne%20tespiti-orange)
![License](https://img.shields.io/badge/kod-MIT-green)
![Dataset](https://img.shields.io/badge/veri%20seti-CC%20BY%204.0-lightgrey)

**Sabit kanatlı İHA tespiti** için açık kaynak etiketli görüntü veri seti ve eğitilmiş YOLOv5s modeli.
Teknofest Savaşan İHA Yarışması için geliştirildi; modelin görevi, uçuş sırasında kamera görüntüsünden
rakip sabit kanatlı hava aracını gerçek zamanlı olarak tespit etmekti.

🇬🇧 For English: [README.md](README.md)

---

## İçerik

| Dosya | Açıklama |
|---|---|
| `best.pt` | Eğitilmiş YOLOv5s ağırlıkları |
| [Veri seti (Release v1)](https://github.com/yarenk/fixed-wing-uav-dataset-and-model/releases/tag/version1) | Etiketli görüntüler + YOLO formatında anotasyonlar |
| `metrics.png` | Eğitim eğrileri ve değerlendirme metrikleri |
| `Example.png` | Örnek tespit çıktıları |

---

## Veri Seti

| | |
|---|---|
| **Toplam görüntü** | TODO_GORSEL_SAYISI |
| **Sınıf sayısı** | 1 (`iha` — sabit kanatlı İHA) |
| **Görüntü formatı** | JPEG / PNG |
| **Anotasyon formatı** | YOLO (her görüntü için bir `.txt`: sınıf + normalize sınırlayıcı kutu) |
| **Bölünme** | TODO_BOLUNME_ORANI (eğitim / doğrulama) |
| **Etiketleme aracı** | [Roboflow](https://roboflow.com), elle sınırlayıcı kutu |

### Dizin yapısı

```
dataset/
├── train/
│   ├── images/     # Eğitim görüntüleri
│   └── labels/     # Eğitim görüntülerinin YOLO formatındaki anotasyonları
└── val/
    ├── images/     # Doğrulama görüntüleri
    └── labels/     # Doğrulama görüntülerinin YOLO formatındaki anotasyonları
```

### `dataset.yaml`

```yaml
path: ./dataset
train: train/images
val: val/images

nc: 1
names: ['iha']
```

---

## Sonuçlar

Doğrulama kümesi üzerinde ölçülmüştür:

| Metrik | Değer |
|---|---|
| Precision (Kesinlik) | %99.2 |
| Recall (Duyarlılık) | %98.6 |
| mAP@0.5 | %98.4 |

Eğitim eğrileri `metrics.png` dosyasındadır.

> **Bu rakamlar hakkında bir not.** Değerler ayrı tutulmuş bir test kümesinden değil, doğrulama
> kümesinden gelmektedir ve problem, görece homojen sahneler üzerinde tek sınıflı bir tespit
> problemidir. Bu sonuçlar "model bu veri setini iyi öğrendi" şeklinde okunmalıdır; herhangi bir
> görüntü üzerinde aynı performansın garantisi değildir. [Kısıtlar](#kısıtlar) bölümüne bakınız.

---

## Hızlı Başlangıç

### Çıkarım (PyTorch Hub)

```python
import torch

model = torch.hub.load('ultralytics/yolov5', 'custom', path='best.pt')
model.conf = 0.25  # güven eşiği

results = model('goruntunuz.jpg')
results.print()
results.show()
results.pandas().xyxy[0]  # tespitler DataFrame olarak
```

### Çıkarım (YOLOv5 komut satırı)

```bash
git clone https://github.com/ultralytics/yolov5
cd yolov5 && pip install -r requirements.txt

python detect.py \
  --weights ../best.pt \
  --source ../goruntuleriniz/ \
  --img 640 \
  --conf 0.25
```

### Eğitimi tekrarlamak

```bash
python train.py \
  --img 640 \
  --batch 16 \
  --epochs 100 \
  --data dataset.yaml \
  --weights yolov5s.pt
```

---

## Eğitim Yapılandırması

| Ayar | Değer |
|---|---|
| Temel model | YOLOv5s |
| Görüntü boyutu | 640 × 640 |
| Batch boyutu | 16 |
| Epoch sayısı | 100 |
| Optimizasyon | Öğrenme oranı zamanlayıcılı SGD |
| Donanım | GPU |

---

## Kısıtlar

Kullanmadan önce bilinmesi gerekenler:

- **Tek sınıf.** Model yalnızca "sabit kanatlı İHA" ile arka planı ayırt eder. İHA tiplerini
  birbirinden ayırmaz ve döner kanatlı hava araçlarına ya da kuşlara karşı test edilmemiştir.
- **Yarışmaya özgü görüntüler.** Veri seti, Teknofest Savaşan İHA Yarışması koşullarını yansıtır:
  gündüz, açık gökyüzü ve sınırlı sayıda hava aracı. Karmaşık arka planlarda, düşük ışıkta veya
  alışılmadık bakış açılarında performans düşüşü beklenmelidir.
- **Metrikler doğrulama kümesinden.** [Sonuçlar](#sonuçlar) bölümündeki nota bakınız.
- **Küçük nesne performansı ölçülmedi.** Uzak mesafedeki tespit kalitesi ayrıca değerlendirilmemiştir.

Veri setini daha zor örneklerle genişleten katkılar memnuniyetle karşılanır.

---

## Lisans

- **Kod ve eğitilmiş ağırlıklar:** MIT Lisansı
- **Veri seti:** CC BY 4.0 — atıf verilerek serbestçe kullanılabilir

## Atıf

Bu veri setini veya modeli çalışmanızda kullanırsanız lütfen atıf verin:

```bibtex
@misc{koc_uav_dataset,
  author       = {Yaren Koç},
  title        = {Fixed-Wing UAV Detection Dataset and Model},
  year         = {2024},
  publisher    = {GitHub},
  howpublished = {\url{https://github.com/yarenk/fixed-wing-uav-dataset-and-model}}
}
```

## Teşekkür

- Ultralytics tarafından geliştirilen [YOLOv5](https://github.com/ultralytics/yolov5)
- Etiketleme altyapısı için [Roboflow](https://roboflow.com)
- HAVTEK Öğrenci Kulübü, Erciyes Üniversitesi

## İletişim

Soru ve önerileriniz için issue açabilir veya **TODO_EPOSTA** adresinden bana ulaşabilirsiniz.
