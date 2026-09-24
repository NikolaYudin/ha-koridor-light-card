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

card-mod берёт `rgb_color` и `brightness` лампы и рисует заливку:
цвет лампы → до процента яркости → прозрачно. Выключено — обычный фон
карточки. Альфа-каналы `0.40 / 0.22` в `lovelace/koridor_card.yaml` — насыщенность.

## 🔘 Логика индикации сцен

- **Яркий**: группа вкл и яркость ≥ 200
- **Ночной**: группа вкл, яркость ≤ 128 и лампа 1 вкл
- **Цветы**: лампа 4 вкл на 38, лампы 1–3 выкл

Индикация считается по фактическому состоянию света, а не по факту нажатия:
поменяли яркость вручную — подсветка сцены погаснет.

## ❓ FAQ

**Q: Пилюли без иконок — как вернуть иконки?**
A: Удалите блок `mushroom-state-item$: | .icon { display: none … }` в карточках ламп.

**Q: Градиент не появляется.**
A: Проверьте, что установлен card-mod и обновлён (HACS → Frontend), сделайте Ctrl+F5.

**Q: Сцены не подсвечиваются.**
A: Сверьте entity_id в условиях `icon_color` с реальными (Developer Tools → States).

**Q: Лампы только CCT, без RGB.**
A: Замените `rgb_color` в сцене «Цветы» на `color_temp_kelvin`; градиент подхватит
конвертированный цвет автоматически.

## 🛠 Troubleshooting

| Симптом | Решение |
|---|---|
| Карточка не рисуется | Установлены ли mushroom-cards и card-mod |
| Сцены не создались | Проверить конфигурацию → перезапуск |
| Пилюли слишком узкие | `columns: 4` → `3` или `2` в блоке ламп |
| Заливка слишком бледная | Альфы `0.40/0.22` → `0.55/0.35` |

## 🗺 Roadmap

- [ ] Автоматизации: движение + закат → Ночной, движение днём → Яркий
- [ ] Плавные переходы (transition) между сценами
- [ ] Wall-версия для настенной панели
- [ ] Пресеты «Утро / День / Вечер»

## 📄 Лицензия

MIT © NikolaYudin. См. [LICENSE](LICENSE).

---

<p align="center"><i>Пригодилось — поставьте ⭐</i></p>
