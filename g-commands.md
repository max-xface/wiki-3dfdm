---
title: G-Code Команды управления
description: 
published: true
date: 2023-03-12T10:07:35.945Z
tags: 
editor: markdown
dateCreated: 2023-03-12T09:35:13.590Z
---

# Аппаратное управление

## Коды управления лампой

| Команда | Описание |
| --- | --- |
| M960 S0 P0/1 | Калибровочные лазеры и калибровочная головка, белые светодиоды выкл./вкл. |
| M960 S1 P0/1 | Калибровка по оси Y ИК-лазер 808 нм (вертикальный) выкл./вкл. |
| M960 S2 P0/1 | Калибровка Ось X Лазер 405 нм (горизонтальный) выкл./вкл. |
| M960 S3 P0/1 | Вроде бы ничего, или я не нашел что это такое |
| M960 S4 P0/1 | Калибровочный блок белые светодиоды выкл./вкл. |
| M960 S5 P0/1 | Выключение/включение подсветки логотипа Bambu Lab |

**Пример:**

```typescript
M960 S5 P1 ;Включение подсветки логотипа печатающей головки Bambu Lab
```

## Коды управления вентилятором

| Команда | Описание |
| --- | --- |
| M106 P1 S0-255 | Part cooling fan off-full |
| M106 P2 S0-255 | Aux fan off-full |
| M106 P3 S0-255 | Chamber fan off-full |

**Пример:**

```typescript
M106 P3 S180 ;Включить вентилятор камеры на 70%
```

# Разное

## Коды сообщений на LCD-дисплее

Эти команды вызывают отображение сообщений на LCD-экране принтера и активируют отображение соответствующих сообщений в приложении/слайсере.

| Команда | Сообщение |
| --- | --- |
| M1002 gcode\_claim\_action : 0 | *(Clear screen of messages?)* |
| M1002 gcode\_claim\_action : 1 | Auto bed levelling |
| M1002 gcode\_claim\_action : 2 | Heatbed preheating |
| M1002 gcode\_claim\_action : 3 | Sweeping XY mech mode |
| M1002 gcode\_claim\_action : 4 | Changing filament |
| M1002 gcode\_claim\_action : 5 | M400 pause |
| M1002 gcode\_claim\_action : 6 | Paused due to filament runout |
| M1002 gcode\_claim\_action : 7 | Heating hotend |
| M1002 gcode\_claim\_action : 8 | Calibrating extrusion |
| M1002 gcode\_claim\_action : 9 | Scanning bed surface |
| M1002 gcode\_claim\_action : 10 | Inspecting first layer |
| M1002 gcode\_claim\_action : 11 | Identifying build plate type |
| M1002 gcode\_claim\_action : 12 | Calibrating Micro Lidar |
| M1002 gcode\_claim\_action : 13 | Homing toolhead |
| M1002 gcode\_claim\_action : 14 | Cleaning nozzle tip |
| M1002 gcode\_claim\_action : 15 | Checking extruder temperature |
| M1002 gcode\_claim\_action : 16 | Paused by the user |
| M1002 gcode\_claim\_action : 17 | Pause due to the falling off of the tool head’s front cover |
| M1002 gcode\_claim\_action : 18 | Calibrating the micro lidar |
| M1002 gcode\_claim\_action : 19 | Calibrating extruder flow |
| M1002 gcode\_claim\_action : 20 | Paused due to nozzle temperature malfunction |
| M1002 gcode\_claim\_action : 21 | Paused due to heat bed temperature malfunction |