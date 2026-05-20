# 🛒 MiniMarketERP - Katmanlı Mimari Otomasyon Sistemi

MiniMarketERP, bir marketin ürün stoklarını, müşteri hesaplarını, veresiye borç takibini, tedarikçi ilişkilerini, satış ve ödeme süreçlerini merkezi ve güvenli bir şekilde yönetmek için geliştirilmiş **Çok Katmanlı Mimari (N-Tier Architecture)** tabanlı bir kurumsal kaynak planlama (ERP) otomasyonudur.

Bu proje, akademik standartlara ve kurumsal yazılım geliştirme prensiplerine uygun olarak **ADO.NET** ve **İlişkisel Veritabanı (RDBMS)** modeli kullanılarak geliştirilmiştir.

---

##  Proje Mimarisi (N-Tier Architecture)

Proje, kodun sürdürülebilirliği, okunabilirliği ve tekrar kullanılabilirliği (reusability) göz önünde bulundurularak **4 ana katman** halinde tasarlanmıştır:

### 1.  MiniMarketERP.Entities (Varlık Katmanı)
Veritabanındaki tabloların kod tarafındaki nesnel karşılıklarını (sınıflarını) barındırır. Sadece özellikleri (properties) içerir ve veri transfer objesi (DTO) işlevi görür.
* `Urun.cs`, `Musteri.cs`, `Satis.cs`, `SatisDetay.cs`, `Odeme.cs`, `Tedarikci.cs`

### 2. 🔹MiniMarketERP.DataAccess (Veri Erişim Katmanı - DAL)
Veritabanı ile doğrudan iletişim kuran katmandır. SQL sorguları, bağlantı yönetimi ve CRUD (Ekle, Sil, Güncelle, Listele) fonksiyonları bu katmanda ADO.NET bileşenleri ile yürütülür.
* `DbHelper.cs`: Veritabanı bağlantı (`SqlConnection`) açma/kapama ve komut çalıştırma süreçlerini merkezi olarak yönetir.
* `UrunDAL.cs`, `MusteriDAL.cs` vb.: Tabloya özgü parametreli SQL sorgularını çalıştırır ve verileri `SqlDataReader` ile okuyarak listelere aktarır.

### 3.  MiniMarketERP.Business (İş Mantığı Katmanı - BLL)
Projenin beynidir. Kullanıcı arayüzünden (UI) gelen veriler veritabanına işlenmeden önce bu katmandaki iş kurallarından (Validation) ve doğrulama süreçlerinden geçer.
* `UrunManager.cs`, `MusteriManager.cs` vb.: İş mantığını denetleyen ve veri tutarlılığını sağlayan sınıflardır.

### 4.  MiniMarketERP.UI (Kullanıcı Arayüzü Katmanı)
Kullanıcının sistemle etkileşime girdiği Windows Forms tabanlı modern ve modüler arayüz katmanıdır.
* `MainForm.cs`: Ana yönetim paneli.
* `Views/`: Modüllere ait (Müşteri, Ürün, Satış vb.) alt arayüz formları.

---

##  Öne Çıkan Teknik Özellikler & Kurallar

* **Gelişmiş Validasyon (İş Kuralları):** Eksik veri girişleri (`string.IsNullOrWhiteSpace`), sıfırdan küçük fiyat veya stok adetleri gibi hatalı durumlar Business katmanında `throw new Exception` ile yakalanarak engellenir.
* **Güvenli SQL Sorguları:** SQL Injection (siber saldırı) açıklarını tamamen önlemek adına tüm sorgularda **Parametreli Sorgu Yapısı (`AddWithValue`)** kullanılmıştır.
* **LINQ & Lambda Expressions:** Bellek üzerindeki koleksiyonları hızlıca filtrelemek, arama yapmak veya kritik stok seviyesindeki (`list.FindAll(u => u.Adet <= u.Kritik_Stok)`) ürünleri listelemek için aktif olarak LINQ yapıları kullanılmıştır.
* **Bire-Çok İlişkili Satış Yapısı:** Yapılan her satış, `Satis` ve `SatisDetay` tablolarına ilişkisel olarak kaydedilir ve stok miktarları veritabanından dinamik olarak düşürülür.

---

##  Kullanılan Teknolojiler

* **Dili:** C# (.NET Framework)
* **Arayüz:** Windows Forms App
* **Veritabanı:** Microsoft SQL Server
* **Veri Erişim Teknolojisi:** ADO.NET (SqlClient, SQL Command, SQL Data Reader)
* **Sorgulama:** T-SQL & LINQ (Language Integrated Query)
