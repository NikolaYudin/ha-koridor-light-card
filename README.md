<p align="center">
  <img src="https://img.shields.io/github/v/release/YOUR_NICK/ha-koridor-light-card?style=flat-square" alt="Release">
  <img src="https://img.shields.io/github/license/YOUR_NICK/ha-koridor-light-card?style=flat-square" alt="License">
  <img src="https://img.shields.io/github/stars/YOUR_NICK/ha-koridor-light-card?style=flat-square" alt="Stars">
  <img src="https://img.shields.io/badge/Home%20Assistant-2024.8%2B-41BDF5?style=flat-square&logo=homeassistant" alt="HA">
</p>

<h1 align="center">💡 Koridor Light PRO</h1>

<p align="center">
  Панель управления освещением коридора для Home Assistant<br>
  <i>Группа ламп · 3 сцены · индикация · автоматизации</i>
</p>

<p align="center">
  <img src="screenshots/bright.png" width="45%">
  <img src="screenshots/flowers.png" width="45%">
</p>

## ✨ Что внутри

- 💡 **Группа ламп** — полное управление 4 лампами (яркость, цветовая температура, RGB)
- 🎬 **3 сцены**: Яркий (100%, 4000K), Ночной (10%, 2700K), Цветы (акцентная подсветка)
- 🔘 **4 кнопки ламп** — тап для вкл/выкл, долгое нажатие для more-info
- 🌈 **Индикация** — горящие лампы подсвечиваются янтарным, активные сцены цветные
- 🤖 **Автоматизации** — ночной/яркий режим по датчику движения и времени суток
- 🎨 **Адаптив к теме** — поверхности через `var(--…)`, работает в светлой и тёмной теме
- 🧩 **Package-подход** — один файл, не ломает `configuration.yaml`

## 🧩 Требования

- Home Assistant **2024.8+**
- Компоненты из HACS (Frontend):
  - [mushroom-cards](https://github.com/piitaya/lovelace-mushroom)
- 4 умные лампы (автор использует CAMELION GX53 10Вт RGB+CCT)
- Группа ламп (`light.koridor`)
- Опционально: датчик движения для автоматизаций

## 🚀 Установка за 5 минут

### 1. Установите зависимость
HACS → Frontend → установите **mushroom-cards**.

### 2. Скопируйте package-файл
```bash
packages/koridor.yaml  →  /config/packages/
```

### 3. Подключите packages (если ещё не сделано)
Добавьте в `configuration.yaml`:
```yaml
homeassistant:
  packages: !include_dir_named packages
```

### 4. Замените entity_id ламп
В `packages/koridor.yaml` замените:
```
light.camelion1  →  light.ваша_лампа_1
light.camelion2  →  light.ваша_лампа_2
light.camelion3  →  light.ваша_лампа_3
light.camelion4  →  light.ваша_лампа_4
```

В `lovelace/koridor_card.yaml` замените те же entity_id (4 места).

### 5. Создайте группу ламп
Настройки → Устройства и службы → Помощники → Создать помощника → Группа → Свет:
- Название: `Коридор`
- Entity ID: `light.koridor`
- Добавьте все 4 лампы

### 6. Проверьте и перезапустите
Настройки → Система → Проверить конфигурацию → Перезапустить.

### 7. Добавьте карточку
Дашборд → ⋮ → Редактор кода → вставьте `lovelace/koridor_card.yaml`.

## ⚙️ Настройка

| Что заменить | Где | Пример |
|---|---|---|
| `light.camelion1` … `light.camelion4` | package + карточка | `light.yeelight_1` |
| `light.koridor` | карточка (1 место) | `light.hallway` |
| `binary_sensor.motion_koridor` | package (2 места) | `binary_sensor.pir_hall` |
| Цвета сцен | `packages/koridor.yaml` | `rgb_color: [255, 0, 0]` для красного |

## 🎨 Сцены

### Яркий (`scene.koridor_bright`)
- Все 4 лампы включены
- Яркость: 100% (255)
- Цветовая температура: 4000K (нейтральный белый)

### Ночной (`scene.koridor_night`)
- Все 4 лампы включены
- Яркость: 10% (25)
- Цветовая температура: 2700K (тёплый приглушённый)

### Цветы (`scene.koridor_flowers`)
- Лампы 1–3 выключены
- Лампа 4 включена:
  - Яркость: 15% (38)
  - Цвет: фиолетовый RGB [148, 0, 211]

## 🌈 Индикация

- **Лампы (Л1–Л4):** янтарная иконка = включена, серая = выключена
- **Сцены:** цветная кнопка = сцена активна (проверяется по фактическому состоянию света)
  - Яркий: группа вкл + яркость ≥ 200
  - Ночной: группа вкл + яркость ≤ 128 + лампа 1 вкл
  - Цветы: только лампа 4 вкл + яркость 38

## 🤖 Автоматизации

### Ночной режим
- **Триггер:** датчик движения сработал
- **Условие:** после заката (с запасом 30 мин)
- **Действие:** применить сцену «Ночной»

### Яркий режим
- **Триггер:** датчик движения сработал
- **Условие:** до заката (с запасом 1 час)
- **Действие:** применить сцену «Яркий»

## 🎨 Кастомизация

| Что крутить | Где | Эффект |
|---|---|---|
| `amber` (4 места) | кнопки ламп | цвет индикации включённых ламп |
| `brightness: 255/25/38` | package | яркость сцен (0–255) |
| `color_temp_kelvin: 4000/2700` | package | цветовая температура |
| `rgb_color: [148, 0, 211]` | package | цвет сцены «Цветы» |

## 🌗 Темы

Поверхности и тексты берутся из переменных темы HA (`--ha-card-background`, `--primary-text-color` и др.), поэтому карточка нативно выглядит и в светлой, и в тёмной теме. Mushroom cards автоматически адаптируются.

## ❓ FAQ

**Q: Сцены не работают после перезапуска.**
A: Проверьте, что в `configuration.yaml` есть `scene: !include scenes.yaml` или `packages` подключены правильно.

**Q: Индикация сцен не подсвечивается.**
A: Убедитесь, что entity_id группы в карточке совпадает с реальным (`light.koridor`). Проверьте атрибуты группы в Developer Tools → States.

**Q: Хочу больше сцен.**
A: Добавьте новую сцену в `packages/koridor.yaml` и кнопку в блок `grid` (колонки 4).

**Q: Лампы не RGB, только CCT.**
A: Удалите `rgb_color` из сцены «Цветы» и замените на `color_temp_kelvin`.

**Q: Нет датчика движения.**
A: Удалите автоматизации из package-файла или замените триггер на другое событие (например, открытие двери).

## 🛠 Troubleshooting

| Симптом | Причина | Решение |
|---|---|---|
| `mushroom-light-card is not installed` | Не установлен HACS-компонент | HACS → Frontend → mushroom-cards |
| Сцены не появляются | YAML не загрузился | Проверить конфигурацию → Перезапустить |
| Индикация серая при активной сцене | Неправильные условия в шаблонах | Проверьте атрибуты ламп в States |
| Группа не создаётся | Нет entity_id ламп | Создайте лампы сначала, затем группу |
| Автоматизации не срабатывают | Неправильный entity_id датчика | Замените `binary_sensor.motion_koridor` |

## 🗺 Roadmap

- [ ] Пресеты для разных времён суток (утро / день / вечер / ночь)
- [ ] Плавное затухание при выключении (transition)
- [ ] Синхронизация с другими комнатами (весь дом яркий/ночной)
- [ ] Голосовое управление через Яндекс Алису / Google Assistant
- [ ] Уведомления о перегоревших лампах

## 🤝 Contributing

PR'ы и идеи приветствуются! См. [CONTRIBUTING.md](CONTRIBUTING.md).

## 📄 Лицензия

MIT © [YOUR_NAME]. См. [LICENSE](LICENSE).

---

<p align="center">
  <i>Если панель пригодилась — поставьте ⭐, это лучшая благодарность.</i>
</p>
