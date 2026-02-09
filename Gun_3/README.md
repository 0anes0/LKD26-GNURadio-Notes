
## 1. CW (Continuous Wave - Sürekli Dalga)

En temel haberleşme biçimidir.

* **Tanım:** Herhangi bir modülasyon (bilgi) içermeyen saf sinüs dalgasıdır. Genliği ve frekansı sabittir.
* **Kullanım Alanı:** Genellikle Mors alfabesi (Telgraf) ile iletişimde kullanılır. Bilgi, taşıyıcının açılıp kapatılmasıyla (On-Off Keying) aktarılır.
* **Spektrum Görünümü:** FFT ekranında sadece taşıyıcı frekansında (örneğin 100 MHz) tek bir dik çubuk (pik) görülür. Bant genişliği teorik olarak sıfıra yakındır.

## 2. AM (Amplitude Modulation - Genlik Modülasyonu)

Bilgi sinyali, taşıyıcı sinyalin genliğini değiştirir.

* **Yapı:** Bir taşıyıcı ve iki yan banttan oluşur.
    * **Taşıyıcı (Carrier):** Bilgi taşımaz, sadece sinyali taşır. Gücün büyük kısmı buradadır.
    * **Yan Bantlar:** Bilgi bu bantlarda bulunur.
* **Bant Genişliği:** Bilgi sinyalinin frekansının iki katıdır (2 x fm).
* **Dezavantaj:** Güç verimliliği düşüktür çünkü enerjinin çoğu bilgi taşımayan taşıyıcı frekansına harcanır. Ayrıca atmosferik gürültüden (genlik bozulmalarından) çok etkilenir.

## 3. LSB ve USB (Tek Yan Bant - SSB)

AM modülasyonunda aynı bilgi hem sağda hem solda simetrik olarak bulunur. Bant genişliği ve güçten tasarruf etmek için taşıyıcı bastırılır ve yan bantlardan sadece biri gönderilir. Buna SSB (Single Side Band) denir.

### LSB (Lower Side Band - Alt Yan Bant)
* **Tanım:** Taşıyıcı frekansının solunda (altında) kalan frekans bandıdır.
* **Matematiksel Karşılık:** fc - fm
* **Kullanım:** Genellikle 10 MHz altındaki frekanslarda (7 MHz amatör bandı gibi) tercih edilir.

### USB (Upper Side Band - Üst Yan Bant)
* **Tanım:** Taşıyıcı frekansının sağında (üstünde) kalan frekans bandıdır.
* **Matematiksel Karşılık:** fc + fm
* **Kullanım:** Genellikle 10 MHz üzerindeki frekanslarda (14 MHz, 20 MHz gibi) tercih edilir.

**Not:** SSB dinlerken alıcı frekansının tam olarak tutturulması gerekir. Birkaç Hz'lik kayma bile sesin "Donald Duck" (ince) veya kalın çıkmasına neden olur.

## 4. FM (Frequency Modulation - Frekans Modülasyonu)

Bilgi sinyali, taşıyıcı sinyalin frekansını değiştirir. Genlik sabit kalır.

* **Çalışma Mantığı:** Ses sinyali yükseldiğinde frekans artar, düştüğünde frekans azalır.
* **Gürültü Bağışıklığı:** Atmosferik gürültü ve parazitler genellikle sinyalin genliğini bozar. FM alıcılarında bulunan "Limiter" devresi genliği tıraşladığı için FM, gürültüye karşı AM'den çok daha dirençlidir.
* **Bant Genişliği:** AM'e göre çok daha geniştir (Carson Kuralı ile hesaplanır). Bu yüzden ses kalitesi daha yüksektir.

---

### Laboratuvar Notu: GNU Radio Gözlemleri
Ders sırasında yapılan akış diyagramlarında şu farklar gözlemlendi:
1.  **AM:** FFT ekranında ortada sabit bir taşıyıcı ve yanında oynayan iki küçük çubuk görüldü.
2.  **SSB (LSB/USB):** Ortadaki taşıyıcı yok oldu ve sadece tek bir tarafta hareketlilik görüldü.
