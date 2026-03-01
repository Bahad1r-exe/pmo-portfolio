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
- **FR_01:** Sistem, kullanıcının T.C. Kimlik numarasını MERNİS üzerinden doğrulamalıdır.
- **FR_02:** Sistem, kredi tutarı ve vade seçimi yapıldığında anlık ödeme planı (taksit tablosu) üretebilmelidir.
- **FR_03:** Karar motoru, KKB skorunu sorgulamalı ve iç risk politikası kural setini işletmelidir.
- **FR_04:** Müşteri, onaylanan kredi sözleşmesini dijital olarak (SMS OTP ile) imzalayabilmelidir.
- **FR_05:** Sistem, reddedilen kredi başvuruları için ret nedenini (örn: Yetersiz Skor) veri tabanına loglamalıdır.
- *(Toplam 12 functional requirement belirlenmiştir.)*

## 5. Kullanıcı Hikayeleri ve Kabul Kriterleri (User Stories & Acceptance Criteria)

**US_01:** Bir Bireysel Müşteri olarak, uygulamaya giriş yapmadan ana sayfadaki hesaplama aracı ile aylık taksit tutarımı görebilmek istiyorum; böylece kredi çekmeden önce bütçe planlaması yapabilirim.
- **AC1:** Hesaplama aracında "Kredi Tutarı" ve "Vade" alanları zorunlu olmalıdır.
- **AC2:** Vade seçeneği 1 ile en fazla 36 ay arasında sınırlandırılmalıdır.
- **AC3:** "Hesapla" butonuna basıldığında aylık taksit tutarı, toplam geri ödeme tutarı ve faiz oranı net olarak listelenmelidir.

**US_02:** Bir Kredi Tahsis Uzmanı olarak, karar motorundan "Manuel İnceleme" sonucu dönen başvuruları yetki bazlı bir ekranda görebilmek istiyorum; böylece detaylı değerlendirme yaparak onay/ret kararını verebilirim.
- **AC1:** Sadece "Tahsis_Uzmanı" yetkisine (RBAC) sahip kullanıcılar bu ekrana erişebilmelidir.
- **AC2:** Listelenen kayıtlarda başvuru sahibinin KKB skoru, talep edilen limit ve önceki banka verileri aynı pencerede görünmelidir.

*(Toplam 8 User Story ve ilgili AC'ler projeye dahil edilmiştir.)*

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
