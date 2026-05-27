# Sensor_CARD_V2 (STM32F407 Based Multi-Sensor & Avionics DAQ Board)

Sensor_CARD_V2, endüstriyel standartlarda sinyal bütünlüğü (Signal Integrity) ve elektromanyetik uyumluluk (EMC) kriterleri gözetilerek tasarlanmış, **STM32F407VGT6** tabanlı yüksek performanslı bir veri toplama (Data Acquisition - DAQ) ve sensör kontrol kartıdır. 

Özellikle vibrasyonlu, yüksek gürültülü ve ekstrem ortamlarda (aviyonik sistemler, roket elektroniği vb.) kararlı çalışması adına özel donanımsal koruma ve izolasyon katmanlarına sahiptir.

---

## 🛠️ Öne Çıkan Donanım Özellikleri ve Tasarım Kriterleri

### 1. Mikrodenetleyici (MCU) Katmanı
* **Core:** ARM Cortex-M4 (32-bit RISC), 168 MHz CPU saat hızı, DSP ve FPU desteği.
* **Memory:** 1 Mbyte Flash, 192 Kbytes SRAM.
* **Boot Configuration:** Donanımsal `BOOT0` kontrolü, 10 kΩ pull-down direnci ile kararlı lojik seviyede tutulmuş ve harici pin header/jumper mimarisiyle izole edilmiştir.

### 2. Sinyal Bütünlüğü ve Haberleşme Protokolleri
* **MicroSD Kart (4-Bit SDIO Modu):** High-Speed Modda (50 MHz) sinyal yükselme zamanını (Rise Time < 3ns) korumak ve parazitik kapasitansı tolere etmek adına veri ve komut hatlarında **10 kΩ Strong Pull-up** dirençleri tercih edilmiştir. Saat (CLK) hattı ise yansımaları (ringing) önlemek amacıyla doğrudan sürülmüş ve sonlandırma direnciyle desteklenmiştir.
* **Gelişmiş I2C Arayüzü:** SDA ve SCL hatları arasındaki sinyal karışmasını (Crosstalk) önlemek amacıyla layout aşamasında `3W kuralı` uygulanmış ve hatlar GND düzlemleriyle elektriksel olarak kalkanlanmıştır (Shielding).
* **UART Hat Koruması:** TX ve RX hatlarında, anlık yüksek akım sıçramalarını sınırlamak ve EMI gürültüsünü sönümlemek amacıyla işlemci pini çıkışına yakın konumlandırılmış **22 Ω - 33 Ω** seri hat sonlandırma (series termination) dirençleri kullanılmıştır.

### 3. Güç Mimarisi ve Analog İzolasyon
* **+3V3_ADC Analog Referansı:** Entegre analog-dijital çeviricilerin (ADC) ve hassas sensörlerin gürültüden etkilenmesini önlemek amacıyla, ana 3.3V dijital hattından izole edilmiş, ultra-low noise bir LDO regülatör üzerinden beslenen bağımsız bir **+3V3_ADC** güç düzlemi (power plane) oluşturulmuştur.
* **Dekuplaj Kapasitör Yerleşimi:** Akım döngü empedansını minimumda tutmak adına, yüksek frekans gürültülerini süzen 100 nF dekuplaj kapasitörleri ilgili entegre ve LDO pinlerine en yakın konumda (point-of-load) yerleştirilmiştir.
* **GND Kesintisiz Dönüş Yolu (Return Path):** Çok katmanlı (Multi-layer) PCB mimarisinde, sinyal yollarının hemen altında kesintisiz GND polygon dökümleri kullanılarak döngü alanları (loop area) küçültülmüş ve EMI emisyonu minimuma indirilmiştir.

### 4. Aktif Sürücü Katmanları
* **Transistörlü Buzzer Sürücü:** GPIO pinini aşırı akımdan korumak adına low-side NPN BJT (BC847) anahtarlama devresi tasarlanmıştır. Floating (havada kalma) durumlarını engellemek ve ilk açılış kararlılığı için beyz bacağı **4.7 kΩ pull-down** ile sabitlenmiştir.

---

## 📂 Depo (Repository) Yapısı

```text
├── Hardware/             # AEasyEDA Şematik ve PCB Çizimleri
│   ├── Schematics/       # Devre Şemaları (PDF / Source)
│   └── Layout/           # PCB Katman Tasarımları ve Gerber Çıktıları
├── Firmware/             # STM32CubeIDE / Keil MDK Proje Klasörü
│   └── Sensor_CARD_V2.ioc # STM32CubeMX Konfigürasyon Dosyası
└── README.md             # Proje Tanıtım Dokümanı
