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
```

## Зависимости

| Программа | Зачем |
|---|---|
| `niri` | сам композитор |
| `dms-shell` (плюс `quickshell`, `qs`) | DankMaterialShell |
| `matugen` | генерация цветов из обоев (тема `dynamic`) |
| `kitty` | терминал |
| `Nightcap-x86_64.AppImage` в `~/.local/bin/` | бинд затемнения |

DMS запускается как пользовательский сервис systemd: `systemctl --user status dms.service`.

## Мониторы

| Выход | Режим | Масштаб | Позиция |
|---|---|---|---|
| `eDP-1` (BOE, ноутбук) | 1920x1080@60.012 | 1.25 | 0,0 |
| `HDMI-A-2` (LG 22MP55) | 1920x1080@60.000 | 1 | 1536,0 |

## Клавиатура

Раскладки: `us`, `ru` (переключение — стандартными средствами Niri).

## Установка

```sh
git clone https://github.com/nuchohent/dotfile-niri ~/dotfiles
cd ~/dotfiles
sudo pacman -S stow        # если нет
stow niri DankMaterialShell
```

Перед первым `stow` уберите существующие конфиги или сделайте их бэкап,
иначе stow откажется перезаписывать (он не трогает уже существующие файлы).

Откат:

```sh
cd ~/dotfiles
stow -D niri DankMaterialShell
```

## Заметки

- Файлы `niri/.config/niri/dms/*.kdl` **генерируются самим DMS** и при работе
  перезаписываются. В репозитории они лежат как снимок рабочего состояния.
- Свои правки в Niri держите в `config.kdl` — DMS-файлы не редактируйте.
- Секретов и токенов в конфигах нет.
