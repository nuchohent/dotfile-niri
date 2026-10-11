# dotfile-niri

Мой рабочий стол: **Niri** (Wayland-композитор) + **DankMaterialShell** (DMS).
Конфиги разложены как пакеты для [GNU Stow](https://www.gnu.org/software/stow/).

Сделано под **niri 26.04 / DMS 1.6 / quickshell 0.3.x**.
Проверено на Arch (CachyOS); в репозитории Solus ровно те же версии,
поэтому конфиги переносятся без правок (см. «Совместимость с Solus»).

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

| Программа | Arch / CachyOS | Solus | Зачем |
|---|---|---|---|
| niri | `niri` | `niri` + `niri-session` | композитор |
| DMS | `dms-shell` | `dankmaterialshell` | оболочка |
| quickshell | `quickshell` | `quickshell` | движок DMS (обязателен) |
| matugen | `matugen` | `matugen` | цвета из обоев (тема `dynamic`) |
| kitty | `kitty` | `kitty` | терминал (Mod+Return) |
| fastfetch | `fastfetch` | `fastfetch` | приветствие в терминале |
| stow | `stow` | `stow` | раскладка конфигов |
| nautilus | `nautilus` | `nautilus` | файловый менеджер (Super+E) |
| flatpak | `flatpak` + FreshGram | `flatpak` + FreshGram | мессенджер (Mod+T) |
| Nightcap | `~/.local/bin/Nightcap-x86_64.AppImage` | то же | бинд затемнения |

DMS запускается как пользовательский сервис systemd: `systemctl --user status dms.service`.

## Установка

### 1. Пакеты

Arch / CachyOS:

```sh
sudo pacman -S --needed niri dms-shell quickshell matugen kitty fastfetch stow
```

Solus:

```sh
sudo eopkg install niri niri-session dankmaterialshell quickshell matugen kitty fastfetch stow
```

> В Solus `niri-session` — отдельный подпакет (файлы сессии для greeter), поэтому его
> ставим явно. Если какой-то пакет «не найден» — включи unstable-репозиторий:
> ```sh
> sudo eopkg add-repo "Solus Unstable" https://cdn.getsol.us/repo/unstable/eopkg-index.xml.xz
> sudo eopkg update-repo
> ```

### 2. Конфиги

```sh
git clone https://github.com/nuchohent/dotfile-niri ~/dotfiles
cd ~/dotfiles
stow niri DankMaterialShell fastfetch wallpapers
```

Если на месте уже есть конфиги — сначала убери их или сделай бэкап
(stow не перезаписывает существующие файлы).

### 3. Опционально

- FreshGram (Mod+T): `flatpak install io.github.snowyfluffy.freshgram`
- Nightcap AppImage положить в `~/.local/bin/` (для AppImage нужен FUSE)
- Обои один раз выбрать в DMS из `~/.local/share/wallpapers/`

### Откат

```sh
cd ~/dotfiles
stow -D niri DankMaterialShell fastfetch wallpapers
```

## Совместимость с Solus

Проверено по официальному репозиторию пакетов Solus — всё нужное есть:

| Пакет | Solus | У меня (CachyOS) |
|---|---|---|
| niri | **26.04** | 26.04 |
| dankmaterialshell | **1.6.3** | 1.6.2 |
| quickshell | **0.3.1** | 0.3.2 |
| matugen | 4.1.0 | 4.1.0 |
| kitty | 0.49.2 | — |
| fastfetch | 2.69.0 | — |
| stow | 2.4.1 | — |

Вывод: **да, заработает.** Версии niri/DMS/quickshell совпадают с теми, под которые
сделаны конфиги. DMS есть в официальном репозитории Solus (`dankmaterialshell`),
niri — тоже (`niri` + подпакет `niri-session`).

На что обратить внимание при переезде:

- **`dms/*.kdl`** DMS генерирует сам под свою версию — на Solus он их
  перезапишет/досоздаст, отдельно переносить не нужно.
- **Мониторы** (`monitors.json` и `output "..."` в `config.kdl`) привязаны к
  железу — если на новом ноуте другие выходы/разрешения, поправь под себя.
- **Nightcap AppImage** — не пакет, а файл; перенеси вручную и убедись, что есть FUSE.
- **FreshGram** — flatpak, ставится отдельно (Solus поддерживает flatpak).
- Клавиатурные раскладки `us`/`ru` задаются в niri/Hypr и от дистрибутива не зависят.

## Мониторы

| Выход | Режим | Масштаб | Позиция | Обои |
|---|---|---|---|---|
| `eDP-1` (BOE, ноутбук) | 1920x1080@60.012 | 1.25 | 0,0 | `wallpaper_upscayl_5x_digital-art-4x.png` |
| `HDMI-A-2` (LG 22MP55) | 1920x1080@60.000 | 1 | 1536,0 | `wallpaper-sasiyama_upscayl_5x_upscayl-standard-4x.png` |

## Клавиатура

Раскладки: `us`, `ru`.

## Заметки

- Файлы `niri/.config/niri/dms/*.kdl` **генерируются самим DMS** и при работе
  перезаписываются. В репозитории они лежат как снимок рабочего состояния.
- Stow **не трогает** этот `dms/` (см. `niri/.stow-local-ignore`) — им владеет DMS,
  stow линкует только `config.kdl`.
- Свои правки в Niri держите в `config.kdl` — DMS-файлы не редактируйте.
- Выбор обоев DMS хранит в `~/.local/state/DankMaterialShell/session.json`
  (не в этом репозитории). После установки выбери обоины из
  `~/.local/share/wallpapers/` в настройках DMS.
- Секретов и токенов в конфигах нет.
