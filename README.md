WT32SC01 display with Esphome and Home-Assistant
================================================
Example config for a WT32SC01 ( WT32-SC01 with ESP32 WRover module ) display using Esphome, controlling Home Assistant entities and using Home Assistant sensors to display Meteo forecast, temperatures,....

## Sägesteuerung (saegesteuerung.yaml)
Dark-Mode UI für eine Werkstatt-Sägesteuerung mit zwei großen Toggle-Buttons im Kapsel/Pill-Design:
- **SÄGE** – schaltet `switch.saege` in Home Assistant
- **ABSAUGUNG** – schaltet `switch.absaugung` in Home Assistant

### Anpassung
In der Datei `saegesteuerung.yaml` unter `substitutions` die Entitäten anpassen:
```yaml
switch1_entity: "switch.saege"       # Deine Säge-Entität
switch2_entity: "switch.absaugung"   # Deine Absaugung-Entität
```
Außerdem `ip_saegesteuerung` in deiner `secrets.yaml` anlegen.

## Original Wetter-Dashboard (wt32sc01a.yaml / wt32sc01b.yaml)
The meteo forecaste part is depending on a template sensor in Home Assistant. See ./sensor_ha/forecast_8hours.yaml. 
The images in ./images/weather1 should be places in the home assistant config/esphome/images/weather1 folder 

In the latest version I added a RCWL-0516 radar sensor behind the display to undim the screen... Also stopped using the 'delayed_on' and 'delayed_off' filters of the binary sensor to 'debounce' the touchscreen. Was not working for me....  Now using script with delay...

The code in this project is based on contributions of many others on https://community.home-assistant.io/!
 
![display.jpg](./display.jpg)

![display2.jpg](./display2.jpg)

