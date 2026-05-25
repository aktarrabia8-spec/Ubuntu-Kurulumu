# 🐧 Linux Temelleri: Bir Mühendislik Not Defteri

Bu depo, bir Yazılım Mühendisliği öğrencisinin Linux dünyasına attığı adımların, kurumsal sistem mimarilerinin ve öğrendiği kritik bilgilerin derlemesidir. 

Karmaşık ve ezbere dayalı rehberler yerine, bir mühendisin perspektifinden *"bu işin mantığı ve mimarisi nedir?"* sorusuna yanıt arar.

---

## 🚀 İçerik
* **Dağıtım Mimarisi:** Doğru OS seçimi ve kurumsal ekosistem analizi.
* **Görsel Kurulum Protokolü:** Adım adım Ubuntu işletim sistemi kurulumu ve konfigürasyonu.
* **Dosya Sistemi (Yakında):** `/root`'tan `/home`'a hiyerarşik Linux dosya ağacı.
* **Terminal Gücü (Yakında):** Sektörde hayat kurtaran ve otomasyon sağlayan komutlar.
* **Süreç Yönetimi (Yakında):** İşletim sisteminin arka planındaki süreç (Process) ve Thread dünyası.

## 🛠️ Neden Bu Repo?
Teorik olarak gördüğümüz **"İşletim Sistemleri (Operating Systems)"** dersindeki soyut kavramları (I/O yönetimi, dosya sistemleri, yetkilendirme modelleri) pratikle birleştirmek, kalıcı hale getirmek ve sektöre hazır bir altyapı kurmak için bu çalışmayı başlattım.

---

## 🏗️ Dağıtım Mimarisi ve Seçim Analizi

### Dağıtım Seçimi: Neden Ubuntu 24.04 LTS?
Bir mühendis olarak sistem seçiminde popülist ("en havalı") olanı değil, üretim ortamında (production) en sürdürülebilir, kararlı ve optimize olanı seçtik.

* **Kernel ve Kararlılık:** Ubuntu 24.04, Linux Kernel 6.8 ile gelir. Bu; modern donanımlarla (NVMe diskler, yeni nesil çok çekirdekli işlemciler) tam uyum, gelişmiş güvenlik yamaları ve yüksek I/O (Girdi/Çıktı) performansı demektir.
* **Paket Yönetimi ve Bağımlılıklar:** `apt` (Advanced Package Tool) paket yöneticisinin olgunluğu ve geniş PPA (Personal Package Archive) desteği, mühendislik araçlarını (Docker, GCC, Python venv, Kubernetes CLI) kurarken **bağımlılık cehennemine (dependency hell)** düşmemizi engeller.
* **LTS (Long Term Support) Stratejisi:** 5 yıllık kurumsal destek süreci, geliştirme ortamımızın (development environment) işletim sistemi güncellemeleri veya kararsız kütüphaneler nedeniyle sık sık bozulmayacağının güvencesidir.

---

## 🛠️ Adım Adım Kurulum Protokolü

Sistemi hazırladığımız ISO imajı üzerinden, modern donanım standartlarına uygun olarak **UEFI (Unified Extensible Firmware Interface)** modunda başlatıyoruz.

### Adım 1: Dil ve Lokalizasyon Seçimi
Sistemin çekirdek dil ayarlarını yapılandırıyoruz. Geliştirme ortamlarında log takibi ve hata analizi kolaylığı açısından İngilizce dil seçimi sektörel bir standarttır.

![Adım 1: Dil Seçimi](images/Screenshot%20%282%29.png)

### Adım 2: Ağ Bağlantısı ve Donanım Doğrulaması
Sistemin internete bağlanma durumunu test ediyoruz. Kurulum ekranında normal kurulum veya temel araçların yükleneceği minimal kurulum arasında seçim yapıyoruz.

![Adım 2: Ağ Bağlantısı](images/Screenshot%20%286%29.png)

### Adım 3: Klavye Düzeni (Keyboard Layout)
Kurulum esnasında sistemin en güncel paketleri internetten indirmesini sağlıyoruz. Ayrıca ekran kartı ve Wi-Fi sürücülerinin otomatik yüklenmesi için gerekli onayları veriyoruz.

![Adım 3: Klavye Düzeni](images/Screenshot%20%2810%29.png)

### Adım 4: Uygulama Katmanı Seçimi
Diskin tamamen silinip yeniden mi kurulacağı, yoksa başka bir işletim sisteminin yanına mı kurulacağı kararını son kez gözden geçirip diske yazdırma işlemine geçiyoruz.

![Adım 4: Uygulama Seçimi](images/Screenshot%20%2813%29.png)

### Adım 5: Disk Bölümleme (Partitioning Mimarisi)
En kritik mühendislik kararlarından biri. Temelde iki senaryo üzerinden ilerlenir:
1. **Diski Sil ve Kur:** Temiz kurulumlar ve sanallaştırma (VirtualBox/VMware) ortamları için idealdir. Arka planda diski `ext4` dosya sisteminde biçimlendirir.
2. **Elle Bölümlendirme (Advanced):** Disk üzerinde `swap` (takas alanı), `/home` ve `/root` dizinlerini fiziksel olarak ayırmak veya **LVM (Logical Volume Management)** yapısı kurmak için tercih edilir.

![Adım 5: Disk Seçimi](images/Screenshot%20%2815%29.png)

### Adım 6: Zaman Dilimi ve Lokasyon Ayarları
Sistem saatinin ve güncellemelerin bulunduğunuz bölgeye göre doğru şekilde senkronize edilmesi için harita üzerinden yaşadığınız şehri veya zaman dilimini seçiyoruz.

![Adım 6: Zaman Dilimi](images/Screenshot%20%2816%29.png)

### Adım 7: Kullanıcı Tanımlama ve Yetkilendirme (IAM)
Bilgisayarınız için bir kullanıcı adı belirliyoruz. Sistemde tam yetkiyle (sudo) işlem yapabilmeniz ve güvenliği sağlamanız için güçlü bir giriş şifresi oluşturuyoruz.

* **Hostname:** Bilgisayarın ağ üzerindeki benzersiz kimliği (Örn: `dev-machine-01`).
* **Password Policy:** Brute-force saldırılarına karşı güçlü bir şifre politikası ilk savunma hattıdır.

![Adım 7: Hesap Oluşturma](images/Screenshot%20%2818%29.png)

### Adım 8: Özet ve Değişikliklerin Diske Yazılması (Commit)
Kurulum sihirbazının topladığı konfigürasyonların son kontrol noktası. "Kur" butonuna basıldığı an disk tablosu (Partition Table) yeniden yazılır ve Linux dosya sistemi (`sda1`, `sda2` vb.) inşa edilmeye başlanır.

![Adım 8: Kuruluma Hazır](images/Screenshot%20%2819%29.png)