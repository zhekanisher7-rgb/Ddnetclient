# Forja DDNet Neon / Форжа DDNet Neon

Community fork of [DDNet](https://ddnet.org) with **neon glow** menu accents.  
Visuals only — no cheats, aimbots, or gameplay advantages.  
**Not** an official DDNet build.

---

## Downloads / Скачать

**Release:** [v20.1-forja-neon](https://github.com/zhekanisher7-rgb/Ddnetclient/releases/tag/v20.1-forja-neon)  
(Previous: [v20.0-forja-neon](https://github.com/zhekanisher7-rgb/Ddnetclient/releases/tag/v20.0-forja-neon))

| Platform | Asset | Install |
|----------|-------|---------|
| **Windows** | `Forja-DDNet-Neon-win64.zip` | Unzip anywhere → run `DDNet.exe` (keep `data/` next to it). |
| **macOS** | `Forja-DDNet-Neon-macos.dmg` | Open DMG → drag **DDNet** to Applications (or run from the volume). |
| **Linux** | `Forja-DDNet-Neon-linux_x86_64.tar.xz` | `tar -xf … && cd Forja-… && ./DDNet` |

### Windows (EN / RU)
1. Download the `.zip` from the release page.  
2. Extract the folder (e.g. with Explorer or 7-Zip).  
3. Run `DDNet.exe` inside the extracted folder. Do not separate the exe from `data/`.  
1. Скачайте `.zip` со страницы релиза.  
2. Распакуйте архив.  
3. Запустите `DDNet.exe` из папки; рядом должна остаться `data/`.

### macOS (EN / RU)
1. Download the `.dmg`.  
2. Open it and launch **DDNet** (or copy the app to Applications).  
3. If Gatekeeper blocks it: right-click → Open (first run), or allow in System Settings → Privacy & Security.  
1. Скачайте `.dmg`.  
2. Откройте и запустите **DDNet** (или перетащите в Программы).  
3. Если macOS блокирует: ПКМ → Открыть, либо Разрешения в настройках безопасности.

### Linux (EN / RU)
1. Download the `.tar.xz`.  
2. `tar -xf Forja-DDNet-Neon-linux_x86_64.tar.xz && cd DDNet-*` (folder name may vary).  
3. Run `./DDNet` from that directory (`data/` must be beside the binary).  
1. Скачайте `.tar.xz`.  
2. Распакуйте и перейдите в каталог.  
3. Запустите `./DDNet` из этой папки.

CI builds (Windows / macOS / Linux) are produced by [`.github/workflows/release.yml`](.github/workflows/release.yml) on tags `v*` and via **Actions → Release → Run workflow**.

---

## What changed / Что изменилось

| Area | Change |
|------|--------|
| Menu buttons / tabs | Soft multi-pass outer glow (`CUIRect::DrawNeonGlow`) in accent color |
| Button feedback | Soft scale on hover, dim/scale on press, lerped over a few frames |
| List selection | Neon highlight instead of plain white wash |
| Background presets | OLED / Dark / Light / Blue (`fj_bg`) applied across menus |
| Window perimeter | Neon outline along the client screen edge |
| Play / browser panel | Neon glow border around the server browser (“Play”) panel |
| Colors | Theme-aware tab/panel tint (`UpdateColors`) |
| Branding | “Forja Neon” mark on the start menu |
| Settings | Toggle + strength + accent color + background under **Settings → Appearance** |

Glow is **fake neon** (translucent expanding rounded rects), not a real bloom shader — intentional, rebase-friendly.

**RU кратко:** пресеты фона (OLED/Тёмный/Светлый/Синий), цвет и сила свечения, анимация кнопок, неоновая рамка окна и панели Play — всё в **Настройки → Внешний вид**.

---

## Config variables / Переменные

| Console / cfg | Default | Description |
|---------------|---------|-------------|
| `fj_neon` | `1` | Enable neon glow (0/1) |
| `fj_neon_color` | `0xE600FF96` | Accent color (packed HSLA+alpha). Default ≈ `#ff2d2d` red |
| `fj_neon_strength` | `70` | Glow strength 0–100 (buttons, perimeter, Play panel) |
| `fj_bg` | `1` | Background preset: `0` OLED, `1` Dark, `2` Light, `3` Blue |

### Background presets / Пресеты фона

| `fj_bg` | Name | Look |
|---------|------|------|
| `0` | **OLED** | Near-black `#000000` / `#050505` |
| `1` | **Dark** | Dark charcoal (classic Forja Neon) |
| `2` | **Light** | Light gray/white panels, dark readable text |
| `3` | **Blue** | Deep navy / blue-tinted dark |

Example `settings_ddnet.cfg` / launcher `-f` snippet:

```
fj_neon 1
fj_neon_strength 80
fj_neon_color 3858825110
fj_bg 0
```

(`3858825110` = `0xE600FF96` unsigned.)

Also still works with stock `ui_color` for overall interface tint (Dark preset blends with it).

---

## Build / Сборка

On Debian/Ubuntu-like systems:

```bash
sudo apt install build-essential cmake ninja-build libsdl2-dev \
  libfreetype6-dev libglew-dev libcurl4-openssl-dev libpng-dev \
  libogg-dev libopus-dev libopusfile-dev libwavpack-dev \
  libsqlite3-dev libssl-dev libnotify-dev python3 rustc cargo

git clone --recursive https://github.com/zhekanisher7-rgb/Ddnetclient.git
cd Ddnetclient
cmake -Bbuild -GNinja \
  -DAUTOUPDATE=OFF \
  -DVIDEORECORDER=OFF \
  -DVULKAN=OFF \
  -DSERVER=OFF \
  -DTOOLS=OFF \
  -DDEV=ON
cmake --build build
```

**Binary:** `build/DDNet`  
**Data:** run with cwd = `build/` (or the tree that contains `data/`), same as upstream.

Flags used here: client-only, no autoupdater (fork), no FFmpeg video recorder, no Vulkan (optional deps skipped).

Prebuilt packages from CI use `package_default` (zip / dmg / tar.xz) with `AUTOUPDATE=OFF`.

---

## Run / Запуск

```bash
cd build   # or extracted package dir
./DDNet
# or
./DDNet -f /path/to/neon.cfg
./DDNet 'fj_neon 1' 'fj_neon_strength 90' 'fj_bg 0'
```

Smoke-test without a real display:

```bash
xvfb-run -a ./DDNet -s 'quit'   # if xvfb is installed
```

Config still lives in `~/.local/share/ddnet/` (same app name as upstream so settings can be shared).

---

## Point Forja launcher at this binary / Лаунчер

Electron launcher: `/workspace/ddnet-launcher`

1. Prefer path: **`/workspace/ddnet-neon/build/DDNet`** (wired into `paths.js` as first Linux candidate when present).
2. Or set custom binary path in launcher settings to that file.
3. On Play, launcher can still inject `ui_color` via generated cfg; neon vars work alongside:

```
fj_neon 1
fj_neon_color <packed>
fj_neon_strength 70
fj_bg 1
```

cwd for spawn must be the directory containing `data/` next to the binary (`build/`).

---

## Files touched

- `src/engine/shared/config_variables.h` — `fj_neon*`, `fj_bg`
- `src/game/client/ui_rect.{h,cpp}` — `DrawNeonGlow`
- `src/game/client/components/menus.{h,cpp}` — helpers, button anim, themes, window perimeter
- `src/game/client/ui_listbox.cpp` — selected/hot row glow
- `src/game/client/components/menus_start.cpp` — Forja Neon mark
- `src/game/client/components/menus_browser.cpp` — Play/server browser perimeter glow
- `src/game/client/components/menus_settings_appearance.cpp` — settings UI
- `src/game/client/components/menus_settings_graphics.cpp` — quick toggle

---

## Disclaimer

Unofficial community client theme for Melgy / Forja. Protocol-compatible with DDNet servers; do not present as official DDNet.
