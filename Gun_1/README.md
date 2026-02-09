# 1. Gün: SDR ve GNU Radio Temelleri

---

### SDR Sisteminin Avantajları (Software Defined Radio)

**Avantajları:**
* **Esneklik:** Aynı donanımla FM Radyo, GSM, GPS veya Wi-Fi sinyalleri işlenebilir.
* **Maliyet:** Özelleşmiş pahalı devreler yerine genel amaçlı işlemciler kullanılır.
* **Güncellenebilirlik:** Yeni bir protokol için donanım değiştirmeye gerek yoktur, kod güncellemek yeterlidir.

---

### GNU Radio

GNU Radio sinyal işleme bloklarını birleştirerek akış şemaları oluşturmamızı sağlayan açık kaynaklı bir yazılımdır.

---

### CTRL + F ile komut arayabilirsin.

---

### Veri Tipleri ve Renk Kodları
GRC arayüzünde port renkleri veri tipini belirtir. Uyumsuz renkler birbirine bağlanamaz.

| Renk | Veri Tipi | Açıklama |
| :--- | :--- | :--- |
| **Mavi** | Complex (Karmaşık) | I ve Q bileşenleri (Genelde RF sinyali) |
| **Turuncu** | Float (Kayan Nokta) | Reel sayılar (Ses sinyali, Demodüle edilmiş veri) |
| **Sarı** | Short | 16-bit tam sayı |
| **Mor** | Byte | 8-bit veri (Dijital bitler) |

Aynı zamanda Help > Types de bunu gösterir.


---

### Örnekleme (Sampling) 
Bir sinyali kayıpsız dijitale çevirmek için örnekleme frekansı ($f_s$), sinyalin en yüksek frekans bileşeninin ($f_{max}$) en az iki katı olmalıdır.

$$f_s \ge 2 \

---

### Dijital Filtreler
LPF, HPF, BPF, Notch 

---


