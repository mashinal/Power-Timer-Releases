# Power Timer

Power Timer - Windows-приложение для управления питанием: выключение, перезагрузка, сон, пробуждение, Wake-on-LAN и правила планировщика.

Текущий релиз: 1.1.0.

## Возможности

- Таймер выключения, перезагрузки и сна.
- Отмена активного таймера и отмена системного `shutdown`.
- Пробуждение из сна через Планировщик заданий Windows.
- Wake-on-LAN по MAC/IP/порту.
- Scheduler для правил по времени, дням недели и интервалам.
- Работа в системном трее.
- Single instance: повторный запуск активирует уже открытое окно.
- Production-структура папок рядом с приложением.

## Структура релиза

После сборки приложение находится в `dist`:

```text
dist/
├── PowerTimer.exe
├── config/
│   ├── settings.example.json
│   ├── ui.example.json
│   └── scheduler.example.json
├── logs/
│   ├── app.log
│   ├── scheduler.log
│   ├── tray.log
│   └── error.log
├── data/
│   ├── schedules.json
│   ├── profiles.json
│   └── devices.json
├── backups/
├── docs/
│   └── README.md
├── screenshots/
├── assets/
│   ├── icons/
│   ├── images/
│   └── fonts/
└── runtime/
    ├── temp/
    ├── cache/
    └── locks/
```

`PowerTimer.exe` лежит прямо в корне `dist`.

## Конфиги

В репозитории хранятся только шаблоны:

```text
config/settings.example.json
config/ui.example.json
config/scheduler.example.json
```

При первом запуске приложение автоматически создаёт реальные файлы:

```text
config/settings.json
config/ui.json
config/scheduler.json
```

Эти файлы можно редактировать вручную для локальных настроек. Они не должны попадать в Git.

## Данные

Пользовательские данные хранятся в `data`:

- `data/schedules.json` - правила планировщика.
- `data/profiles.json` - будущие профили.
- `data/devices.json` - устройства Wake-on-LAN.

## Логи

Логи разделены по назначению:

- `logs/app.log` - запуск приложения, настройки и действия пользователя.
- `logs/scheduler.log` - работа scheduler engine.
- `logs/tray.log` - работа системного трея.
- `logs/error.log` - ошибки и исключения.

## Runtime

Временные и служебные файлы находятся в `runtime`:

- `runtime/temp` - временные файлы.
- `runtime/cache` - кеш.
- `runtime/locks/powertimer.lock` - marker single instance.

Постоянные пользовательские данные в `runtime` не пишутся.

## Запуск из исходников

```powershell
cd "D:\Projects\Power Timer\PowerTimer"
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe .\app\main.py
```

## Сборка exe

```powershell
cd "D:\Projects\Power Timer\PowerTimer"
.\build_exe.ps1
```

Готовый файл:

```text
dist\PowerTimer.exe
```

## Важные замечания

- Приложение предназначено для Windows.
- Закрытие окна сворачивает приложение в трей.
- Для некоторых действий пробуждения нужны разрешённые wake timers в Windows и поддержка BIOS/UEFI.
- Wake-on-LAN зависит от настроек сетевого адаптера, BIOS/UEFI и драйвера.
- Scheduler, tray, single instance и логи используют пути относительно папки запуска приложения.
