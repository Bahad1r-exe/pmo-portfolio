# İş Gereksinimleri Dokümanı (BRD) - Dijital Kredi Başvuru Süreci Tasarımı

## 1. Proje Özeti
Bu proje, mevcut manuel süreçlerin yarattığı darboğazları ortadan kaldırmak amacıyla, müşterilerin mobil ve web platformları üzerinden uçtan uca dijital kredi başvurusunda bulunabilmelerini sağlayacak sistemin analizi ve tasarımıdır.

## 2. Mevcut Durum Analizi ve Darboğazlar
- **Uzun Onay Süreleri:** Fiziksel evrak teslimi onay sürecini ortalama 3 iş gününe uzatmaktadır.
- **Müşteri Kaybı (Churn):** Başvuru sırasındaki karmaşık formlar nedeniyle %45 oranında yarıda bırakma (drop-off) gözlemlenmiştir.
- **Sistem İçi İletişim Eksikliği:** Risk, tahsis ve müşteri hizmetleri birimleri arasında entegrasyon eksikliğinden kaynaklanan operasyonel yükler.

## 3. Yeni Süreç Tasarımı (BPMN Akış Özeti)
1. **Başvuru Girişi:** Müşteri, T.C. Kimlik No ve gelir bilgilerini girer.
2. **KKB & Risk Sorgulaması:** Sistem, API aracılığıyla Kredi Kayıt Bürosu (KKB) ve iç risk skorlamasını otomatik yapar.
3. **Karar Motoru (Decision Engine):**
   - **Otomatik Onay:** Risk skoru yüksek ve limit uygunsa sistem anında onay verir.
   - **Otomatik Red:** Skor yetersizliği durumunda ret SMS/Push bildirimi gönderilir.
   - **Manuel İnceleme:** Sınırda kalan profiller Tahsis birimi iş listesine düşer.
4. **Evrak & Sözleşme Onayı:** E-Devlet entegrasyonu ile dijital onay alınır.
5. **Kullandırım:** Tutar, müşterinin vadesiz hesabına anında aktarılır.

## 4. Fonksiyonel Gereksinimler (Functional Requirements)
- **FR_01 (Kimlik Doğrulama):** Sistem, kullanıcının T.C. Kimlik numarasını MERNİS üzerinden senkron olarak doğrulamalıdır.
- **FR_02 (Giriş Doğrulama):** Müşterinin başvuru esnasında OTP (One Time Password) ile telefon numarasını doğrulaması zorunludur.
- **FR_03 (Taksit Hesaplama):** Sistem, kredi tutarı ve vade seçimi yapıldığında anlık ödeme planı (taksit tablosu) üretebilmelidir.
- **FR_04 (Skor Sorgula):** Karar motoru, kredi başvurusu anında KKB skorunu API üzerinden sorgulamalıdır.
- **FR_05 (İç Risk Modeli):** Karar motoru, KKB skoruna ek olarak bankanın kendi iç risk politikası kural setini işletmelidir.
- **FR_06 (Otomatik Onay):** Skor >= 1300 ve Tutar <= 100.000 TL şartlarını sağlayan başvurular otomatik onay durumuna çekilmelidir.
- **FR_07 (Manuel Gözetme):** Skor 1000-1299 arasında olan veya tutar > 100.000 TL olan başvurular, "Tahsis İş Listesi" ekranına yönlendirilmelidir.
- **FR_08 (Otomatik Red):** Skor < 1000 olan başvurular sistem tarafından otomatik reddedilmelidir.
- **FR_09 (Sözleşme Onayı):** Müşteri, onaylanan kredi sözleşmesini dijital olarak (E-Devlet altyapısı veya OTP onayı ile) imzalayabilmelidir.
- **FR_10 (Loglama):** Sistem, reddedilen kredi başvuruları için ret nedenini (örn: Yetersiz Skor) veri tabanına loglamalıdır.
- **FR_11 (Bildirimler):** Kullanıcıya başvuru durum değişikliklerinde (Onay, Red, Tahsis Durumu) SMS ile bilgilendirme yapılmalıdır.
- **FR_12 (Hesaba Aktarım):** Sözleşme onaylandığı an, kredi tutarı sistem tarafından asenkron bir job aracılığıyla kullanıcının vadesiz Müşteri Hesabına saniyeler içinde yatırılmalıdır.

## 5. Kullanıcı Hikayeleri ve Kabul Kriterleri (User Stories & Acceptance Criteria)

**US_01:** Bir Bireysel Müşteri olarak, uygulamaya giriş yapmadan ana sayfadaki hesaplama aracı ile aylık taksit tutarımı görebilmek istiyorum; böylece kredi çekmeden önce bütçe planlaması yapabilirim.
- **AC1:** Hesaplama aracında "Kredi Tutarı" ve "Vade" alanları zorunlu olmalıdır.
- **AC2:** Vade seçeneği 1 ile en fazla 36 ay arasında sınırlandırılmalıdır.
- **AC3:** "Hesapla" butonuna basıldığında aylık taksit tutarı, toplam geri ödeme tutarı ve faiz oranı net olarak görüntülenmelidir.

**US_02:** Bir Bireysel Müşteri olarak, T.C. kimlik numaramı ve iletişim bilgilerimi kolayca girip OTP ile doğrulayabilmek istiyorum; böylece başvurumu güvenle başlatabilirim.
- **AC1:** Numara formatı (5XX) Regex ile doğrulanmalıdır.
- **AC2:** 180 saniye geçerli SMS OTP kodu sisteme gönderilmeli ve 3 yanlış denemede güvenlik blokesi konulmalıdır.

**US_03:** Bir Bireysel Müşteri olarak, başvuru tamamlandıktan sonra saniyeler içinde sonucumu ekranımda görebilmek istiyorum; böylece bankadan yanıt beklemek zorunda kalmam.
- **AC1:** API çağrı süresi boyunca kullanıcıya bir "loading" komponenti gösterilmelidir.
- **AC2:** Onay, Red veya Manuel Değerlendirme durumlarına göre farklı UI sonuç ekranları gösterilmelidir.

**US_04:** Bir Bireysel Müşteri olarak, onaylanan kredimin yasal belgelerini ekran üzerinden okuyup tek tıkla onaylayabilmek istiyorum; böylece şubeye gitmeden süreci tamamlayabilirim.
- **AC1:** Ön Bilgilendirme ve Kredi Sözleşmesi PDF olarak açılabilir olmalıdır.
- **AC2:** Sözleşme onayı işlemi için SMS doğrulama adımı gereklidir.

**US_05:** Bir Kredi Tahsis Uzmanı olarak, karar motorundan "Manuel İnceleme" sonucu dönen başvuruları kendime ait yetki bazlı ekranda liste halinde görebilmek istiyorum; böylece iş yükümü planlayabilirim.
- **AC1:** Sadece "Tahsis_Uzmanı" rolüne sahip kullanıcılar (RBAC) bu menüye erişebilmelidir.
- **AC2:** Liste ekranında Başvuru Tarihi, Tutar, KKB Skoru ve İsim soyisim kolonları filtrelenebilmelidir.

**US_06:** Bir Kredi Tahsis Uzmanı olarak, iş listemdeki bir başvurunun üzerine tıkladığımda müşterinin geçmiş kredi ödeme analizini ve KKB risk raporunu aynı ekranda görebilmek istiyorum; böylece hızlı karar alabilirim.
- **AC1:** KKB Raporu özeti ve Gecikmeli Gün Sayısı metriği kullanıcı kartında vurgulanmalıdır.
- **AC2:** Uzman, bu ekrandayken işlemi "Onayla" veya "Reddet" butonları yardımıyla sonuçlandırabilmelidir.

**US_07:** Bir Risk Yönetimi Uzmanı olarak, reddedilen kredi taleplerinin red sebeplerini (skor düşük, limit aşıldı vb.) istatistiksel olarak görebilmek istiyorum; böylece risk politikalarını revize edebilirim.
- **AC1:** Ret kodu eşleşmesine göre gruplanmış bir pasta grafiği ekranda olmalıdır.
- **AC2:** İlgili veriler CSV olarak dışarı aktarılabilmelidir.

**US_08:** Bir Sistem Yöneticisi olarak, karar motorundaki limit parametrelerini (Örn: maksimum otomatik onay tutarı = 100.000 TL) tek bir konfigürasyon ekranından güncelleyebilmek istiyorum; böylece kural değişimi için kod pushlanması gerekmesin.
- **AC1:** Limit konfigurasyonları veritabanında saklanmalıdır.
- **AC2:** Tutarı güncelleme eylemi bir "Audit Log" a kaydedilmelidir.

## 6. Basit Veri Modeli (SQL Tasarımı)
Sistem veritabanında `Customers`, `Loan_Applications`, ve `Credit_Scores` olmak üzere temel 3 tablo tasarlanmıştır.

```sql
CREATE TABLE Loan_Applications (
    ApplicationID INT PRIMARY KEY IDENTITY(1,1),
    CustomerID INT FOREIGN KEY REFERENCES Customers(CustomerID),
    RequestedAmount DECIMAL(18,2),
    TermMonths INT,
    ApplicationDate DATETIME,
    Status VARCHAR(50) -- 'Pending', 'Approved', 'Rejected', 'Manual_Review'
);
```
