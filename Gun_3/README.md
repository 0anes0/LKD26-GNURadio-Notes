# 3. Gün: Analog Modülasyon Teorisi ve Spektral Analiz

## Giriş: Neden Modülasyon?
Haberleşme sistemlerinde bilgi sinyali (ses, veri) doğrudan iletilemez. Bunun temel fiziksel ve mühendislik sebepleri vardır:

1.  **Anten Boyutu:** $\lambda = c/f$ gereği, 3 kHz'lik ses sinyali için 100 km uzunluğunda anten gerekir. Frekans yükseltilerek ($f_c$) anten boyutu makul seviyelere (cm/m) indirilir.
2.  **Multiplexing (Çoklama):** Aynı ortamda (hava) farklı frekans bantları kullanılarak birden fazla kanalın çakışmadan iletilmesi (FDM).

---

## 1. Genlik Modülasyonu (Amplitude Modulation - AM)

AM, en ilkel ancak teoriyi anlamak için en kritik modülasyon türüdür. Bilgi sinyali $m(t)$, taşıyıcı sinyalin **genliğini** değiştirir.

### Matematiksel Model ve İspat
Taşıyıcı sinyal ($c(t)$) ve mesaj sinyali ($m(t)$) şu şekilde tanımlansın:
$$c(t) = A_c \cos(2\pi f_c t)$$
$$m(t) = A_m \cos(2\pi f_m t)$$

AM sinyali $s_{AM}(t)$ şu genel formülle ifade edilir:
$$s_{AM}(t) = [A_c + m(t)] \cos(2\pi f_c t)$$

Parantez içine $A_c$ parantezine alıp **Modülasyon İndeksi ($\mu = A_m / A_c$)** tanımını uygularsak:
$$s_{AM}(t) = A_c [1 + \mu \cos(2\pi f_m t)] \cos(2\pi f_c t)$$

### Spektral Analiz (Frekans Alanına Geçiş)
Trigonometrik dönüşüm formülü olan $\cos(A)\cos(B) = \frac{1}{2}[\cos(A-B) + \cos(A+B)]$ kullanılarak frekans bileşenleri ispatlanır:

$$s_{AM}(t) = A_c \cos(2\pi f_c t) + \frac{A_c \mu}{2} \cos(2\pi (f_c - f_m) t) + \frac{A_c \mu}{2} \cos(2\pi (f_c + f_m) t)$$

Bu denklem bize spektrumda üç bileşen olduğunu kanıtlar:
1.  **Taşıyıcı (Carrier):** $f_c$ frekansında. (Bilgi taşımaz, güç harcar).
2.  **Alt Yan Bant (LSB):** $f_c - f_m$ frekansında.
3.  **Üst Yan Bant (USB):** $f_c + f_m$ frekansında.

### Güç Verimliliği Analizi (Power Efficiency)
Mühendislikte her şey verimdir. AM neden verimsizdir?
Toplam güç, taşıyıcı ve yan bantların güçleri toplamıdır ($P \propto V^2/2$):

$$P_{Total} = P_{Carrier} + P_{USB} + P_{LSB} = \frac{A_c^2}{2} + \frac{(A_c \mu/2)^2}{2} + \frac{(A_c \mu/2)^2}{2}$$
$$P_{Total} = \frac{A_c^2}{2} \left( 1 + \frac{\mu^2}{2} \right)$$

Verimlilik ($\eta$), faydalı gücün (Yan bantlar) toplam güce oranıdır:
$$\eta = \frac{P_{Sidebands}}{P_{Total}} = \frac{\mu^2 / 2}{1 + \mu^2 / 2} = \frac{\mu^2}{2 + \mu^2}$$

* **En iyi senaryoda ($\mu = 1$):** $\eta = 1/3 \approx \%33.3$.
* **Sonuç:** AM vericisinin harcadığı gücün **%66'sı çöp** (taşıyıcıya gider). Sadece %33'ü bilgi taşır. Bu yüzden DSB-SC ve SSB geliştirilmiştir.

---

## 2. Çift Yan Bant - Taşıyıcısı Bastırılmış (DSB-SC)

Güç israfını önlemek için taşıyıcı ($f_c$) gönderilmez. Sadece $m(t) \times c(t)$ işlemi yapılır.

$$s_{DSB-SC}(t) = m(t) \cos(2\pi f_c t)$$

* **Avantaj:** Güç verimliliği %100'dür. Tüm enerji bilgiye harcanır.
* **Dezavantaj:** Alıcıda (Receiver) "Coherent Detection" gerekir. Yani yerel osilatörün frekansı ve **fazı**, vericiyle birebir aynı olmalıdır. Faz kayarsa ($\phi$), sinyal sönümlenir: $V_{out} \propto \cos(\phi)$.

---

## 3. Tek Yan Bant (SSB - Single Sideband)

DSB-SC'de aynı bilgi hem LSB hem USB'de vardır. Bant genişliğini yarıya düşürmek için biri filtrelenir.
Matematiksel olarak bu, **Hilbert Dönüşümü** ($\hat{m}(t)$) ile ifade edilir:

$$s_{SSB}(t) = \frac{A_c}{2} [m(t) \cos(2\pi f_c t) \mp \hat{m}(t) \sin(2\pi f_c t)]$$

* **(-):** Üst Yan Bant (USB)
* **(+):** Alt Yan Bant (LSB)

> **GNU Radio Uygulaması:** `Complex to Real` bloğu veya `Weaver Modulator` yapısı ile SSB üretilir.

---

## 4. Frekans Modülasyonu (Frequency Modulation - FM)

Mesaj sinyalinin genliği, taşıyıcının **frekansını** değiştirir. Gürültü bağışıklığı (Noise Immunity) AM'den çok daha yüksektir çünkü atmosferik gürültü genliği bozar, frekansı kolay kolay bozamaz.

### Matematiksel Model
Anlık frekans $f_i(t) = f_c + k_f m(t)$ şeklindedir. Faz, frekansın integrali olduğundan:

$$s_{FM}(t) = A_c \cos\left( 2\pi f_c t + 2\pi k_f \int_{-\infty}^{t} m(\tau) d\tau \right)$$

### Bant Genişliği (Carson Kuralı)
FM teorik olarak sonsuz bant genişliğine sahiptir (Bessel fonksiyonları gereği). Ancak enerjinin %98'inin bulunduğu bant genişliği **Carson Kuralı** ile hesaplanır:

$$BW \approx 2 (\Delta f + f_m)$$

* $\Delta f$: Frekans sapması (Deviation).
* $f_m$: Mesaj sinyalinin maksimum frekansı.

---

## GNU Radio Blokları ve İpuçları

1.  **Signal Source:** Hem mesaj ($f_m$) hem taşıyıcı ($f_c$) üretmek için temel blok.
2.  **Multiply:** İki sinyali çarpmak (Mikserlemek). DSB-SC için temel işlemdir.
3.  **Add:** Taşıyıcı ekleyerek AM elde etmek için kullanılır.
4.  **Low Pass Filter (Demodülasyon için):** Zarf dedektörü (Envelope Detector) çıkışındaki yüksek frekanslı bileşenleri temizleyip orijinal $m(t)$'yi almak için zorunludur.

> **Hata Ayıklama:** Eğer AM demodülasyonda ses bozuk geliyorsa, `Sample Rate` uyumsuzluğu veya filtre kesim frekansı (Cutoff) yanlıştır. Nyquist'i asla unutma.
