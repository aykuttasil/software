+++
date = "2017-03-12T23:40:36+03:00"
desciption = "arduino,hc06,bluetooth,led,pwm"
thumbnailImage = ""
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
title = "Arduino HC-06 Bluetooth Kullanımı"
metaAlignment = "center"
categories = [
  "yazilim","arduino"
]
tags = [
  "software","arduino","hc06","hc-06","bluetooth"
]
keywords = [
  "yazilim",
  "sofware","arduino","hc06","hc-06","bluetooth"
]
#url = "arduino_hc06_kullanimi"
autoThumbnailImage = false
thumbnailImagePosition = "top"

+++


# HC-06 Bluetooth

- Aşağıdaki kodu **Arduino** kartınıza yükleyiniz
- [Bluetooth Terminal](https://play.google.com/store/apps/details?id=Qwerty.BluetoothTerminal&hl=tr) uygulamasını telefonunuza indirin
- HC-06 modulünün **TX-R**X çıkışlarını Arduino kartınızın **RX-T**X girişlerine entegre edin. (Ters sıralamaya dikkat)
- Cihazınızdan normal bluetooh bağlantısı kurar gibi **HC-06** modülü ile bağlantı kurun ve şifre olarak **1234** girin.
- Aşağıdaki kodda görebileceğiniz gibi 0,1,2,3,4 için farklı işlemler yapılmasını sağladık. Siz de ihtiyacınıza göre ayarlayın. Ben led parlaklığını düzenledim ya da motor hızı olarak da düşünebiliriz.

```arduino
const int LED_PIN = 9;

char veri;


void setup() {
  Serial.begin(9600);
  pinMode(LED_PIN, OUTPUT);
  Serial.println("HC-06 Kontrol Projesi");
}

void loop() {
  if (Serial.available() > 0) {

    veri = Serial.read();

    if (veri == '0') {
      digitalWrite(LED_PIN, LOW);
      Serial.println("LED Sonduruldu." );
    }

    if (veri == '1') {
      digitalWrite(LED_PIN, HIGH);
      Serial.println("LED Yakildi.");
    }
    
    if (veri == '2') {
      analogWrite(LED_PIN, 200);
      Serial.println("LED Yakildi.");
    }
    
    if (veri == '3') {
      //digitalWrite(LED_PIN, HIGH);
      analogWrite(LED_PIN, 255);
      Serial.println("LED Yakildi.");
    }

    if (veri == '4') {
      //digitalWrite(LED_PIN, HIGH);
      analogWrite(LED_PIN, 50);

      Serial.println("LED Yakildi.");
    }


  }
  delay(100);
}
```