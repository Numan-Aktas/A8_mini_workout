# A8 Mini Gimbal Projesi

Bu proje, A8 Mini Gimbal'ın CUAV 5+ otopilot kartı ile kontrolünü, kabloların pin eşlemelerini ve Ethernet portu kullanılarak NVIDIA Xavier NX ile bağlantı kurarak Python kodu ile kontrol ve görüntü alımını açıklamaktadır.

## Amaç

Bu projenin amacı, A8 Mini Gimbal'ı profesyonel görüntüleme ve izleme uygulamaları için hassas bir şekilde kontrol etmektir. CUAV 5+ otopilot kartı ve NVIDIA Xavier NX kullanarak, gimbal kontrolünü optimize etmek ve yüksek kaliteli görüntü verileri almak hedeflenmektedir.

## İçerik
- Kablolama pin eşlemeleri ve bağlantıların doğru şekilde yapılması
- CUAV 5+ otopilot kartı ile A8 Mini Gimbal'ın seri bağlantıyla kontrol edilmesi
- Ethernet portu veya mini HDMI portu kullanarak PC veya NVIDIA Xavier NX ile A8 Mini Gimbaldan görüntü alımının gerçekleştirilmesi
- Python kodu ile gimbal kontrolü

---

### A8 Mini Gimbal Besleme
![a8_mini_feed](https://github.com/Numan-Aktas/A8_mini_workout/blob/main/images/a8_mini_fee.png)

A8 Mini beslenirken 11-16V arasında gerilim ve 0.5-1 Amper akım kaynağı gereklidir. Herhangi bir güç kaynağı ile beslenebilir. Daha verimli çalışması için orta gerilim değerleri seçilebilir. Güç verildikten sonra A8 Mini ısınmaya başlayacaktır; bu durum tamamen normaldir ve sıcaklık yaklaşık 55°C olacaktır.

---

### Firmware & Kalibrasyon & Ayarlar
Firmware yüklemek ve diğer işlemleri yapmak için SIYI'nın kendi uygulaması olan SIYI PC Assistant kullanılabilir.
SIYI DRIVE: [SIYI Firmware İndir](https://drive.google.com/drive/folders/1aSulEJW6OYt8UTtW0osgL20lkXcMuGxp)

![SIYIassistant_firmware](https://github.com/Numan-Aktas/A8_mini_workout/blob/main/images/SIYIassistant_firmware.png)
![SIYIassistant_ayarlar](https://github.com/Numan-Aktas/A8_mini_workout/blob/main/images/SIYIassistant_ayarlar.png)

---

### CUAV V5+ Otopilot Kartı ile Kontrol

CUAV V5+ otopilot kartıyla A8 Mini Gimbal'ı kontrol etmek için doğru kablolama gerekmektedir. Aşağıdaki resimde gösterildiği gibi pinlerin doğru şekilde bağlanmasıyla seri bağlantı sağlanır:

![Pixhawk_pin](https://github.com/Numan-Aktas/A8_mini_workout/blob/main/images/Pixhawk_pin.png)

#### Gerekli Parametreler:
- **SERIALX_PROTOCOL = 8**: SToRM32 Gimbal Serial
- **SERIALX_BAUD = 115**: 115200 bps
- **MNT1_TYPE = 8**: Siyi 
- **CAM_TRIGG_TYPE = 3**: Mount Siyi 
- **CAM1_TYPE = 4**: Mount Siyi 

Parametreleri kaydedip otopilotu yeniden başlatın.

#### RC Kontrolleri:
- **RCX_OPTION = 213**: Pitch Açı Kontrol 
- **RCX_OPTION = 214**: Yaw Açı Kontrol 
- **RCX_OPTION = 166**: Kamera Video Kayıt
- **RCX_OPTION = 167**: Kamera Zoom
- **RCX_OPTION = 168**: Kamera Manuel Odaklanma
- **RCX_OPTION = 169**: Kamera Otomatik Odaklanma

Bu parametreler doğru şekilde ayarlandığında A8 Mini Gimbal istenen şekilde kontrol edilebilir.

---

### Görüntü Alma

#### Ethernet Üzerinden Görüntü Alma
Ethernet üzerinden cihazın şu anki (06.2023) sürümünde maksimum 720p 30 fps alınabilir. İlerleyen yazılım güncellemeleri ile bu değer değişebilir. Aşağıdaki kabloyu kullanarak Ethernet bağlantısı kurulabilir:

![Ethernet_cable](https://github.com/Numan-Aktas/A8_mini_workout/blob/main/images/Ethernet_cable.png)

Bağlantı için statik IP ayarlanmalıdır. IP adresinin kamera IP'si ile çakışmadığından emin olun:

![IPV4_ayar](https://github.com/Numan-Aktas/A8_mini_workout/blob/main/images/IPV4_ayar%C4%B1.png)

Statik IP ayarlandıktan sonra OpenCV gibi kütüphaneler kullanarak cihazdan görüntü alabilirsiniz:

```python
import cv2
kamera_url = 'rtsp://192.168.144.25:8554/main.264'

# Video akışını almak için VideoCapture
video = cv2.VideoCapture(kamera_url)

while True:
    ret, frame = video.read()

    # Görüntüyü işleyin veya gösterin
    cv2.imshow('Kamera Görüntüsü', frame)
    
    # 'q' tuşuna basarak döngüden çıkın
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
        
# Bağlantıyı ve pencereleri serbest bırakın
video.release()
cv2.destroyAllWindows()
```

### Önemli Noktalar:
- Tüm görsellerin doğru yüklendiğinden emin ol.
- Statik IP adresi ve diğer yapılandırmaların projede kullanılan donanıma göre doğru olduğundan emin ol.
- Projeyi başkalarına sunmadan önce kod örneklerini test et.

Bu yapı, projenin profesyonel ve net bir şekilde anlaşılmasını sağlayacaktır.
