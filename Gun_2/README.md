
## Giriş
Bu bölüm, Yazılım Tabanlı Radyo (SDR) uygulamalarında kullanılan temel elektromanyetik teoriyi, anten tasarım hesaplamalarını ve mühendislikte kullanılan logaritmik güç ölçümlerini (dB) kapsar.

---

## Elektromanyetik Dalgalar ve Dalga Boyu

Haberleşme sistemlerinin temeli, elektromanyetik dalgaların uzayda yayılmasına dayanır. Bir mühendis için en kritik parametre **Dalga Boyu ($\lambda$)**'dur; çünkü anten boyutu doğrudan buna göre belirlenir.

### Formül ve Hesaplama
Dalga boyu, ışık hızının frekansa oranıdır.

$$\lambda = \frac{c}{f}$$

* **$\lambda$ (Lambda):** Dalga boyu (Metre)
* **$c$ (Işık Hızı):** $3 \times 10^8$ m/s (Boşlukta)
* **$f$ (Frekans):** Hertz (Hz)

### Örnek Mühendislik Hesabı (446 PMR Bandı)

1.  **Verilen:** $f = 446 \times 10^6$ Hz, $c = 3 \times 10^8$ m/s
2.  **Hesap:**
    $$\lambda = \frac{300}{446} \approx 0.67 \text{ metre} \quad (67 \text{ cm})$$

### Anten Tasarımına Etkisi
Antenler genellikle tam dalga boyunda değil, çeyrek ($\lambda/4$) veya yarım dalga ($\lambda/2$) boyunda rezonansa girecek şekilde tasarlanır.

* **Yarım Dalga Dipol (Half-Wave Dipole):**
    $$L = \frac{\lambda}{2} = \frac{67}{2} = 33.5 \text{ cm}$$
    *(Her bir kol ~16.75 cm)*

* **Çeyrek Dalga Monopol (Quarter-Wave Monopole):**
    Araç antenlerinde veya el telsizlerinde kullanılır.
    $$L = \frac{\lambda}{4} = \frac{67}{4} \approx 16.75 \text{ cm}$$

---

#### Desibel (dB)
İki güç değeri arasındaki oranı belirtir (Kazanç veya Kayıp). Birimsizdir.
$$dB = 10 \cdot \log_{10}\left(\frac{P_{cikis}}{P_{giris}}\right)$$

* **+3 dB:** Gücü 2 katına çıkarır ($2x$).
* **+10 dB:** Gücü 10 katına çıkarır ($10x$).
* **-3 dB:** Gücü yarıya düşürür ($0.5x$).

#### dBm (Desibel-Milliwatt)
Mutlak güç seviyesini belirtir. Referans noktası **1 mW**'dır.
$$P(dBm) = 10 \cdot \log_{10}\left(\frac{P(watt)}{1mW}\right)$$

* **0 dBm:** 1 mW
* **10 dBm:** 10 mW
* **20 dBm:** 100 mW
* **30 dBm:** 1000 mW (1 Watt)

> **Pratik Kural:** Sinyal zincirinde (Link Budget) kazançlar (amplifikatör) toplanır, kayıplar (kablo, filtre) çıkarılır.
> $$P_{rx} = P_{tx} + G_{tx} - L_{path} + G_{rx}$$

---

## Fourier Dönüşümü (FFT) ve Spektrum

GNU Radio'da gördüğümüz "Frequency Sink" veya "Waterfall" grafikleri, zaman alanındaki sinyali frekans alanına çeviren matematiktir.

### Zaman vs. Frekans Alanı
* **Zaman Alanı (Time Domain):** Osiloskop görüntüsü. Sinyalin voltajının zamana göre değişimi. $v(t)$.
* **Frekans Alanı (Frequency Domain):** Spektrum analizör görüntüsü. Sinyalin hangi frekans bileşenlerinden oluştuğu. $X(f)$.

### Matematiksel Temel (DFT)
SDR, sürekli sinyali değil, ayrık (dijital) sinyali işlediği için **Ayrık Fourier Dönüşümü (DFT)** kullanılır.

GNU Radio'da bu işlem, yüksek performanslı **FFT (Fast Fourier Transform)** algoritması ile yapılır.

---

## Filtreleme Teorisi

İstenmeyen sinyalleri bastırmak ve sadece ilgilendiğimiz frekansı almak için filtreler kullanılır.

| Filtre Tipi | İngilizce | Açıklama |
| :--- | :--- | :--- |
| **Alçak Geçiren** | Low Pass Filter (LPF) | Belirlenen frekansın ($f_c$) *altını* geçirir. |
| **Yüksek Geçiren** | High Pass Filter (HPF) | Belirlenen frekansın ($f_c$) *üstünü* geçirir. |
| **Bant Geçiren** | Band Pass Filter (BPF) | İki frekans ($f_1$ ve $f_2$) arasını geçirir. |
| **Bant Söndüren** | Band Stop / Notch | Belirli bir aralığı engeller (Örn: FM radyoyu bastırmak). |

### GNU Radio Uygulaması
* Blok: `Low Pass Filter`
* Parametreler:
    * **Cutoff Freq:** Kesim frekansı (-3dB noktası).
    * **Transition Width:** Geçiş genişliği (Filtrenin ne kadar "keskin" olduğu).

