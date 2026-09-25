# Forja DDNet Neon / Форжа DDNet Neon

Community fork of [DDNet](https://ddnet.org) with **neon glow** menu accents.  
Visuals only — no cheats, aimbots, or gameplay advantages.  
**Not** an official DDNet build.

---

## Downloads / Скачать

**Release:** [v20.0-forja-neon](https://github.com/zhekanisher7-rgb/Ddnetclient/releases/tag/v20.0-forja-neon)

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
| List selection | Neon highlight instead of plain white wash |
| Colors | Darker tab/panel tint when neon is on (`UpdateColors`) |
| Branding | “Forja Neon” mark on the start menu |
| Settings | Toggle + strength + accent color under **Settings → Appearance** (also a checkbox near UI Color in Graphics) |

Glow is **fake neon** (translucent expanding rounded rects), not a real bloom shader — intentional, rebase-friendly.

---

## Config variables / Переменные

| Console / cfg | Default | Description |
|---------------|---------|-------------|
| `fj_neon` | `1` | Enable neon glow (0/1) |
| `fj_neon_color` | `0xE600FF96` | Accent color (packed HSLA+alpha). Default ≈ `#ff2d2d` red |
| `fj_neon_strength` | `70` | Glow strength 0–100 |

Example `settings_ddnet.cfg` / launcher `-f` snippet:

```
fj_neon 1
fj_neon_strength 80
fj_neon_color 3858825110
```

(`3858825110` = `0xE600FF96` unsigned.)

Also still works with stock `ui_color` for overall interface tint.

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
./DDNet 'fj_neon 1' 'fj_neon_strength 90'
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
```

cwd for spawn must be the directory containing `data/` next to the binary (`build/`).

---

## Files touched (Phase B)

- `src/engine/shared/config_variables.h` — `fj_neon*`
- `src/game/client/ui_rect.{h,cpp}` — `DrawNeonGlow`
- `src/game/client/components/menus.{h,cpp}` — helpers, button/tab glow, darker `UpdateColors`
- `src/game/client/ui_listbox.cpp` — selected/hot row glow
- `src/game/client/components/menus_start.cpp` — Forja Neon mark
- `src/game/client/components/menus_settings_appearance.cpp` — settings UI
- `src/game/client/components/menus_settings_graphics.cpp` — quick toggle

---

## Disclaimer

Unofficial community client theme for Melgy / Forja. Protocol-compatible with DDNet servers; do not present as official DDNet.
