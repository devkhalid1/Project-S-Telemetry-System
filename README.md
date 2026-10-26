<div align="center">

# Project S Telemetry app

**Live telemetry, run recording and replay for Forza Horizon 6 touge battles.**

[![Download for Windows](https://img.shields.io/badge/Download-for%20Windows-2ea44f?style=for-the-badge&logo=windows&logoColor=white)](Forza Telemetry Setup.exe)

[All releases](../../releases) · [Latest release notes](../../releases/latest) · [Report a problem](../../issues)

</div>

<!--
  Add screenshots here, for example:
  <p align="center"><img src="docs/screenshots/live.png" width="780" alt="Live page"></p>
-->

Built for the **Project S** community (competitive 1v1 touge racing) and time attack. Drive in Forza Horizon 6, and the app records your runs, replays them in 3D, analyses your braking, and compares you with another run or another car.

## Download and install

1. Press **Download for Windows** above (or open [Releases](../../releases/latest) and take the `.exe`).
2. Run the installer. If Windows SmartScreen warns you, choose **More info → Run anyway** (the installer is not code-signed yet).
3. In Forza Horizon 6 open **Settings → HUD and Gameplay → Data Out** and set:

   | Setting | Value |
   | --- | --- |
   | Data Out | **On** |
   | IP Address | **127.0.0.1** |
   | Port | **5300** (the app shows the exact number in *Settings*) |
   | Format | **Car Dash** |

4. Open the app and drive. The **Live** page shows *IN GAME* as soon as data arrives.

The app updates itself: when a new version is published it downloads in the background and asks you to restart.

> If port 5300 is used by another program, the app picks the next free port and tells you which number to type into Forza.
> Using the Microsoft Store version of the game? Windows may block it from sending to this PC; enable the UDP loopback exemption for the game and restart it.

## What it does

**Live**
- Real-time dashboard fed straight from the game on the same PC (UDP, no extra software).
- Data-flow health check (packets per second, delay) and a warning before a run if your connection looks weak.
- Run recorder with a 3-2-1 countdown. Stand on a course start line and a **green light** turns on (under 2 m), a chime plays, and the run **stops and saves by itself** at the finish.
- Stops the recording with an alarm if data is lost, instead of saving a broken run.
- Interactive **Forza Horizon 6 map** with your car moving live, a course list, and a highlighted course panel.

**Runs and replay**
- Run list with times in `mm:ss`, each run assigned to a course.
- **3D replay** with chase, top-down and free cameras, a car model, and a road drawn from your saved course.
- Live telemetry during the replay: slip angle with a drift light, gear, clutch, handbrake, tires coloured by temperature (with a drift effect), and engine power in HP.
- Speed/RPM and throttle/brake graphs that follow the replay clock.
- **Braking analysis**: the driving line is coloured by brake pressure and trail braking (gradual release), with a list of braking zones you can jump to.
- **Compare two runs** in one scene, with a different colour per run.

**Courses and cars**
- Courses drawn like printed touge maps, with total distance, **corner count** and an **elevation profile**.
- Time spent per course, best and average run.
- Dashboard card of the cars you used, the most used one highlighted, and a **per-course performance rating** for each car.

## For developers

```bash
npm install
npm run dev        # web version with the dev tools
npm run build      # production build into dist/
```


The dev server features need the plugin in `tools/vite-plugin-dev-tools.js` registered in `vite.config.js`:

```js
import devTools from './tools/vite-plugin-dev-tools.js'
export default defineConfig({ plugins: [react(), devTools()] })
```

Useful checks:

```bash
node electron/telemetry/selftest.cjs            # test the packet parser
node electron/telemetry/selftest.cjs --listen   # print what the game is sending (close the app first)
```

### Project layout

```
src/        React app (pages, components, services, hooks)
electron/   desktop shell: main process, UDP receiver, auto-updater
tools/      Vite dev-tools plugin
public/     course catalog, car names, FH6 map + calibration
```

More notes: [`TELEMETRY-SETUP.md`](TELEMETRY-SETUP.md) · [`MAP-SETUP.md`](MAP-SETUP.md) · [`UPDATES-SETUP.md`](UPDATES-SETUP.md)

## Releasing a new version

```bash
npm version patch          # 1.0.0 → 1.0.1
git push --follow-tags     # GitHub Actions builds and publishes the installer
```

The workflow in `.github/workflows/release.yml` publishes the installer and `latest.yml`; installed apps pick the update up automatically.
For the **Download** button to point at a stable file name, keep this in `package.json`:

```json
"build": { "nsis": { "artifactName": "ProjectS-Telemetry-Setup.${ext}" } }
```

## Disclaimer

Unofficial community tool. Forza Horizon is a trademark of Microsoft; this project is not affiliated with or endorsed by Microsoft or Playground Games.
