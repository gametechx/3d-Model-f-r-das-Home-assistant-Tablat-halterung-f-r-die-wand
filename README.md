# 📱 Home Assistant Tablet-Wandhalterung (Versteckt)

Dieses Projekt umfasst ein 3D-gedrucktes Gehäuse für eine flache Wandmontage eines Tablets als Home Assistant Dashboard. Das Besondere: Ein integrierter **ESP32-C3 Super Mini** steuert eine LED-Hintergrundbeleuchtung und überwacht das Raumklima.

---

## ✨ Features
* **Indirektes Licht:** Steuerbarer RGB-LED-Streifen (WS2812B) für Statusanzeigen oder Ambiente.
* **Klimasensor:** Integrierter DHT11 für Temperatur und Luftfeuchtigkeit.
* **Verstecktes Design:** Kabelführung und Elektronik sind komplett im Gehäuse integriert.
* **ESPHome Ready:** Einfache Integration in Home Assistant mit Farbrad und Effekten.

---

## 🛠 Materialliste

### Elektronik
* **ESP32-C3 Super Mini** (Kompakter Mikrocontroller)
* **DHT11 Sensor** (3-Pin Version)
* **WS2812B LED-Streifen** (3-Pin, 5V)
* **Kabel:** USB-A auf USB-C (Stromversorgung), Micro-USB/USB-C (Tablet-Laden)
* **Stecker:** USB-A Buchse, USB-C Stecker (für interne Verteilung)

### Montage & Druck
* **3D-Druck:** Alle STL/F3D Dateien aus diesem Repo (Empfehlung: PETG)
* **Befestigung:** Fischer Dübel & Schrauben, starkes doppelseitiges Klebeband (z.B. 3M VHB)
* **Isolierung:** Schrumpfschläuche

---

## 📐 3D-Modell & Druckhinweise
Die Halterung besteht aus mehreren Teilen, um die Montage zu erleichtern:
* **Hauptplatte:** Basis für Wandmontage und Elektronik-Aufnahme.
* **Seitenleisten:** Zum Fixieren und optischen Abschluss.

**Druck-Empfehlungen:**
* **Material:** PETG (wegen der Wärmeentwicklung von Tablet/ESP32).
* **Infill:** 20-30% für genug Stabilität an den Schraublöchern.
* **Support:** "Tree-Supports" für die Kabelschächte aktivieren.

---

## ⚡ Verkabelung (Wiring)

| Bauteil | Anschluss | ESP32-C3 Pin |
| :--- | :--- | :--- |
| **LED Streifen (Data)** | DIN | **GPIO 4** |
| **DHT11 (Data)** | OUT | **GPIO 5** |
| **Stromversorgung** | 5V / 3.3V | VBUS / 3V3 |

**Wichtig:** Alle GND-Leitungen müssen miteinander verbunden werden. Nutze Schrumpfschläuche für alle Lötstellen!

---

## 💻 Software (ESPHome)

Nutze diesen Code für deine ESPHome-Konfiguration, um das Farbrad und die Sensoren in Home Assistant zu erhalten:

```yaml
esphome:
  name: tablet-wall-mount

esp32:
  board: esp32-c3-devkitm-1
  variant: esp32c3

wifi:
  ssid: "DEIN_WLAN_NAME"
  password: "DEIN_PASSWORT"

api:
ota:
  - platform: esphome

light:
  - platform: neopixelbus
    type: GRB
    variant: WS2812
    pin: GPIO4
    num_leds: 30
    name: "Dashboard Licht"
    effects:
      - pulse:
          name: "Sanftes Atmen"
      - rainbow:
          name: "Regenbogen"

sensor:
  - platform: dht
    pin: GPIO5
    model: DHT11
    temperature:
      name: "Temperatur Dashboard"
    humidity:
      name: "Feuchtigkeit Dashboard"
