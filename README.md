# Enterprise Multi-LAN Network Topology (Cisco Packet Tracer)

Bu proje, Cisco Packet Tracer üzerinde tasarlanmış; 3 farklı yerel ağı (LAN), katlar arası yönlendirme (routing) mekanizmasını, VLSM subnetting yapısını ve temel ağ servislerini (DHCP Relay, DNS, Web HTTP) içeren kurumsal bir ağ simülasyonudur.

---

## 📌 Topoloji Mimarisi

Topoloji, bir ana yönlendirici (Router 2911) üzerinden haberleşen ve `192.168.10.0/24` bloğunun VLSM ile alt ağlara bölündüğü 3 bağımsız kattan oluşmaktadır:

* **Kat 1 (Mor Alan):** `192.168.10.0/25`
* **Kat 2 (Kırmızı Alan):** `192.168.10.128/26`
* **Kat 3 (Yeşil Alan):** `192.168.10.192/27`

---



### 🌐 Alt Ağ ve DHCP Yapılandırması

| Ağ Adı | Network ID | Subnet Mask | Kullanılabilir IP Aralığı | Broadcast IP | Gateway | DHCP Başlangıç | Dağıtılan Cihaz |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| **Kat 1 (serverPool)** | `192.168.10.0` | `255.255.255.128` (/25) | `192.168.10.1 - .126` | `192.168.10.127` | `192.168.10.1` | `192.168.10.5` | 2 PC, Switch, DHCP (`.126`) |
| **Kat 2 (Kat2Pool)** | `192.168.10.128` | `255.255.255.192` (/26) | `192.168.10.129 - .190` | `192.168.10.191` | `192.168.10.129` | `192.168.10.130` | 2 PC, Switch, DNS (`.190`) |
| **Kat 3 (Kat3Pool)** | `192.168.10.192` | `255.255.255.224` (/27) | `192.168.10.193 - .222` | `192.168.10.223` | `192.168.10.193` | `192.168.10.195` | 2 PC, Switch, Web (`.210`) |

---

## ⚙️ Uygulanan Konfigürasyonlar

* **Inter-Network Routing:** Kat 1, Kat 2 ve Kat 3 arasındaki paket iletimi Cisco 2911 Router bacakları (`Gig0/0`, `Gig0/1`, `Gig0/2`) üzerinden yapılandırıldı.
* **DHCP Relay Agent (`ip helper-address`):** Farklı broadcast domain'lerde yer alan Kat 2 ve Kat 3 istemcilerinin IP alabilmesi için router arayüzlerine `ip helper-address 192.168.10.126` komutu tanımlandı.
* **VLSM Alt Ağ Tasarımı:** IP israfını önlemek amacıyla 100, 50 ve 25 kullanıcı ihtiyaçlarına göre `/25`, `/26` ve `/27` subnet maskeleri uygulandı.
* **DNS Yapılandırması:** DNS sunucusu üzerinde alan adı kayıtları yapılandırılarak yerel ağ servisleriyle eşleştirildi.
* **HTTP Servisi:** Web sunucusunda web barındırma hizmeti devreye alındı.

---

## ✅ Test ve Doğrulama

* **Dinamik IP Alımı:** Mor, Kırmızı ve Yeşil ağlardaki PC'lerin DHCP üzerinden sorunsuz IP, Ağ Geçidi ve DNS adresi aldığı doğrulandı.
* **ICMP Ping Testleri:** Farklı katlardaki uç birimler (PC'ler) ve sunucular arasında katlar arası paket iletimi test edildi.

---

## 🚀 Dosyayı Çalıştırma

1. Bilgisayarınızda **Cisco Packet Tracer** kurulu olduğundan emin olun.
2. Depodaki `.pkt` uzantılı dosyayı indirip Packet Tracer ile açın.
3. Herhangi bir PC'nin **Desktop > IP Configuration** sekmesinden IP ayarını **DHCP**'ye alarak dinamik IP dağıtımını gözlemleyebilirsiniz.
