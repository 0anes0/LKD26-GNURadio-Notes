
##  Giriş
---

## 📝 Teorik Temeller

### 1. Neden Sayısal Modülasyon?
Analog sinyaller gürültüye karşı dayanıksızdır. Sayısal haberleşmede ise bilgiyi "Sembollere" kodlarız.
* **Bit:** 0 veya 1.
* **Sembol:** Hat üzerinden gönderilen dalga formu.

### 2. QPSK (Quadrature Phase Shift Keying)
Hocanın üzerinde durduğu temel modülasyon tipi.
* **Mantık:** Taşıyıcı sinyalin **fazını** 4 farklı açıya (45°, 135°, 225°, 315°) kaydırarak bilgi taşırız.
* **Verimlilik:** Her sembol **2 bit** taşır (00, 01, 10, 11). BPSK'ya göre bant genişliğini değiştirmeden hızı 2 katına çıkarır.

### 3. Darbe Şekillendirme (Pulse Shaping)
Kare dalga (0 ve 1'ler) frekans spektrumunda sonsuz bant genişliği kaplar (Sinc fonksiyonu). Yan kanallara taşmayı önlemek için sinyali "yumuşatmamız" gerekir.
* **Kullanılan Filtre:** Root Raised Cosine (RRC).
* **Amaç:** Bant genişliğini sınırlamak ve Semboller Arası Girişimi engellemek.

---

## Uygulama Analizi: `QPSKDemo.grc`

### 1. Veri Kaynağı (TX - Verici)
* **Random Source:** Rastgele bitler üretir. (Bizim verimiz bu).
* **QPSK Mod:** Bitleri sembollere çevirir (Gri Kodlaması kullanılır).
* **RRC Filter (Interpolation):** Sinyali yumuşatır ve örnekleme hızını (Samples per Symbol - SPS) ayarlar. Dosyada `sps=4` olarak belirlenmiş.

### 2. Kanal Simülasyonu (Ortamı Bozma)
Gerçek dünya şartlarını simüle etmek için sinyali bilerek bozduğumuz blok:
* **Channel Model:**
    * **Noise Voltage:** Ortama "Beyaz Gürültü" (AWGN) ekler. Değer arttıkça noktalar dağılır.
    * **Frequency Offset:** Verici ve alıcı arasındaki frekans uyumsuzluğunu simüle eder. Değer 0'dan farklıysa takımyıldız **dönmeye başlar**.

### 3. Görüntüleme ve Analiz (RX - Alıcı)
* **Constellation Sink (Takımyıldız Diyagramı):**
    * I ve Q bileşenlerini X-Y düzleminde gösterir.
    * **İdeal Durum:** 4 net nokta.
    * **Gürültülü Durum:** 4 tane "bulut".
    * **Faz Kayması:** Noktaların kendi etrafında dönmesi.

* **Eye Diagram (Göz Diyagramı):**
    * Sinyallerin üst üste binmiş halidir.
    * **Göz Açıklığı:** Göz ne kadar açıksa, sinyal o kadar temizdir. Göz kapanırsa **ISI (Girişim)** var demektir, veri okunamaz.

---

## 🧪 Laboratuvar Deney Gözlemleri

Akış diyagramı çalışırken `QT GUI` üzerindeki "Slider"lar ile şu testleri yaptık:

1.  **Gürültü Testi:**
    * `Noise Voltage` artırıldığında Constellation noktaları genişleyip birbirine karışmaya başladı.
    * **Sonuç:** SNR düştükçe bit hatası (BER) artar.

2.  **Frekans Kayması Testi:**
    * `Freq Offset` değeri çok az artırıldığında (örn: 0.001) noktalar yavaşça dönmeye başladı.
    * **Mühendislik Çözümü:** Alıcıda bunu durdurmak için "Costas Loop" gibi senkronizasyon blokları gerekir (İleriki derslerin konusu).

3.  **Zamanlama Hatası:**
    * `Time Offset` ile oynadığımızda Göz Diyagramındaki "göz" kapandı.
    * **Sonuç:** Doğru anda örnekleme yapılmazsa semboller yanlış okunur.

---

## ⚠️ Kritik Kavramlar Sözlüğü

| Terim | Açıklama |
| :--- | :--- |
| **ISI (Inter-Symbol Interference)** | Bir sembolün enerjisinin diğerine taşması. |
| **Baud Rate** | Saniyedeki sembol sayısı. |
| **RRC (Root Raised Cosine)** | İdeal bant genişliği filtresi. ($\alpha$ parametresi keskinliği belirler). |
| **AWGN** | Additive White Gaussian Noise (Standart arka plan gürültüsü). |
