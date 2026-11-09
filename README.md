# ESPHome-Zehnder-RF

Aansturing van een Zehnder ComfoFan Silent via een ESP32 en nRF905 (868 MHz), met ESPHome en Home Assistant.

## Hardware
- Zehnder ComfoFan Silent (Randaarde): https://www.zehnder.nl/nl/ventilatie/mechanische-ventilatie/comfofan-silent
- Zehnder RFZ zender
- Zehnder CO2 Sensor 0-10V
- ESP32 (esp32doit-devkit-v1) + nRF905-module (3,3 V, stabiele voeding!)

## Software
- ESPHome 2026.10.x (laatst gecontroleerd tegen de changelog; zelf compileren en testen blijft nodig)
- Home Assistant core: de versie die bij je ESPHome-add-on past

## Gebruik
1. Kopieer `secrets.yaml.example` naar `secrets.yaml` en vul je eigen waarden in. Zet `secrets.yaml` nooit in git.
2. Pas in `utility-bridge.yaml` het vaste IP-adres aan (of verwijder het `manual_ip`-blok).
3. Compileer en flash.

## Koppelen (pairing)
De ESP onthoudt de koppeling in de opgeslagen instellingen. Reageert de Zehnder niet meer:
1. Haal de Zehnder 10 seconden van de spanning.
2. Zet de spanning terug en herstart direct daarna de ESP. De Zehnder staat dan 60 seconden open voor een nieuwe koppeling.

## Wijzigingen t.o.v. de vorige versie
- `control()` laat opdrachten uit Home Assistant niet meer vallen als de radio bezig is; ze komen in de wachtrij.
- Status (`state_`) verandert alleen nog als de zending echt gestart is (geen vastlopers meer bij een bezette radio).
- Koppeling wordt na het opslaan direct naar flash geschreven en er wordt gelogd als opslaan mislukt of er geen koppeling is.
- Ontbrekende `break` in de state machine en een dubbele `rfComplete()` opgeruimd.
- Knoppen in de YAML gebruiken `fan.turn_on`, zodat de status in Home Assistant meeloopt.
- `secrets.yaml` is uit de repo gehaald; er staat nu een `secrets.yaml.example`.

## Logmeldingen
| Melding | Betekenis |
|---|---|
| `Config data valid, start polling` | Koppeling is aanwezig |
| `Invalid config, start paring` / `No stored pairing config found` | Geen koppeling, opnieuw koppelen |
| `Config write failed` | SPI- of voedingsprobleem met de nRF905 |
| `Query Timeout` / `Airway too busy` | Geen antwoord van de Zehnder of storing op de band |

## Dank aan
- https://github.com/eelcohn/nRF905-API
- https://www.jbswebcom.nl/knutselen/index.php?view=article&id=32:zehnder-comfofan-silent-aansturen-met-home-assistant-via-wemos-d1-met-nrf905-en-esphome&catid=2
- https://github.com/Sanderhuisman/ESPHome-Zehnder-RF
- https://github.com/kdv88/ESPHome-Zehnder-RF
