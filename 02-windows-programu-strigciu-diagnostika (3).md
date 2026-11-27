# 02. Programų strigčių diagnostika sistemoje „Windows“

**Data** – 2026-11-27  
**Aplinka** – „Windows 11 Home“  
**Įrankiai** – „Reliability Monitor“, „Event Viewer“

## Tikslas

Išmokti rasti užfiksuotą programos strigtį, susieti tos pačios strigties įrašus dviejuose įrankiuose ir eksportuoti vieną įvykį, kurį būtų galima pridėti prie IT pagalbos užklausos.

## Ką atlikau

1. Su `Win + R` paleidau `perfmon /rel`. „Reliability Monitor“ pasirinkau lapkričio 17 d. programos klaidą ir atidariau **View technical details**.
2. Su `eventvwr.msc` atidariau **Event Viewer → Windows Logs → Application**. Radau tos dienos **Application Error** įrašą (ID **1000**), užfiksuotą **00:03:31**. Abiejuose languose sutapo **Report Id**. Programos lauke buvo `bad_module_info`, modulio lauke – `unknown`, o išimties kodas – `0xc0000409`. Konkreti programa ir strigties priežastis liko nenustatytos.
3. Naudodamas **Filter Current Log…**, pirmiausia atrinkau įrašus pagal **Event ID 1000**. Iš **22 994** žurnalo įrašų liko **227**. Pridėjus šaltinį **Application Error**, liko **16** įrašų.
4. Palyginau du spalio 18 d. įrašus (**20:47:53** ir **20:51:02**). Sutapo programos ir modulio pavadinimai (`sp26-cod.exe`) bei išimties kodas (`0xc0000409`). Laikas ir proceso ID skyrėsi. Žurnale užfiksuotos dvi tos pačios programos strigtys per maždaug tris minutes.
5. Pažymėjau spalio 18 d. **20:47:53** įrašą ir pasirinkau **Save Selected Events…**. Išsaugojau `programos-strigtis.evtx`. Atidaręs failą skiltyje **Saved Logs**, patikrinau, kad jame yra vienas įvykis ir matomos jo techninės detalės.

## Išvada

Ieškodamas konkrečios strigties, pirmiausia tikrinu įvykio laiką, šaltinį ir programos pavadinimą. Sutampantis **Report Id** padėjo susieti tos pačios strigties įrašus. Sutampantis išimties kodas spalio ir lapkričio įrašuose leidžia juos palyginti, tačiau lapkričio įrašo konkrečiai programai priskirti nepakako duomenų.

## Pavyzdinė pastaba L2 specialistui

> The application crashed twice in about three minutes, and the cause is still unknown. I attached one Event Viewer entry for review.
