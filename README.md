# Ubuntu-Kurulumu
Adım adım Ubuntu kurulum rehberi
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

![Adım 1: Dil Seçimi](Screenshot%20(2).jpg)

### Adım 2: Ağ Bağlantısı ve Donanım Doğrulaması
Sistemin internete bağlanarak kurulum esnasında en güncel güvenlik yamalarını ve gerekli üçüncü parti sürücüleri (Driver) çekmesini sağlıyoruz.

![Adım 2: Ağ Bağlantısı](Screenshot%20(6).jpg)

### Adım 3: Klavye Düzeni (Keyboard Layout)
Geliştirme süreçlerinde kod yazım hızını doğrudan etkileyen klavye girdi haritasını seçiyoruz. Tercihe göre Türkçe veya kodlama kolaylığı açısından English (US) seçilebilir.

![Adım 3: Klavye Düzeni](Screenshot%20(10).jpg)

### Adım 4: Uygulama Katmanı Seçimi
Temiz bir geliştirme ortamı için iki opsiyonumuz mevcut:
* **Öntanımlı Kurulum (Minimal):** Sadece temel araçlar ve tarayıcı barındırır.
* **Tam Kurulum (Full):** Ofis araçları ve ek yazılımlarla hazır bir masaüstü deneyimi sunar.

![Adım 4: Uygulama Seçimi](Screenshot%20(13).jpg)

### Adım 5: Disk Bölümleme (Partitioning Mimarisi)
En kritik mühendislik kararlarından biri. Temelde iki senaryo üzerinden ilerlenir:
1. **Diski Sil ve Kur:** Temiz kurulumlar ve sanallaştırma (VirtualBox/VMware) ortamları için idealdir. Arka planda diski `ext4` dosya sisteminde biçimlendirir.
2. **Elle Bölümlendirme (Advanced):** Disk üzerinde `swap` (takas alanı), `/home` ve `/root` dizinlerini fiziksel olarak ayırmak veya **LVM (Logical Volume Management)** yapısı kurmak için tercih edilir.

![Adım 5: Disk Seçimi](Screenshot%20(15).jpg)

### Adım 6: Zaman Dilimi ve Lokasyon Ayarları
Sistem saatini (Hardware Clock) ağ üzerindeki NTP (Network Time Protocol) sunucularıyla senkronize etmek ve log mekanizmalarının doğru zaman damgası (Timestamp) basmasını sağlamak için lokasyonumuzu seçiyoruz.

![Adım 6: Zaman Dilimi](Screenshot%20(18).jpg)

### Adım 7: Kullanıcı Tanımlama ve Yetkilendirme (IAM)
Sistem güvenliği prensipleri gereği (Principle of Least Privilege), en yetkili kullanıcı olan `root` hesabını doğrudan kullanıma açmıyoruz. Kendimize standart bir kullanıcı tanımlayıp, gerektiğinde `sudo` komutuyla geçici yetki alacak şekilde mimariyi kuruyoruz.

* **Hostname:** Bilgisayarın ağ üzerindeki benzersiz kimliği (Örn: `dev-machine-01`).
* **Password Policy:** Brute-force saldırılarına karşı güçlü bir şifre politikası ilk savunma hattıdır.

![Adım 7: Hesap Oluşturma](Screenshot%20(16).jpg)

### Adım 8: Özet ve Değişikliklerin Diske Yazılması (Commit)
Kurulum sihirbazının topladığı konfigürasyonların son kontrol noktası. "Kur" butonuna basıldığı an disk tablosu (Partition Table) yeniden yazılır ve Linux dosya sistemi (`sda1`, `sda2` vb.) inşa edilmeye başlanır.

![Adım 8: Kuruluma Hazır](Screenshot%20(19).jpg)

---
