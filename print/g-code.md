---
title: Custom G-Code
description: 
published: true
date: 2023-03-12T23:16:24.117Z
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

## Меняем стартовый g-code для всех системных профилей пластика одного типа (v1.5.0.60)

Базовые настройки профилей лежат по пути(обязательно нужен первый запуск студии после установки):

```plaintext
c:\Users\<имя юзера в windows>\AppData\Roaming\BambuStudio\system\BBL\filament\
```

Чтобы поменять стартовый g-code для всех дефолтных профилей одного типа(например для печати **Bambu PLA XXXXX** задать chamber кулеру скорость 50%) делаем следующее:

-   по пути выше ищем и редактируем файл **fdm\_filament\_pla.json**
-   в конце файла будет базовый g-code, в котором меняем скорость вентилятора на ту, которая нужна:
-   для generic профиля меняем в файле **Generic PLA @base.json**

```json
"filament_start_gcode": [
        "; filament start gcode\n{if  (bed_temperature[current_extruder] >45)||(bed_temperature_initial_layer
        [current_extruder] >45)}M106 P3 S125\n{elsif(bed_temperature[current_extruder] >35)||
        (bed_temperature_initial_layer[current_extruder] >35)}M106 P3 S125\n{endif}"
    ]
```

**M106 P3 S125** - в данном случае **S125** это 50% от скорости(диаппазон значений **S0 - S255**).
Субъективно, я не вижу смысла вообще раскручивать вентилятор chamber выше 50%, так как он не сильно влияет на температуру в камере.
Из опыта пользователей, его стоит включать даже при печати ABS, снижается вредные испарения, так как создается разрежение внутри камеры и воздух из нее выходит, преимущественно, через угольный фильтр. ***(прошу поправить, если не прав, физику учил давно и уже мог что-то забыть)***

> Примечание 1: перед обновлением версии сохраните эти файлы на всякий случай, возможно, bambu studio их перепишет на дефолт

> Примечание 2: финишный g-code находится в файле **fdm\_filament\_common.json,** применяется для всех системных профилей всех типов пластика

> Примечание 3: например, Generic PLA и Generic PLA Silk это разыне типы и каждому надо менять g-code в соответствующих файлах

#### Список файлов, где задана скорость chamber-вентилятора в стартовом g-code:
**BBL X1C:**
fdm_filament_pet.json
fdm_filament_pla.json
fdm_filament_pva.json
fdm_filament_tpu.json
Generic PETG @base.json
Generic PLA @0.2 nozzle.json
Generic PLA @base.json
Generic PLA Silk @base.json
Generic PLA-CF @base.json
Generic TPU.json
PolyLite PLA @BBL X1C.json
PolyTerra PLA @BBL X1C.json

**BBL P1P:**
Generic PLA @BBL P1P 0.2 nozzle.json
Generic TPU @BBL P1P.json
> Примечание: не ясно почему для P1P задан этот параметр, возможно не досмотрели разработчики)
