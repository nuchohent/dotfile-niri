# dotfile-niri

Мой рабочий стол: **Niri** (Wayland-композитор) + **DankMaterialShell** (DMS).
Конфиги разложены как пакеты для [GNU Stow](https://www.gnu.org/software/stow/).

## Структура

```
niri/.config/niri/
├── config.kdl            # основной конфиг Niri
└── dms/*.kdl             # генерируемые DMS куски (подключаются через include)

DankMaterialShell/.config/DankMaterialShell/
├── settings.json         # настройки DMS (тема, бар, виджеты)
├── monitors.json         # профили мониторов
└── zen.css               # моя тема

fastfetch/.config/fastfetch/
├── config.jsonc          # конфиг fastfetch
└── sasakiU.png           # логотип для fastfetch

wallpapers/.local/share/wallpapers/
├── wallpaper_upscayl_5x_digital-art-4x.png                # обои для eDP-1
└── wallpaper-sasiyama_upscayl_5x_upscayl-standard-4x.png  # обои для HDMI-A-2
```

## Зависимости

| Программа | Зачем |
|---|---|
| `niri` | сам композитор |
| `dms-shell` (плюс `quickshell`, `qs`) | DankMaterialShell |
| `matugen` | генерация цветов из обоев (тема `dynamic`) |
| `kitty` | терминал |
| `fastfetch` | приветственное окно терминала |
| `Nightcap-x86_64.AppImage` в `~/.local/bin/` | бинд затемнения |

DMS запускается как пользовательский сервис systemd: `systemctl --user status dms.service`.

## Мониторы

| Выход | Режим | Масштаб | Позиция | Обои |
|---|---|---|---|---|
| `eDP-1` (BOE, ноутбук) | 1920x1080@60.012 | 1.25 | 0,0 | `wallpaper_upscayl_5x_digital-art-4x.png` |
| `HDMI-A-2` (LG 22MP55) | 1920x1080@60.000 | 1 | 1536,0 | `wallpaper-sasiyama_upscayl_5x_upscayl-standard-4x.png` |

## Клавиатура

Раскладки: `us`, `ru`.

## Установка

```sh
git clone https://github.com/nuchohent/dotfile-niri ~/dotfiles
cd ~/dotfiles
sudo pacman -S stow fastfetch      # если ещё не установлены
stow niri DankMaterialShell fastfetch wallpapers
```

Перед первым `stow` уберите существующие конфиги или сделайте их бэкап,
иначе stow откажется перезаписывать (он не трогает уже существующие файлы).

Откат:

```sh
cd ~/dotfiles
stow -D niri DankMaterialShell fastfetch wallpapers
```

## Заметки

- Файлы `niri/.config/niri/dms/*.kdl` **генерируются самим DMS** и при работе
  перезаписываются. В репозитории они лежат как снимок рабочего состояния.
- Stow **не трогает** этот `dms/` (см. `niri/.stow-local-ignore`) — им владеет DMS,
  стow линкует только `config.kdl`.
- Свои правки в Niri держите в `config.kdl` — DMS-файлы не редактируйте.
- Выбор обоев DMS хранит в `~/.local/state/DankMaterialShell/session.json`
  (не в этом репозитории). После установки выбери обоины из
  `~/.local/share/wallpapers/` в настройках DMS.
- Секретов и токенов в конфигах нет.
