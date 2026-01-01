Endüstriyel Kusur Tespiti için Self-Supervised Learning

SimCLR ve SimSiam Karşılaştırmalı Analizi

Bu proje, endüstriyel üretim ortamlarından elde edilen görüntüler üzerinde kusurlu / kusursuz ürün sınıflandırması yapan akademik bir derin öğrenme çalışmasıdır.
Çalışmada, Self-Supervised Learning (SSL) tabanlı iki yöntem olan SimCLR ve SimSiam modelleri karşılaştırmalı olarak analiz edilmiştir.

Amaç, etiketli verinin sınırlı olduğu senaryolarda hangi SSL yaklaşımının daha ayırt edici, stabil ve genellenebilir temsiller öğrendiğini ortaya koymaktır.

1. Problemin Tanımı

Endüstriyel kalite kontrol süreçlerinde kusurlu ürünlerin erken ve doğru tespiti, üretim maliyetlerinin düşürülmesi açısından kritik öneme sahiptir.

Temel Zorluklar:

Etiketli veri miktarının sınırlı olması

Kusur tiplerinin görsel olarak birbirine benzemesi

Modellerin kusurlu bölgelere gerçekten odaklanıp odaklanmadığının yorumlanabilirliği

Bu çalışma, bu zorlukları Self-Supervised Learning yaklaşımlarıyla ele almayı hedeflemektedir.

2. Kullanılan Veri Seti ve Ön İşleme

Veri Seti:

Endüstriyel üretim ortamlarından elde edilmiş görüntüler

İkili sınıflandırma: Kusurlu / Kusursuz

Ön İşleme ve Veri Artırma:

Görüntüler yeniden boyutlandırılmış ve normalize edilmiştir

Random flip, rotation ve color jitter gibi veri artırma teknikleri uygulanmıştır

SSL aşamasında, kontrastif öğrenmeye uygun iki farklı görüntü dönüşümü kullanılmıştır

3. Model Mimarisi ve Yaklaşım Gerekçesi
A. Self-Supervised Learning (SSL)
🔹 SimCLR (Contrastive Learning)

SimCLR, aynı görüntünün farklı augmentasyonlarını pozitif çift, farklı görüntüleri ise negatif çift olarak ele alarak kontrastif kayıp fonksiyonu ile temsil öğrenir.

Avantajı:

Kusurlu ve kusursuz bölgeler arasında daha belirgin ayrımlar öğrenir

Özellik uzayında sınıflar arası mesafe daha nettir

🔹 SimSiam (Non-Contrastive Learning)

SimSiam, negatif örneklere ihtiyaç duymadan, simetri kırma prensibi ile temsil öğrenir.

Avantajı:

Daha hızlı eğitim süresi

Daha az bellek ihtiyacı

B. Supervised Fine-Tuning

SSL ile ön-eğitilmiş encoder ağı, sınıflandırma başlığı eklenerek denetimli öğrenme ile fine-tune edilmiştir.

Optimizasyon: Adam

Kayıp Fonksiyonu: CrossEntropyLoss

Early Stopping: Aşırı öğrenmeyi önlemek için uygulanmıştır

Hiperparametre Optimizasyonu: Optuna (Learning Rate & Batch Size)

4. Çalıştırma Talimatları
Bağımlılıklar

Proje Python 3.x ve PyTorch ortamında geliştirilmiştir.
Gerekli kütüphaneler requirements.txt dosyasında listelenmiştir.

pip install -r requirements.txt

Eğitim Adımları
# SSL Pretraining
python ssl_pretrain/simclr.py
python ssl_pretrain/simsiam.py

# Fine-Tuning ve Değerlendirme
python classification/train.py

5. Model Çıktıları ve Metrikler
Nicel Sonuçlar (Test Seti)
Metrik	SimCLR	SimSiam
Accuracy	%98.30	%90.82
F1 Score (Macro)	0.98	0.92
IoU	0.91	0.85
Dice	0.98	0.92
Eğitim Stabilitesi	Yüksek	Orta
Eğitim Süresi	Daha Uzun	Daha Kısa
Görsel Analizler

t-SNE: Özellik uzayı dağılımı

Grad-CAM: Modelin odaklandığı kusurlu bölgeler

Yanlış Sınıflandırma Analizi: Hatalı tahmin edilen örneklerin incelenmesi

Tüm çıktılar ./outputs/ klasörü altında sunulmuştur.

6. Sonuç ve Değerlendirme

Genelleme başarımı ve sınıf ayrım gücü dikkate alındığında, kontrastif kayıp yapısı sayesinde daha ayrıştırıcı temsiller öğrenen SimCLR modelinin, endüstriyel kusur tespiti uygulamaları için SimSiam’a kıyasla daha uygun olduğu sonucuna varılmıştır.

SimSiam daha hızlı eğitilmesine rağmen, SimCLR daha stabil öğrenme süreci ve daha yüksek sınıflandırma performansı sergilemiştir.
