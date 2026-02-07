# 🪚 Sägesteuerung – ESP32 Touchdisplay mit ESPHome

Dark-Mode Touchscreen-UI für eine Werkstatt-Sägesteuerung auf dem **WT32-SC01** Display (ESP32 + 3.5" ILI9xxx).  
Zwei große Toggle-Buttons im Kapsel/Pill-Design steuern **Säge** und **Absaugung** – direkt am Gerät oder über Home Assistant.

![UI Preview](./unnamed.jpg)

---

## ✨ Features

- **Dark-Mode UI** – Optimiert für hohen Kontrast in Werkstattumgebungen
- **2 Toggle-Buttons** (Säge & Absaugung) im Pill/Kapsel-Design mit Türkis/Mint-Akzent
- **Lokale Switches** – Die Entitäten leben auf dem ESP, kein manuelles Anlegen in HA nötig
- **Bidirektional** – Steuerung sowohl über das Touch-Display als auch über Home Assistant
- **Offline-fähig** – Funktioniert auch ohne Home Assistant Verbindung
- **Auto-Dimming** – Display dimmt nach 30s Inaktivität automatisch herunter
- **WiFi-Statusanzeige** – Kleine Anzeige unten links
- **Remote-Package** – Direkt aus GitHub in ESPHome einbindbar, keine lokalen Dateien nötig
- **Nur MDI-Icons** – Keine externen Bilddateien erforderlich

---

## 🛠 Hardware

- **Display:** [WT32-SC01](http://www.wireless-tag.com/product-item-2.html) (ESP32-WROVER + 3.5" 480×320 ILI9xxx Touchscreen)
- **Kein weiteres Zubehör nötig** – Alles über Touch & WiFi

---

## 🚀 Installation

### 1. Secrets in ESPHome anlegen

In deiner **lokalen** `secrets.yaml` im ESPHome-Verzeichnis (auf dem HA-Server) müssen folgende Einträge existieren:

```yaml
ha_defaultkey: "DEIN_BASE64_API_KEY"      # HA API-Schlüssel
esphome_ota_pw: "dein_ota_passwort"        # OTA-Update Passwort
wifi_ssid_iot: "DEIN_WLAN_NAME"            # WiFi SSID
wifi_pw_iot: "DEIN_WLAN_PASSWORT"          # WiFi Passwort
ip_saegesteuerung: "192.168.1.100"         # Statische IP des Displays
ip_iot_gateway: "192.168.1.1"              # Gateway
ip_iot_subnet: "255.255.255.0"             # Subnetz
ip_iot_dns: "192.168.1.1"                  # DNS Server
esphome_fb_pw: "fallback_passwort"         # Fallback-Hotspot Passwort
```

Eine Vorlage findest du in [`secrets.yaml.example`](./secrets.yaml.example).

### 2. ESPHome-Config erstellen

Erstelle eine neue Datei in deinem ESPHome-Dashboard (z.B. `saege.yaml`) mit folgendem Inhalt:

```yaml
packages:
  saege:
    url: https://github.com/linbo24/wt32sc01-with-esphome
    ref: main
    files:
      - saegesteuerung.yaml
```

Das ist alles! ESPHome lädt die komplette Konfiguration automatisch aus diesem Repository.

### 3. Optional: Werte überschreiben

Du kannst jede Einstellung aus dem Package lokal überschreiben. Beispiel:

```yaml
packages:
  saege:
    url: https://github.com/linbo24/wt32sc01-with-esphome
    ref: main
    files:
      - saegesteuerung.yaml

# Eigenen Namen vergeben:
substitutions:
  name: "werkstatt-display"
  friendly_name: "Werkstatt Display"

# Statische IP weglassen (DHCP nutzen):
wifi:
  manual_ip:
```

### 4. Flashen

1. Klicke im ESPHome-Dashboard auf **Install**
2. Beim **ersten Mal** über USB: **"Plug into this computer"**
3. Danach geht alles **Over-the-Air** (OTA)

### 5. In Home Assistant

Nach dem Flashen erscheint das Gerät automatisch unter **Einstellungen → Geräte & Dienste → ESPHome**.  
Folgende Entitäten werden **automatisch** erstellt:

| Entität | Beschreibung |
|---|---|
| `switch.saegesteuerung_saege` | Toggle für die Säge |
| `switch.saegesteuerung_absaugung` | Toggle für die Absaugung |
| `light.saegesteuerung_backlight` | Display-Helligkeit |

---

## 📁 Projektstruktur

```
wt32sc01-with-esphome/
├── saegesteuerung.yaml      # ← Hauptkonfiguration (self-contained, als Remote-Package nutzbar)
├── secrets.yaml.example     # Vorlage für secrets.yaml
├── .gitignore               # Schützt secrets.yaml vor Commit
├── README.md
├── unnamed.jpg              # UI-Preview
├── includes/                # Touch-Helper (nur für Original-Dashboard)
│   ├── iTouch.yaml
│   └── iTouch2.yaml
├── images/                  # Wetterbilder (nur für Original-Dashboard)
│   └── weather1/
├── wt32sc01a.yaml           # Original Wetter-Dashboard Variante A
└── wt32sc01b.yaml           # Original Wetter-Dashboard Variante B
```

---

## ⚙️ Anpassung

### Andere Entitäts-Namen

Überschreibe `substitutions` in deiner lokalen Config:

```yaml
substitutions:
  name: "mein-geraet"
  friendly_name: "Mein Gerät"
```

### Statische IP entfernen

Wenn du DHCP statt einer festen IP verwenden willst, überschreibe den `wifi`-Block ohne `manual_ip`.

### Dimming-Timeout ändern

Überschreibe die `undim_script`-Sektion mit einem anderen `delay`-Wert.

---

## 🔒 Sicherheit

- `secrets.yaml` ist per `.gitignore` geschützt und wird **nicht** ins Repository committed
- Alle sensiblen Daten (WiFi, API-Keys, IPs) werden über `!secret` referenziert und liegen nur lokal
- Der Fallback-Hotspot wird nur aktiv, wenn das konfigurierte WiFi nicht erreichbar ist

---

## 📜 Lizenz & Credits

Basiert auf dem [WT32-SC01 ESPHome-Projekt](https://community.home-assistant.io/t/wt32-sc01-with-esphome/473531) und Beiträgen der Home Assistant Community.

---

## 🗂 Original Wetter-Dashboard

Die Dateien `wt32sc01a.yaml` und `wt32sc01b.yaml` enthalten das originale Wetter-/Smart-Home-Dashboard mit Wettervorhersage, Temperaturanzeigen und 7 Buttons. Diese nutzen lokale Bilddateien und `!include`-Referenzen und sind **nicht** als Remote-Package geeignet.

![Original Dashboard](./display2.jpg)

