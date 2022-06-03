# Parte 2: Conexión Bluetooth

* ## Control de LED mediante bluetooth

Primeramente se añade la librería *BluetoothSerial.h*. Luego se comprueba que la conexión se haya realizado correctamente y se inicia una variable *SerialBT* del tipo BluetoothSerial.

```cpp
#include <Arduino.h>
#include "BluetoothSerial.h"

#if !defined(CONFIG_BT_ENABLED) || !defined(CONFIG_BLUEDROID_ENABLED)
#error Bluetooth is not enabled! Please run `make menuconfig` to and enable it
#endif
BluetoothSerial SerialBT;
```
**Setup:** Inicializa el dispositivo bluetooth y se le asigna el nombre *ESP32test*. A continuación, devulve un mensaje para comprobar la conexión.  

```cpp
void setup() {
  Serial.begin(9600);
  SerialBT.begin("ESP32test"); //Bluetooth device name
  Serial.println("The device started, now you can pair it with bluetooth!");
  
}
```

**Loop:** aquí está el funcionamiento esencial del programa, comprueba si estamos recibiendo bytes de información por el puerto serie o si hay bytes disponibles para leer.

```cpp
void loop() {
  if (Serial.available()) {
    SerialBT.write(Serial.read());
  }
  if (SerialBT.available()) {
    Serial.write(SerialBT.read());
  }
  delay(20);
}
```