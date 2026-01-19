# ---# Vehicle-Licence-System

![Python](https://img.shields.io/badge/Python-3.x-blue?style=for-the-badge&logo=python)
![YOLOv8](https://img.shields.io/badge/YOLOv8-Object_Detection-purple?style=for-the-badge)
![EasyOCR](https://img.shields.io/badge/EasyOCR-Text_Recognition-green?style=for-the-badge)

<br>

<div align="center">
  <img src="assets/yasakli_arac_ornegi.png" width="700" alt="Sistem Çıktısı">
  <p><em>Yapay zeka tabanlı araç tanıma, plaka okuma ve erişim kontrol sistemi.</em></p>
</div>


Vehicle System with YOLOV,VLM and OCR

Bu proje, güvenlik kapılarındaki araç giriş-çıkış süreçlerini otomatize etmek için geliştirilmiş Çok Modelli (Multi-Model) bir yapay zeka sistemidir. Sistem; araç tespiti, plaka okuma ve görsel betimleme yeteneklerini birleştirerek araçların geçiş iznini yönetir.

# Proje Özellikleri

Bu sistem 3 farklı yapay zeka disiplinini tek bir akışta birleştirir:

Nesne Tespiti (Object Detection): Kamera görüntüsündeki araçları (Araba, Otobüs, Kamyon) tespit eder ve konumlarını belirler.

Optik Karakter Tanıma (OCR): Araç üzerindeki plakayı okuyarak metne dönüştürür.

Görsel Dil Modelleme (VLM): Aracı analiz eder ve fiziksel özellikleri hakkında (örn. "kırmızı spor araba") yazılı betimleme yapar.

Kural Tabanlı Erişim Kontrolü: Belirlenen kurallara göre (Kamyon yasağı, İzinli plaka listesi) kapıyı açar veya reddeder.


# Kullanılan Teknolojiler ve Kütüphaneler

Ultralytics YOLOv8: Yüksek hızlı ve hassas araç tespiti için.

EasyOCR: Plaka üzerindeki metinleri okumak için.

Hugging Face Transformers (BLIP): Görseli metne dökmek (Image Captioning) için.

FiftyOne: COCO veri setinden test görsellerini indirmek ve yönetmek için.

OpenCV & Matplotlib: Görüntü işleme ve görselleştirme için.

PyTorch: Derin öğrenme modellerinin altyapısı için.

# Proje Google Colab üzerinde çalışacak şekilde tasarlanmıştır ancak yerel ortamda da çalıştırılabilir.
pip install ultralytics
pip install easyocr
pip install transformers torch torchvision "fiftyone[desktop]"

# Nasıl Çalışır? (Sistem Mantığı)
Sistem aşağıdaki adımları sırasıyla uygular:

Görüntü Alma: Test görselleri (COCO dataset veya kullanıcı yüklemesi) sisteme verilir.

Tespit (YOLOv8n): Model, görüntüdeki Car (2), Bus (5) ve Truck (7) sınıflarını arar.

Kırpma (Cropping): Tespit edilen aracın olduğu bölge (bounding box) kesilir.

#Analiz:

VLM: Kesilen görüntü Salesforce/blip-image-captioning-base modeline gönderilir ve araç betimlenir.

OCR: Kesilen görüntü EasyOCR ile taranır ve plaka metni çıkarılır.

#Karar Mekanizması (Logic):

Eğer araç Kamyon (Truck) ise -> ⛔ DENIED (Giriş Yasak)

Eğer araç Araba/Otobüs ise ve plaka İzinli Listede (Whitelist) varsa -> ✅ ALLOWED (Kapı Açılıyor)

Plaka listede yoksa -> ⛔ DENIED (Kayıtlı Değil)

# Konfigürasyon
 İzin verilen plakalar listesi
ALLOWED_PLATES = ["16ACD433", "34TB123"]

 Yasaklı veya hedef araç sınıfları (COCO ID'leri)
 2: Car, 5: Bus, 7: Truck
TARGET_CLASSES = [2, 5, 7]

Kullanım (Örnek Çıktı)
Kod çalıştırıldığında, tespit edilen her araç için konsola detaylı bir rapor basar ve görsel üzerinde işaretleme yapar.

Konsol Çıktısı:
--- TESPİT EDİLEN ARAÇ: Car ---
VLM Betimlemesi: a silver sedan parked on the street
Okunan Plaka (OCR): 16ACD433
KARAR: Plaka (16ACD433) onaylı. Kapı açılıyor.
