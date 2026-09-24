<p align="center">
  <img src="https://img.shields.io/github/v/release/YOUR_NICK/ha-koridor-light-card?style=flat-square" alt="Release">
  <img src="https://img.shields.io/github/license/YOUR_NICK/ha-koridor-light-card?style=flat-square" alt="License">
  <img src="https://img.shields.io/badge/Home%20Assistant-2024.8%2B-41BDF5?style=flat-square&logo=homeassistant" alt="HA">
</p>

<h1 align="center">💡 Koridor Light PRO</h1>

<p align="center">
  Панель освещения коридора: группа, 4 лампы-пилюли с живым градиентом, 3 сцены с индикацией<br>
  <i>Один package-файл · один стек карточки · две зависимости из HACS</i>
</p>

<p align="center">
  <img src="screenshots/bright.png" width="45%">
  <img src="screenshots/flowers.png" width="45%">
</p>

## ✨ Что внутри

- 💡 **Группа** — полное управление: яркость, цветовая температура, RGB, слайдер
- 🎬 **3 сцены**: Яркий (100%, 4000K), Ночной (10%, 2700K), Цветы (акцент RGB на одной лампе)
- 🟪 **Лампы-пилюли** — узкие карточки без иконок: имя + состояние, слайдер, тап = вкл/выкл
- 🌈 **Живой градиент** — фон пилюли заливается текущим цветом лампы до процента яркости
- 🔘 **Индикация сцен** — кнопка сцены подсвечивается, только если свет реально в этом состоянии
- 🎨 **Темы** — поверхности через `var(--…)`, нативно в светлой и тёмной
- 🧩 **Package-подход** — один файл, не ломает `configuration.yaml`

## 🧩 Требования

- Home Assistant **2024.8+**
- HACS (Frontend): **mushroom-cards**, **card-mod**
- Группа ламп (`light.koridor`) и 4 управляемые лампы (RGB или CCT)

## 🚀 Установка

1. HACS → Frontend → установите **mushroom-cards** и **card-mod**.
2. Скопируйте `packages/koridor.yaml` в `/config/packages/`.
3. В `configuration.yaml` (один раз):
   ```yaml
   homeassistant:
     packages: !include_dir_named packages
   ```
4. Замените entity_id ламп и группы на свои в `packages/koridor.yaml`
   и `lovelace/koridor_card.yaml` (`light.camelion1…4`, `light.koridor`).
5. Проверьте конфигурацию → перезапуск.
6. Дашборд → ⋮ → Редактор кода → вставьте `lovelace/koridor_card.yaml`.

## ⚙️ Настройка

| Что заменить | Где | Пример |
|---|---|---|
| `light.camelion1…4` | package + карточка | `light.yeelight_1…4` |
| `light.koridor` | карточка + сцены | `light.hallway` |
| `brightness: 255/25/38` | package | яркость сцен (0–255) |
| `rgb_color: [148, 0, 211]` | package | цвет акцент-сцены |

## 🌈 Как работает градиент пилюли

card-mod берёт `rgb_color` и `brightness` лампы и рис
