A8 Mini Gimbal Projesi

Bu proje, A8 Mini Gimbal'ın CUAV 5+ otopilot kartı ile kontrolünü, kabloların pin eşlemelerini ve
Ethernet portu kullanılarak NVIDIA Xavier NX ile bağlantı kurularak Python kodu ile kontrol ve görüntü alımını açıklamaktadır.




## Amaç

Bu projenin amacı, A8 Mini Gimbal'ı profesyonel görüntüleme ve izleme uygulamaları için hassas bir şekilde kontrol etmektir.
CUAV 5+ otopilot kartı ve NVIDIA Xavier NX kullanarak, gimbal kontrolünü optimize etmek ve yüksek kaliteli görüntü verileri almak hedeflenmektedir.


## Özellikler
- Kablolama pin eşlemeleri ve bağlantıların doğru şekilde yapılması.
- CUAV 5+ otopilot kartı kullanarak A8 Mini Gimbal'ın seri bağlantıyla kontrol edilmesi.
- Ethernet portu kullanarak NVIDIA Xavier NX ile A8 Mini Gimbal arasında bağlantı kurulması.
- Python kodu ile gimbal kontrolü ve görüntü alımının gerçekleştirilmesi.


### CUAV v5+ Otopilot Kartı ile Kontrol

CUAV v5+ otopilot kartıyla A8 Mini Gimbal'ı kontrol etmek için doğru bir kablolama gerekmektedir. 
Aşağıdaki resimde gösterildiği gibi pinlerin doğru şekilde bağlanmasıyla seri bağlantı sağlanır:
![

Kontrol için ayrıca uçuş kontrolcüsü üzerinde ayarlanması gereken parametreler bulunmaktadır. Bu parametreleri,
GCS (Ground Control Station) üzerinden yapabilirsiniz. Gerekli parametreler şunlardır:

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

Warningğru bir şekilde ayarladığınızdan emin olun, çünkü bu, A8 Mini Gimbal'ın doğru ve istenen şekilde kontrol edilmesini sağlayacaktır.
