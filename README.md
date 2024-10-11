
ReadMe - Fensterüberwachungsprojekt mit Wetterdaten und Telegram-Benachrichtigungen

### Übersicht

Dieses Projekt überwacht den Status eines Fensters (offen oder geschlossen) mithilfe eines Hall-Sensors. Wenn das Fenster offen ist und es regnet, wird eine Benachrichtigung über Telegram gesendet und eine LED blinkt als Warnsignal. Die Wetterdaten werden über die Open-Meteo API abgerufen.

---

### Voraussetzungen

1. Hardware:
   - ESP32 (oder kompatibler Mikrocontroller mit WLAN)
   - Hall-Sensor
   - LED (für visuelles Signal)
   - Jumper-Kabel zur Verkabelung der Komponenten
   - WLAN-Verbindung (mit Zugangsdaten)

2. Software:
   - [Arduino IDE](https://www.arduino.cc/en/software)
   - Bibliotheken:
     - `WiFi.h` (Standard für ESP32, zum Verbindungsaufbau mit WLAN)
     - `HTTPClient.h` (für HTTP-Requests)
     - `ArduinoJson.h` (für die Verarbeitung der Wetterdaten)

---

### Installation der Bibliotheken

1. WiFi Library:
   - Wird normalerweise mit dem ESP32-Boardpaket installiert.
   - Installiere das ESP32 Board-Paket:
     - Öffne die Arduino IDE.
     - Gehe zu `Datei -> Voreinstellungen`.
     - Füge die folgende URL zu "Zusätzliche Boardverwalter-URLs" hinzu:  
       `https://dl.espressif.com/dl/package_esp32_index.json`
     - Gehe zu `Werkzeuge -> Board -> Boardverwalter`.
     - Suche nach "ESP32" und installiere das Paket.

2. HTTPClient Library:
   - Diese Bibliothek wird normalerweise mit dem ESP32-Boardpaket installiert und ist direkt nutzbar.

3. ArduinoJson Library:
   - Öffne die Arduino IDE.
   - Gehe zu `Sketch -> Bibliothek einbinden -> Bibliotheken verwalten`.
   - Suche nach `ArduinoJson`.
   - Installiere die Bibliothek von Benoit Blanchon (Version 6.x oder höher).

---

### Hardware-Verkabelung

- Hall-Sensor:
  - Schliesse den Datenpin des Hall-Sensors an den GPIO-Pin 10 des ESP32 an.
  - Schliesse die Stromversorgung (VCC) und Masse (GND) korrekt an.

- LED:
  - Schliesse den positiven Pol der LED an einen Widerstand (ca. 220 Ohm) und dann an den GPIO-Pin 2 des ESP32.
  - Schliesse den negativen Pol der LED an GND.

---

### Konfiguration

1. WLAN-Zugangsdaten:
   - Ändere den WLAN-Namen (`ssid`) und das Passwort (`password`) im Code:

     const char* ssid = "WLANNAME";    // Dein WLAN-Netzwerkname
     const char* password = "PAssWORT";  // Dein WLAN-Passwort


2. Telegram-Bot:
   - Erstelle einen Telegram-Bot:
     - Gehe in Telegram zu @BotFather und erstelle einen neuen Bot.
     - Notiere dir den Bot-Token.
     - Füge diesen in den Code ein:

       String botToken = "BOT-TOKEN"; // Dein Telegram Bot Token

   - Erhalte deine `chat_id`:
     - Schreibe dem Bot eine Nachricht, um die `chat_id` zu erhalten.
     - Füge sie in den Code ein:

       String chat_id = "CHAT-ID";  // Deine Chat-ID


3. Open-Meteo API:
   - Die Wetterdaten werden von der Open-Meteo API bezogen. Die Anfrage verwendet eine feste URL basierend auf den Koordinaten (Breitengrad, Längengrad):

     const String weatherApiUrl = "https://api.open-meteo.com/v1/forecast?latitude=47.5227&longitude=7.6451&current_weather=true&timezone=Europe%2FBerlin";

   - Du kannst die URL anpassen, indem du die `latitude` (Breitengrad) und `longitude` (Längengrad) entsprechend deiner Region änderst.

---

### Funktionsweise

1. WiFi-Verbindung:
   - Das Programm verbindet sich automatisch mit dem angegebenen WLAN.

2. Fensterstatus prüfen:
   - Der Hall-Sensor überwacht den Status des Fensters.
   - Wenn das Fenster offen ist, werden Wetterdaten von der Open-Meteo API abgerufen.

3. Wetterdaten:
   - Die JSON-Daten von der API enthalten die aktuelle Temperatur und den Wettercode.
   - Wettercode 61–67 oder 80–82 bedeutet, dass es regnet.

4. LED-Blinken und Telegram-Nachricht:
   - Wenn das Fenster offen ist und es regnet, beginnt die LED zu blinken und eine Telegram-Nachricht wird gesendet.

5. Blinken stoppen:
   - Das Blinken stoppt, sobald das Fenster geschlossen oder der Regen aufgehört hat.

---

### Code-Anpassungen

- LED: Du kannst den Pin der LED ändern, indem du die `ledPin`-Konstante anpasst:
  
  const int ledPin = 2;  // Pin für die LED
  
- Sensoranpassung: Wenn du einen anderen Hall-Sensor oder einen anderen Pin verwendest, passe den Pin wie folgt an:
  
  const int hallSensorPin = 10;  // Pin für den Hall-Sensor
  

---

### Hochladen des Codes

1. Verbinde dein ESP32 mit dem Computer.
2. Öffne die Arduino IDE und wähle das ESP32-Board aus (`Werkzeuge -> Board -> ESP32 Dev Module`).
3. Wähle den richtigen Port aus (`Werkzeuge -> Port`).
4. Lade den Code auf das Board hoch (`Sketch -> Hochladen`).

---

### Testen

1. Öffne den Serial Monitor (`Strg + Umschalt + M` in der Arduino IDE), um die Debug-Ausgabe zu sehen.
2. Öffne das Fenster, um die LED blinken zu lassen und eine Nachricht zu erhalten, wenn es regnet.

---

### Fehlerbehebung

- Keine WLAN-Verbindung: Stelle sicher, dass der WLAN-Name und das Passwort korrekt eingegeben wurden.
- Keine Telegram-Nachricht: Überprüfe, ob der Bot-Token und die Chat-ID korrekt sind. Teste den Bot in Telegram, indem du ihm eine Nachricht sendest.

---

Viel Erfolg mit deinem Projekt!
