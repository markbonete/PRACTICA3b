# PRACTICA3b
## Codi sencer
```cpp
#include <Arduino.h>
#include "BluetoothSerial.h"

#if !defined(CONFIG_BT_ENABLED) || !defined(CONFIG_BLUEDROID_ENABLED)
#error Bluetooth is not enabled! Please run `make menuconfig` to and enable it
#endif

BluetoothSerial SerialBT;

void setup() {
Serial.begin(115200);
SerialBT.begin("ESP32test2"); //Bluetooth device name
Serial.println("The device started, now you can pair it with bluetooth!");
}

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

## Explicació del codi per parts
### Llibreries Necessàries
```cpp
#include <Arduino.h>
#include "BluetoothSerial.h"
```
Aquestes línies inclouen les llibreries necessàries per utilitzar el Bluetooth amb l'ESP32. Arduino.h és la llibreria bàsica d'Arduino, mentre que BluetoothSerial.h proporciona la funcionalitat per gestionar la comunicació Bluetooth Serial.

### Verificar la Configuració de Bluetooth
```cpp
#if !defined(CONFIG_BT_ENABLED) || !defined(CONFIG_BLUEDROID_ENABLED)
#error Bluetooth is not enabled! Please run `make menuconfig` to and enable it
#endif
```
Aquest bloc de codi comprova si el Bluetooth està habilitat en la configuració del microcontrolador ESP32. Si no ho està, es genera un missatge d'error durant la compilació indicant que s'ha de configurar correctament el Bluetooth utilitzant menuconfig.

### Creació de l'Objecte BluetoothSerial
```cpp
BluetoothSerial SerialBT;
```
Es crea un objecte SerialBT de la classe BluetoothSerial, que s'utilitzarà per gestionar la comunicació Bluetooth.

### Setup
```cpp
void setup() {
  Serial.begin(115200);
  SerialBT.begin("ESP32test2"); //Bluetooth device name
  Serial.println("The device started, now you can pair it with bluetooth!");
}
```
1. Inicialització Serial: S'inicia la comunicació serial a 115200 bps per permetre la comunicació amb l'ordinador o un altre dispositiu a través del port serial.
2. Inicialització Bluetooth: Es configura el Bluetooth amb el nom de dispositiu "ESP32test2" mitjançant SerialBT.begin(). Aquest nom serà visible per altres dispositius Bluetooth que cerquin dispositius disponibles.
3. Missatge d'Inici: Es mostra un missatge a la consola serial indicant que el dispositiu ha començat i que ara es pot emparellar mitjançant Bluetooth.

### Loop
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
Aquest bucle es crida contínuament per gestionar la comunicació entre el port serial i el dispositiu Bluetooth:

1. Lectura del Port Serial: Si hi ha dades disponibles per llegir des del port serial (Serial.available() retorna true), es llegeix un byte del port serial (Serial.read()) i s'envia al dispositiu Bluetooth (SerialBT.write()).
2. Lectura del Bluetooth: Si hi ha dades disponibles per llegir des del dispositiu Bluetooth (SerialBT.available() retorna true), es llegeix un byte del Bluetooth (SerialBT.read()) i s'envia al port serial (Serial.write()).
3. Retard Curt: S'introdueix un retard de 20 mil·lisegons (delay(20)) per permetre que altres processos puguin executar-se i per evitar saturar el microcontrolador amb lectures i escriptures contínues.

Basicament les dades que escribim ja sigui pel monitor serial o per el dispositiu emparellat per Bluetooth, les sortides dels dos dispositius mostren el mateix, ja sigui llegint les dades d'un canal i fent la transmissió com rebent les dades i mostrant-les pel monitor del dispositiu.
