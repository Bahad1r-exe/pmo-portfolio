# PMO Simülasyonu: Mobil Bankacılık Güncelleme (Versiyon 2.4 - Modernizasyon) Projesi

## 1. Projenin Amacı
Mevcut mobil bankacılık uygulamasının kullanıcı deneyimini iyileştirmek, performansı artırmak ve backend sunucu yükünü azaltmak amacıyla Version 2.4 devreye alınması projesidir. Proje süresi 3 aydır.

## 2. İş Kırılım Ağacı (WBS - Work Breakdown Structure)
1. **Faz 1: Planlama ve Analiz** (Hafta 1-3)
   - 1.1 İlk İhtiyaç Analizi
   - 1.2 UX/UI Araştırmaları & Kullanıcı Geri Bildirimlerinin İncelenmesi
   - 1.3 Mimari Değerlendirme & Teknoloji Seçimi
2. **Faz 2: Tasarım (Design)** (Hafta 4-5)
   - 2.1 Yeni Ekran/UI Prototiplemesi (Figma)
   - 2.2 Mimari Altyapı Revizyonu
3. **Faz 3: Geliştirme (Development / Sprints)** (Hafta 6-9)
   - 3.1 Backend Optimizasyon API Entegrasyonları
   - 3.2 Frontend Refactoring & Ekran Tasarımlarının Geçişi
4. **Faz 4: Test ve Kalite Güvencesi (QA)** (Hafta 10-11)
   - 4.1 Fonksiyonellik Testleri
   - 4.2 Güvenlik & Penetrasyon Testleri (SecOps)
   - 4.3 Kullanıcı Kabul Testleri (UAT)
5. **Faz 5: Yaygınlaştırma (Deployment) & Go-Live** (Hafta 12)
   - 5.1 App Store & Play Store Gönderimi
   - 5.2 Satış/Çağrı Merkezi Eğitimleri
   - 5.3 Rollback Planının Hazırlanması

## 3. Risk Matrisi (Risk Management Matrix)
| Risk ID | Risk Tanımı | Olasılık (1-5) | Etki (1-5) | Risk Skoru | Azaltma (Mitigation) Planı |
|---------|-------------|----------------|------------|------------|---------------------------|
| R01 | API Entegrasyonu sırasında yaşanan uyumsuzluk ve kesintiler | 3 | 5 | 15 (Yüksek) | Tüm backend değişiklikleri için Sandbox ortamında ayrı bir yük testi organize edilmesi. |
| R02 | Apple/Google Store onay süreçlerinin 3 günden uzun sürmesi | 4 | 4 | 16 (Yüksek) | İnceleme süreci için proforma sürümün Go-Live tarihinden 10 gün önce başlatılması. |
| R03 | UX/UI Tasarım onaylarında gecikmeler yaşanması (Stakeholder onayı gelmemesi) | 2 | 3 | 6 (Düşük) | Tasarımcıyla Product Owner arasında haftalık Design Sync toplantısı oturtmak. |

## 4. Paydaş (Stakeholder) Analizi
- **Sponsor:** Dijital Bankacılık Başkan Yrd. (Yüksek Etki / Yüksek İlgi - Karar Alıcı)
- **Product Owner (PO):** Mobil Şube İş Birimi (Yüksek Etki / Yüksek İlgi - Yönetilecek Esas Kişi)
- **Geliştirme Ekipleri (IT):** Backend ve Mobile Native Dev. Takımları (Yüksek Etki / Beklentisi Yönetilen)
- **Pazarlama / Satış Ekipleri:** Müşteriye ürünü sunacak ekipler (Düşük Etki / Yüksek İlgi - Bilgilendirilecek)

## 5. Sprint Planlaması (Agile)
- Takım 2 haftalık (10 İş Günü) Sprint koşuları uygulamaktadır.
- Toplam 6 Sprint belirlenmiştir.
- **Sprint 2 Hedefi:** Ana Sayfa Dashboad ekranının React Native ile mock uplarının çalışır hale getirilmesi. Backend tarafında OTP girişinin optimize edilmesi.
- Her gün 15 dakikalık **Daily Standup** yapılmış ve Jira üzerinden ilerlemeler **Burn-down Chart** ile takip edilmiştir.
