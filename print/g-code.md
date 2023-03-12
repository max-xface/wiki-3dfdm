---
title: Custom G-Code
description: 
published: true
date: 2023-03-12T16:54:11.396Z
tags: 
editor: markdown
dateCreated: 2023-03-10T10:26:32.507Z
---

## Z-offset в зависимости от типа используемой пластины

Для изменения z-offset в зависимости от типа используемой пластины, нужно вставить в стартовый g-code следующий раздел:

```gcode
;===== Z-Offset correction for Engineering Plate, High Temp Plate and Textured PEI Plate ===============
;curr_bed_type={curr_bed_type}
{if curr_bed_type=="Engineering Plate"}  
     G29.1 Z0.03  ;raise 0.03mm for Engineering Plate
{elsif curr_bed_type=="High Temp Plate"}  
     G29.1 Z0.02  ;raise 0.02mm for High Temp Plate
{elsif curr_bed_type=="Textured PEI Plate"}  
     G29.1 Z{-0.05}  ;squish 0.05mm for Textured PEI Plate
{endif}
```

Собственно, из кода видно, что зазор для Engineering Plate увеличивается на 0,03мм, для High Temp Plate увеличивается на 0,02мм, и для Textured PEI Plate уменьшается на 0,05мм.

Величина коррекции подобрана экспериментальным путём, и для каждого конкретного принтера скорее всего будет отличаться.

Я вставил этот кусок перед разделом "nozzle load line", но, возможно, методологически более верным было бы вставить его после калибровки потока (то есть перед разделом "turn off light and wait extrude temperature").

Используйте на свой страх и риск 😉😁

## Меняем стартовый g-code для всех системных профилей пластика одного типа

Базовые настройки профилей лежат по пути(обязательно нужен первый запуск студии после установки):

```plaintext
c:\Users\<имя юзера в windows>\AppData\Roaming\BambuStudio\system\BBL\filament\
```

Чтобы поменять стартовый g-code для всех дефолтных профилей одного типа(например для печати PLA задать chamber кулеру скорость 50%) делаем следующее:

-   по пути выше ищем и редактируем файл **fdm\_filament\_pla.json**
-   в конце файла будет базовый g-code, в котором меняем скорость вентилятора на те, которые нужны:
-   для generic профиля меняем в файле **Generic PLA @base.json**

```json
"filament_start_gcode": [
        "; filament start gcode\n{if  (bed_temperature[current_extruder] >45)||(bed_temperature_initial_layer
        [current_extruder] >45)}M106 P3 S125\n{elsif(bed_temperature[current_extruder] >35)||
        (bed_temperature_initial_layer[current_extruder] >35)}M106 P3 S125\n{endif}"
    ]
```

**M106 P3 S125** - в данном случае **S125** это 50% от скорости(диаппазон значений **S0 - S255**).

> Примечание: перед обновлением версии сохраните эти файлы на всякий случай, возможно, bambu studio их перепишет на дефолт

> Примечание: финишный g-code находится в файле **fdm\_filament\_common.json,** применяется для всех системных профилей всех типов пластика



