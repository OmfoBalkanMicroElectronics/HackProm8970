# HackProm8970 🚀

HackProm8970, National Chip (Guoxin) **GX3201H / GX3211** işlemci ailesini kullanan **Neta HD 8950 ve HD 8970** Kablo TV set-top box (STB) cihazları için geliştirilmiş bir gömülü sistem modifikasyon, tersine mühendislik ve özel arayüz (UI) simülatör projesidir.

Bu projenin amacı, kapalı devre operatör cihazlarının donanım kısıtlamalarını aşmak, gömülü Linux tabanlı yazılımlar geliştirmek ve **LVGL** kütüphanesi kullanarak tamamen özelleştirilmiş, modern televizyon arayüzleri inşa etmektir.

---

## 🛠️ Proje Mimarisi

* **İşlemci Mimarisi:** C-SKY (CK610M Çekirdeği) 32-bit SoC
* **İşletim Sistemi Altyapısı:** Gömülü Linux (Buildroot framework)
* **Grafik Motoru (UI):** LVGL (Light and Versatile Graphics Library) v9.x
* **Geliştirme / Simülasyon Ortamı:** Windows Subsystem for Linux (WSL) / Ubuntu & VS Code

---

## 💻 Geliştirme Ortamı Kurulumu

Proje, fiziksel donanıma ihtiyaç duymadan önce bilgisayar üzerinde simüle edilecek şekilde tasarlanmıştır.

### 1. Gerekli Linux Paketlerinin Kurulması (WSL / Ubuntu)
Derleme araçları ve bağımlılıkların hatasız çalışması için WSL terminalinizde aşağıdaki komutları sırasıyla çalıştırın:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install build-essential cmake git libsdl2-dev libsdl2-image-dev pkg-config libncurses-dev bison flex libssl-dev -y
```

### 2. vcpkg ve SDL2 Bağımlılık Yönetimi
Grafik simülatörünün çalışması için vcpkg üzerinden Linux (`x64-linux`) üçlüsü ile SDL2 kütüphanesini derleyin:

```bash
# vcpkg indirme ve kurulum
git clone https://github.com
cd vcpkg
./bootstrap-vcpkg.sh

# SDL2 bağımlılığını Linux için derleme
./vcpkg install sdl2:x64-linux
```

### 3. VS Code Ayarları (`.vscode/settings.json`)
VS Code'un vcpkg kütüphanelerini CMake üzerinden otomatik tanıyabilmesi için projenizin `.vscode/settings.json` dosyasına şu bloğu ekleyin:

```json
{
    "cmake.configureArgs": [
        "-DCMAKE_TOOLCHAIN_FILE=\${workspaceFolder}/vcpkg/scripts/buildsystems/vcpkg.cmake"
    ]
}
```

---

## 🚀 Simülatörü Çalıştırma

1. VS Code'u WSL modunda açın: `code .`
2. `Ctrl + Shift + P` basarak `CMake: Configure` komutunu çalıştırın ve **GCC** derleyicisini seçin.
3. Alt barda bulunan **Build** butonuna basarak projeyi derleyin.
4. Derleme bittikten sonra **Play (Run/Debug)** butonuna basarak sanal Neta arayüz ekranını bilgisayarınızda başlatın.

---

## 🔌 Donanım Müdahale Kılavuzu (CH341A)

Yazılan veya modifiye edilen `.bin` yazılım imajlarını cihaza aktarırken **CH341A Programlayıcı** kullanılır.

### Kritik Donanım Kuralları:
* **Voltaj Ayarı:** Neta anakartındaki SPI Flash çipleri (Winbond, MXIC vb.) kesinlikle **3.3V** ile çalışır. CH341A jumper ayarının 5V konumunda olmadığından emin olun. Aksi takdirde işlemci hattı kalıcı zarar görebilir.
* **Yedekleme (Dump):** Cihaza müdahale etmeden önce mevcut orijinal bozuk dökümü `Read` yaparak bilgisayarınıza kaydedin. Cihaza özel şifreleme anahtarları (CA/Conax ID) bu çipte saklanır. Kendi yedeklerinizi kaybetmek sinyal eşleşme hatalarına yol açar.
* **Mandal (SOP8) Bağlantısı:** Kırmızı kablo, çip üzerindeki 1. Pine (`CS - Chip Select`) denk gelmelidir.

---

## 📝 Lisans ve Feragatname
Bu proje tamamen eğitim ve tersine mühendislik araştırmaları amacıyla geliştirilmiştir. Yapılan donanımsal müdahaleler (CH341A ile flaş yazma) cihazınızı garanti dışı bırakabilir veya kalıcı olarak kullanılmaz hale getirebilir (brick). Oluşabilecek donanım hasarlarından geliştiriciler sorumlu değildir.
