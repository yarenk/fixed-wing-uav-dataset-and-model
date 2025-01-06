# ***Sabit Kanatlı İHA Veri Seti ve Modeli***

Bu depo, YOLOv5 kullanılarak eğitilmiş sabit kanatlı İHA'lara (İnsansız Hava Araçları) ait etiketlenmiş görüntülerin yer aldığı bir veri seti içermektedir. Veri seti, nesne tespiti görevleri için uygundur ve İHA tanıma amacıyla derin öğrenme modellerini eğitmek, doğrulamak ve test etmek için kullanılabilir. Bu çalışmayı Teknofest Savaşan İHA yarışması için oluşturdum.

## ***Datasete buradan ulaşabilirsiniz: [Dataset](https://github.com/yarenk/fixed-wing-uav-dataset-and-model/releases/tag/version1)***

## Veri Seti Genel Bakış
Veri seti, sabit kanatlı İHA'ların sınıf etiketleri ve sınır kutuları ile etiketlenmiş görüntülerini içerir. YOLOv5 eğitim hattıyla uyumlu olacak şekilde yapılandırılmıştır.

### Özellikler:
- **Görüntü Formatı**: JPEG/PNG
- **Etiket Formatı**: YOLO formatı (Her görüntü için sınıf ve sınır kutusu koordinatlarını içeren TXT dosyaları)
- **Sınıflar**: iha
- **Bölünme**: Eğitim, doğrulama ve test setleri dahildir.

## Dizin Yapısı
Veri setinin dizin yapısı aşağıdaki gibidir:

```
┌── dataset/
│   ├── train/
│   │   ├── images/        # Eğitim görüntüleri
│   │   └── labels/          # Doğrulama görüntüleri
│   ├── val/
│   │   ├── images/        # Eğitim için YOLO formatındaki etiketler
│   │   └── labels/          # Doğrulama için YOLO formatındaki etiketler
└── README.md         # Veri seti açıklaması
```

## Veri Seti Hazırlığı ve Eğitim Süreci

1. **Veri Etiketleme**:
   Görüntüler, sabit kanatlı İHA'ların etrafında sınır kutuları oluşturmak için [Roboflow](https://app.roboflow.com) kullanılarak manuel olarak etiketlendi. Etiketler, YOLO formatında kaydedildi ve YOLOv5 eğitimine uygun hale getirildi.

2. **Veri Seti Bölünmesi**:
   Eğitim sürecinde doğru değerlendirme sağlamak amacıyla veri seti eğitim ve doğrulama olarak bölündü. Verilerin yaklaşık %90'ı train için, %10'u validation için ayrıldı.

3. **Model Eğitimi**:
   YOLOv5 modeli, hazırlanan veri seti kullanılarak eğitildi. Eğitim yapılandırması şunları içeriyordu:
   - **Model**: YOLOv5s
   - **Görüntü Boyutu**: 640x640
   - **Batch Boyutu**: 16
   - **Epoch**: 100
   - **Optimizatör**: Öğrenme oranı zamanlayıcısıyla SGD

   Eğitim süreci, hesaplamayı hızlandırmak için bir GPU üzerinde gerçekleştirildi. Model, İHA tespiti görevleri için uygun hale getiren yüksek performans metriklerine ulaştı.

4. **Değerlendirme**:
   Eğitilmiş model doğrulama seti üzerinde değerlendirildi. Performansı ölçmek için Doğruluk (Precision), Hatırlama (Recall) ve Ortalama Ortalama Doğruluk (mAP) gibi metrikler hesaplandı. Sonuçlar şu şekildedir:
   - **Doğruluk (Precision)**: %99.2
   - **Hatırlama (Recall)**: %98.6
   - **mAP**: %98.4
   Eğitilen modeli best.pt dosyasını indirerek kullanbilirsiniz.

## Örnek Görüntüler
![Örnek](https://github.com/yarenk/fixed-wing-uav-dataset-and-model/blob/main/Example.png)
![Metrics](https://github.com/yarenk/fixed-wing-uav-dataset-and-model/blob/main/metrics.png)

## Teşekkür
Bu veri setinin oluşturulmasında katkıda bulunanlara ve şu araçlara teşekkür ederim:
- [YOLOv5](https://github.com/ultralytics/yolov5)
- Etiketleme araçları [Roboflow](https://app.roboflow.com)

## İletişim
Bu veri setiyle ilgili herhangi bir sorunuz veya öneriniz varsa, lütfen bir konu açın ya da [ykoc7070@gmail.com](mailto:ykoc7070@gmail.com) adresinden bana ulaşın.

---

Sabit kanatlı İHA veri setine ilgi gösterdiğiniz için teşekkürler!
