# 🪚 Sägesteuerung – ESP32 Touchdisplay mit ESPHome

Dark-Mode Touchscreen-UI für eine Werkstatt-Sägesteuerung auf dem **WT32-SC01** Display (ESP32 + 3.5" ILI9xxx).  
Zwei große Toggle-Buttons steuern **Säge** und **Absaugung** – direkt am Gerät oder über Home Assistant.

![UI Preview](./unnamed.jpg)

---

## ✨ Features

- **Dark-Mode UI** – Hoher Kontrast für Werkstattumgebungen
- **2 Toggle-Buttons** (Säge & Absaugung) mit Türkis/Mint-Akzent
- **Lokale Switches** – Entitäten leben auf dem ESP, kein Anlegen in HA nötig
- **Bidirektional** – Steuerung über Touch-Display und Home Assistant
- **Offline-fähig** – Funktioniert ohne Home Assistant Verbindung
- **Optimiert** – Minimale CPU-Last, Display wird nur bei Änderung aktualisiert
- **Remote-Package** – Direkt aus GitHub einbindbar, keine lokalen Dateien nötig

---

## 🚀 Installation

### 1. Secrets in ESPHome anlegen

In deiner **lokalen** `secrets.yaml` im ESPHome-Verzeichnis:

```yaml
ha_defaultkey: "DEIN_BASE64_API_KEY"
esphome_ota_pw: "dein_ota_passwort"
wifi_ssid_iot: "DEIN_WLAN_NAME"
wifi_pw_iot: "DEIN_WLAN_PASSWORT"
ip_saegesteuerung: "192.168.1.100"
ip_iot_gateway: "192.168.1.1"
ip_iot_subnet: "255.255.255.0"
ip_iot_dns: "192.168.1.1"
esphome_fb_pw: "fallback_passwort"
```

### 2. ESPHome-Config erstellen

Neue Datei im ESPHome-Dashboard (z.B. `saege.yaml`):

```yaml
packages:
  saege:
    url: https://github.com/linbo24/wt32sc01-with-esphome
    ref: main
    files:
      - saegesteuerung.yaml
```

### 3. Flashen

Beim **ersten Mal** über USB, danach Over-the-Air (OTA).

### 4. Entitäten in Home Assistant

Erscheinen automatisch:

| Entität | Beschreibung |
|---|---|
| `switch.saegesteuerung_saege` | Toggle Säge |
| `switch.saegesteuerung_absaugung` | Toggle Absaugung |
| `light.saegesteuerung_backlight` | Display-Helligkeit |

---

## 📁 Dateien

```
├── saegesteuerung.yaml      # Hauptkonfiguration (self-contained)
├── secrets.yaml.example     # Vorlage für secrets.yaml
├── .gitignore
└── README.md
```

---

## ⚙️ Anpassung

Werte lokal überschreiben:

```yaml
packages:
  saege:
    url: https://github.com/linbo24/wt32sc01-with-esphome
    ref: main
    files:
      - saegesteuerung.yaml

substitutions:
  name: "werkstatt-display"
  friendly_name: "Werkstatt Display"
```

---

## 📜 Credits

Basiert auf dem [WT32-SC01 ESPHome-Projekt](https://community.home-assistant.io/t/wt32-sc01-with-esphome/473531) der Home Assistant Community.

