# A8 Mini Gimbal Projesi

Bu proje, A8 Mini Gimbal'ın CUAV 5+ otopilot kartı ile kontrolünü, kabloların pin eşlemelerini ve
Ethernet portu kullanılarak NVIDIA Xavier NX ile bağlantı kurularak Python kodu ile kontrol ve görüntü alımını açıklamaktadır.

## Amaç

Bu projenin amacı, A8 Mini Gimbal'ı profesyonel görüntüleme ve izleme uygulamaları için hassas bir şekilde kontrol etmektir.
CUAV 5+ otopilot kartı ve NVIDIA Xavier NX kullanarak, gimbal kontrolünü optimize etmek ve yüksek kaliteli görüntü verileri almak hedeflenmektedir.


## İçerik
- Kablolama pin eşlemeleri ve bağlantıların doğru şekilde yapılması
- CUAV 5+ otopilot kartı kullanarak A8 Mini Gimbal'ın seri bağlantıyla kontrol edilmesi
- Ethernet portu  veya mini HDMI portu kullanarak PC veya NVIDIA Xavier NX ile A8 Mini Gimbaldan görüntü alımının gerçekleştirilmesi
- Python kodu ile gimbal kontrolü

### A8 Mini Gimbal Besleme
![a8_mini_feed](https://github.com/Numan-Aktas/A8_mini_workout/blob/main/images/a8_mini_fee.png)

A8 Mini beslenirken 11-16V arasında gerilim ve 0,5-1 Amper Akım kaynağı gereklidir.
Herhangi bir güç kaynağı ile beslenebilir. Ayrıca daha verimli çalışması için orta gerilim değerleri seçilebilir. Güç verdikten sonra A8 mini ısınmaya başlayacaktır bu durum tamamen normal. Yaklaşık sıcaklığı 55 C olucaktır.
### Firmware & Kalibrasyon & Ayarlar
Firmware yüklemek ve diğer işlemleri yapmak için SIYI'nın kendi uygulaması olan SIYI PC Assistant yapılabilir.
SIYI DRİVE: https://drive.google.com/drive/folders/1aSulEJW6OYt8UTtW0osgL20lkXcMuGxp

![SIYIassistant_firmware](https://github.com/Numan-Aktas/A8_mini_workout/blob/main/images/SIYIassistant_firmware.png)
![SIYIassistant_ayarlar](https://github.com/Numan-Aktas/A8_mini_workout/blob/main/images/SIYIassistant_ayarlar.png)

### CUAV v5+ Otopilot Kartı ile Kontrol

CUAV v5+ otopilot kartıyla A8 Mini Gimbal'ı kontrol etmek için doğru bir kablolama gerekmektedir. 
Aşağıdaki resimde gösterildiği gibi pinlerin doğru şekilde bağlanmasıyla seri bağlantı sağlanır:

![Pixhawk_pin](https://github.com/Numan-Aktas/A8_mini_workout/blob/main/images/Pixhawk_pin.png)

Kontrol için ayrıca uçuş kontrolcüsü üzerinde ayarlanması gereken parametreler bulunmaktadır. Bu parametreleri,
GCS (Ground Control Station) üzerinden yapabilirsiniz. Gerekli parametreler şunlardır:
##### Not: Bu gimballer için destek, ArduPilot 4.3.1 ve (ve üstü) versiyonlar için mevcuttur.
SERIALX_PROTOCOL = 8   SToRM32 Gimbal Serial\
SERIALX_BAUD = 115     for 115200 bps\
MNT1_TYPE = 8          Siyi \
CAM_TRIGG_TYPE = 3     Mount Siyi \
CAM1_TYPE = 4          Mount Siyi \
Parametreleri kaydedip  otopilotu yeniden başlatın.

RCX_OPTION = 213 Pitch Açı Kontrol \
RCX_OPTION = 214 Yaw Açı Kontrol 

RCX_OPTION = 166 Kamera Video Kayıt\
RCX_OPTION = 167 Kamera Zoom\
RCX_OPTION = 168 Kamera Manual Odaklanma\
RCX_OPTION = 169 Kamera Otamatik Odaklanma

Parametrelerin doğru bir şekilde ayarladığınızdan emin olun, çünkü bu, A8 Mini Gimbal'ın doğru ve istenen şekilde kontrol edilmesini sağlayacaktır.
### Görüntü Alma
#### Ethernet üzerinden görüntü alma
Ethernet üzerinden cihazın şuanki(06.2023) sürümünde maksimum 720p 30 fps alınabilir. Bu değer ilerleyen zamanlarda değişebilir.
Resimdeki kabloyu kullanarak kamera ve cihaz arasında Ethernet bağlantısı kurulabilir.

![Ethernet_cable](https://github.com/Numan-Aktas/A8_mini_workout/blob/main/images/Ethernet_cable.png)

Bağlanmak için statik IP ayarlanmalıdır. ayarlanılan IP'nin kamera IP'sini kapsaması ve aynı olmaması gerekmektedir.

![IPV4_ayar](https://github.com/Numan-Aktas/A8_mini_workout/blob/main/images/IPV4_ayar%C4%B1.png)

Statik IP ayarlandıktan sonra opencv gibi kütüphaneler kullanılarak cihaz üzerinden görüntü alabilirsiniz. 
```
import cv2
kamera_url = 'rtsp://192.168.144.25:8554/main.264'

# Video akışını almak için VideoCapture
video_port = 0
video = cv2.VideoCapture(video_port)

while True:
    ret, frame = video.cap()

    # Görüntüyü işleyin veya gösterin
    cv2.imshow('Kamera Görüntüsü',frame)
    
    # 'q' tuşuna basarak döngüden çıkın
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
        
# Bağlantıyı ve pencereleri serbest bırakın
video.release()
cv2.destroyAllWindows()

```
Ayrıca daha kısa bir kablo kullanmak ve doğrudan ethernet girişine dönüştürmek için aşağıdaki kablolama yapılabilir.

![ethernet_to_pin](https://github.com/Numan-Aktas/A8_mini_workout/blob/main/images/ethernet_to_pin.png)

#### Mini HDMI üzerinden görüntü alma 
Ethernet üzerinden cihazın şuanki(06.2023) sürümünde maksimum 720p 30 fps alınabilir. Bu değer ilerleyen zamanlarda değişebilir.
HDMI üzerinden doğrudan veya HDMI VIDEOCAPTURE cihazlarıyla usb veya hdmi üzerinden görüntü alınabilir.

### SDK Üzerinden Python Kodu ile Kontrol 

Repodaki A8_mini_sdk dosyası içindeki siyi_sdk.py dosyasının içindeki fonksiyonları kullanarak kontroller ve çeşitli parametrelerin alınması ve yazılması gerçekleştirilebilir.
##### Not: kamera kontrolleri baş aşağı olucak şekilde çalışmaktadır baş yukarı kullanmak için ekstra değerler eklenmelidir.
