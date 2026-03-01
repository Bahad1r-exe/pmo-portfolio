# Şube Performans Dashboardu & Veri Analitiği Projesi

## 1. Projenin Amacı
Banka yönetiminin her bir şubenin satış, operasyonel verimlilik ve müşteri memnuniyet oranlarını tek bir ekrandan, canlı (veya T-1 günlük) takip edebilmesi için bir **Şube Performans Dashboardu (Power BI)** oluşturulmasıdır.

## 2. Tanımlanan Temel Performans Göstergeleri (KPI)
İş birimi (Sales & Branch Management) ile yapılan toplantılar sonucunda aşağıdaki **KPI'lar** belirlenmiştir:
- **Toplam Kredi Kullandırım Hacmi (Aylık/Yıllık):** (Gerçekleşen vs. Hedef)
- **Yeni Müşteri Edinimi (NCA - Net Customer Acquisition):** Mobil, Şube ve Diğer Kanallar ayrımında.
- **Şube Gişe Bekleme Süresi Ortalama (SLA):** 5 dakikayı aşan işlemlerin yüzdesi.
- **NPS (Net Promoter Score) & Müşteri Şikayet Oranı:** Şube bazlı müşteri geri bildirim anket sonuçları.
- **Personel Verimliliği:** (Toplam İşlem Hacmi / Aktif Çalışan Sayısı)

## 3. Veri Kaynağı ve SQL Entegrasyon Mantığı (Data Extraction)
Veriler, bankanın "Core Banking" sistemindeki `Transactions`, `Customers` ve `BranchMetrics` tablolarından periyodik olarak Data Warehouse'a aktarılmaktadır. Dashboard için örnek T-SQL mantığı:

```sql
-- Aylık Şube Bazlı Kredi Hacmi (Örnek Sorgu)
SELECT 
    b.BranchName,
    b.Region,
    COUNT(c.CreditID) AS TotalCreditCount,
    SUM(c.Amount) AS TotalCreditVolume_TRY,
    m.TargetVolume_TRY,
    (SUM(c.Amount) / m.TargetVolume_TRY) * 100 AS AchievementPercentage
FROM Credits c
JOIN Branches b ON c.BranchID = b.BranchID
JOIN MonthlyTargets m ON b.BranchID = m.BranchID 
                      AND MONTH(c.IssueDate) = m.TargetMonth
WHERE YEAR(c.IssueDate) = 2026 AND MONTH(c.IssueDate) = 3
GROUP BY b.BranchName, b.Region, m.TargetVolume_TRY
ORDER BY AchievementPercentage DESC;
```

## 4. Power BI Dashboard Tasarım Özeti
Dashboard 3 ana sekmeden (sayfadan) oluşacak şekilde tasarlandı:
1. **Yönetici Özeti (Executive Summary):** 
   - Üst yönetim için Türkiye geneli ısı haritası (MapLibre/PowerBI Maps).
   - En İyi & En Kötü 5 Şube (Bar Chart).
   - Genel Gerçekleşme Oranı (Gauge/Metric vizualleri).
2. **Şube Detay Analizi (Branch Drill-down):** 
   - İlgili şube seçildiğinde, personel bazlı performans (Table/Matrix).
   - Zaman çizelgesine göre satış trendleri (Line Chart).
3. **Müşteri Deneyimi (Customer Experience):** 
   - NPS skoru (Donut Chart).
   - Çağrı Merkezi ve Şube entegrasyonlu destek çözme süreleri.

## 5. Proje Çıktıları & İyileştirmeler
- **Manuel Raporlama Yükünün Azaltılması:** Daha önce Excel üzerinden haftalık olarak manuel %60 eforla hazırlanan raporlar, Power BI ile tamamen otomatik hale getirildi.
- **Veriye Dayalı Karar:** Bekleme süresi SLA'sını geçen (5 dk üzeri) 3 kritik şube tespit edilip, rotasyon personel desteği uygulandı.
- **Performans Takibi:** Gişe personeli prim sistemine adil bir görünürlük katıldı.
