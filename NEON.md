# Forja DDNet Neon / Форжа DDNet Neon

Community fork of [DDNet](https://ddnet.org) with **neon glow** menu accents.  
Visuals only — no cheats, aimbots, or gameplay advantages.  
**Not** an official DDNet build.

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

cd /workspace/ddnet-neon   # or your clone
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

---

## Run / Запуск

```bash
cd /workspace/ddnet-neon/build
./DDNet
# or
./DDNet -f /path/to/neon.cfg
./DDNet 'fj_neon 1' 'fj_neon_strength 90'
```

Smoke-test without a real display:

```bash
cd /workspace/ddnet-neon/build
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
