##  Günün Özeti
Bu laboratuvar gününde iki farklı dünyaya odaklandık:
1.  **Öğleden Önce:** Mevcut SDR ağlarını kullanmak OpenWebRX ve gerçek dünya sinyallerini (ADS-B) yakalamak.
2.  **Öğleden Sonra:** Modern haberleşmenin temeli olan sayısal modülasyon tekniklerini (QPSK) analiz etmek.

---

#  Bölüm 1: Remote SDR ve Havacılık (Sabah Oturumu)

Kendi donanımımız sınırlı olduğunda veya anten kuramadığımızda, internet üzerinden başka SDR'lara erişebiliriz.

### 1. Web Tabanlı SDR Sistemleri
* **WebSDR:** Tarayıcı üzerinden (HTML5) dünyanın farklı yerlerindeki amatör telsiz istasyonlarını dinlememizi sağlar. Genelde HF (Kısa Dalga) bantlarında popülerdir.
* **OpenWebRX:** Daha modern, Python tabanlı ve modüler bir yapıdır. Sadece sesi değil, üzerindeki dijital modları (DMR, YSF, FT8) da tarayıcı içinde çözebilir.
    * *Kullanım Amacı:* Kendi yaptığımız vericinin (TX) sinyalinin dünyanın öbür ucuna ulaşıp ulaşmadığını (Propagasyon testi) kontrol etmek.

### 2. ADS-B ile Uçak Takibi (1090 MHz)
**ADS-B (Automatic Dependent Surveillance–Broadcast)**, uçakların kimlik, konum, hız ve irtifa bilgilerini yayınladığı sistemdir.

**Kullanılan Donanım ve Yazılım:**
* **Donanım:** RTL-SDR (RTL2832U)
* **Yazılım:** SDRangel (Gelişmiş SDR alıcı/verici yazılımı)
* **Anten:** 1090 MHz uyumlu dikey anten.

**Deney Sonuçları:**
* RTL-SDR ile 1090 MHz frekansı dinlendi.
* **SDRangel** üzerindeki ADS-B demodülatörü aktif edildi.
* Harita eklentisi kullanılarak, laboratuvarın üzerinden geçen uçaklar gerçek zamanlı olarak haritada tespit edildi.

---

#  Bölüm 2: Sayısal Haberleşme ve QPSK (Öğleden Sonra)

Analog dünyadan çıkıp bitlerin dünyasına (0 ve 1) geçiş yaptık. Odak noktamız **QPSK** modülasyonuydu.

### 1. Neden Sayısal?
Analog sinyal bozulursa "cızırtı" duyarsın, anlaşılır. Sayısal sinyal bozulursa veri tamamen kaybolur. Bu yüzden **Eye Diagram (Göz Diyagramı)** ve **Constellation (Takımyıldız)** gibi analiz araçlarına muhtacız.

### 2. QPSK (Quadrature Phase Shift Keying)
* **Mantık:** Taşıyıcının fazını 4 farklı açıya (45°, 135°, 225°, 315°) kaydırarak bilgi taşır.
* **Verim:** Her sembol **2 bit** taşır (00, 01, 10, 11).

### 3. Uygulama Analizi: `QPSKDemo.grc`
Derste incelediğimiz GNU Radio akış diyagramının detayları:

* **Verici (TX):**
    * `Random Source`: Rastgele bit üretir.
    * `RRC Filter (Root Raised Cosine)`: **Darbe Şekillendirme** yapar. Kare dalgayı yumuşatarak bant genişliğini sınırlar ve Semboller Arası Girişimi (ISI) önler.

* **Kanal (Simülasyon):**
    * `Noise Voltage`: Ortama **AWGN (Beyaz Gürültü)** ekler. Artırıldığında Constellation noktaları dağılır.
    * `Freq Offset`: Alıcı-verici frekans uyumsuzluğunu simüle eder. Değer girildiğinde noktalar **dönmeye başlar**.

* **Alıcı (RX - Görüntüleme):**
    * **Constellation Sink:** I/Q verisini X-Y düzleminde çizer. İdealde 4 net nokta görmeliyiz.
