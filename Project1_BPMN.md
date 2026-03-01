```mermaid
graph TD
    A[Müşteri Başvurusu: T.C. Kimlik & Tutar Girişi] --> B{Sistem: MERNİS Doğrulaması}
    B -->|Başarılı| C[KKB & Risk Skorlaması API Çağrısı]
    B -->|Başarısız| D[Hata Mesajı: Bilgiler Uyumsuz]
    C --> E{Karar Motoru (Decision Engine)}
    
    E -->|Skor Yetersiz| F[Reddedildi]
    F --> G[Müşteriye Ret SMS/Push Gönderimi]
    
    E -->|Skor Sınırda (Manuel İnceleme)| H[Tahsis Birimi İş Listesi]
    H --> I{Uzman İncelemesi}
    I -->|Red| F
    I -->|Onay| J
    
    E -->|Skor Yeterli & Limit Uygun| J[Otomatik Onay]
    
    J --> K[E-Devlet Entegrasyonu ile Sözleşme Onayı]
    K --> L[Kredi Tutarının Vadesiz Hesaba Aktarımı]
    L --> M((Süreç Sonu))
    
    classDef startFill fill:#d4edda,stroke:#28a745,stroke-width:2px;
    classDef errorFill fill:#f8d7da,stroke:#dc3545,stroke-width:2px;
    classDef manualFill fill:#fff3cd,stroke:#ffc107,stroke-width:2px;
    
    class A startFill;
    class D,F errorFill;
    class H,I manualFill;
```
