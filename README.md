# MaestrWave

![version](https://img.shields.io/badge/version-M7s-blue)
![last commit](https://img.shields.io/github/last-commit/ChromiteCr/MaestrWave)
![commit activity](https://img.shields.io/github/commit-activity/m/ChromiteCr/MaestrWave)
![stars](https://img.shields.io/github/stars/ChromiteCr/MaestrWave)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**AI 生成管弦乐素材，然后用身体指挥它。** React + Vite 前端，FastAPI 后端。
**Generate orchestral material with AI, then conduct it with your body.** React + Vite frontend, FastAPI backend.

十几条声部各自独立播放，你的手势实时改变它们的音量、速度和平衡。
Dozens of parts play independently, and your gestures change their volume, speed, and balance in real time.

## 项目定位
## Project Positioning

界面上是两条并列的路径，不分主次。
The interface presents two parallel paths, with neither primary nor secondary.

**指挥教学**：从零学怎么打拍子。七课分三个单元，每课有标准依据、动画示范、
现场写谱渲染的练习曲，打完按维度给分并给出讲评。另有考试页，用固定曲目打分，
分数可以横向比较。
**Conducting tutorial**: learn to beat time from scratch. Seven lessons across three units, each with
standards references, animated demonstrations, and exercises rendered from live notation; after
conducting you get dimension-level scores with commentary. There is also an exam page that scores
fixed pieces, so scores can be compared across students.

**指挥体验**：做一首曲子再指挥它。五步流程是文件、构型、生成、浏览、输出。
前四步产出每件乐器一条独立音轨，最后一步是指挥台。
**Conducting experience**: make a piece, then conduct it. The five-step flow is files, configuration,
generate, browse, output. The first four steps produce one independent track per instrument; the
last step is the conductor's podium.

## 当前能力
## Current Capabilities

- 项目模型组织「项目 → 乐器 → take」的生成流程，支持单乐器生成、重新生成与局部重绘
  - Project model organizes generation as "project → instrument → take", supporting per-instrument generation, regeneration, and local repainting
- 三种生成路线：本机 ACE-Step、腾讯音乐天琴云端 API、AI 写谱加采样器演奏
  - Three generation routes: local ACE-Step, Tencent Music Tianqin cloud API, AI score-writing plus sampler playback
- 摄像头指挥，MediaPipe 在浏览器本地认手，画面不出这台机器
  - Camera conducting; MediaPipe recognizes hands locally in the browser, and video never leaves this machine
- 手机扫码当指挥棒，声音从电脑放出，走局域网或公网隧道都行
  - Use a phone (via QR code) as the baton with audio from the computer, over LAN or a public tunnel
- 七课教学与考试，摄像头采集，六个维度给出具体数字与建议
  - Seven lessons and exams, camera capture, six dimensions with concrete numbers and suggestions
- 管弦乐四族十三件乐器。「打击乐」指定音鼓与铙钹，跟流行鼓组无关
  - Four orchestral families, thirteen instruments. "Percussion" means timpani and cymbals, not a pop drum kit
- ACE-Step 不可用时自动回退到程序化占位音频，便于本地验证
  - Automatically falls back to procedural placeholder audio when ACE-Step is unavailable, for easy local verification

## 一键启动包（GitHub Actions 自动构建）
## One-Click Startup Package (built automatically by GitHub Actions)

通过 `.github/workflows/release-build.yml`，GitHub Actions 会把项目自动打包成
**解压即用、双击启动**的绿色发布包（内置 Python/Node 运行时，无需安装环境、无需命令行）：
Via `.github/workflows/release-build.yml`, GitHub Actions automatically packages the project into an
**unzip-and-run, double-click-to-start** portable release (with Python/Node runtimes bundled — no
environment install, no command line):

- **手动触发**：仓库 Actions 页 → `Build & Publish Startup Package` → Run workflow，
  生成 `MaestrWave-macOS.zip` 与 `MaestrWave-Windows.zip`（可在 Actions 页下载）。
  - **Manual trigger**: repo Actions page → `Build & Publish Startup Package` → Run workflow,
    producing `MaestrWave-macOS.zip` and `MaestrWave-Windows.zip` (downloadable from the Actions page).
- **打 tag 自动发布**：推送 `v*` 标签（如 `v1.0.0`）时自动构建并发布到 GitHub Releases。
  - **Auto-publish on tag**: pushing a `v*` tag (e.g. `v1.0.0`) triggers a build and publishes to GitHub Releases.

发布包结构：
Release package layout:

```
MaestrWave-{平台}.zip
├── MaestrWave/                  # PyInstaller 打包的后端（FastAPI + uvicorn）
├── frontend/dist/               # 前端构建产物（vite build）
├── output/                      # 运行时数据（生成的项目与音频）
├── config.env                   # 可选配置（TME 密钥 / 端口）
├── Start-MaestrWave.command(.bat)  # 双击启动，自动打开浏览器
└── README.txt
```

使用方式：解压 → 双击 `Start-MaestrWave.command`（macOS）/ `Start-MaestrWave.bat`
（Windows）→ 浏览器自动打开 `http://localhost:3000`。
Usage: unzip → double-click `Start-MaestrWave.command` (macOS) / `Start-MaestrWave.bat`
(Windows) → the browser opens `http://localhost:3000` automatically.

音频生成后端在发布包里默认使用占位音频（演示链路完整可用）；如需腾讯天琴云端真实
生成，用文本编辑器在 `config.env` 填入 `TME_APP_ID` / `TME_APP_KEY` 并将
`GENERATION_BACKEND` 设为 `tme` 即可，同样不需要命令行。若仓库为私有且信任下载者，
也可在手动触发时勾选 `tme_inject`，由 Actions 从仓库 Secrets 注入密钥。
The audio generation backend in the release package defaults to placeholder audio (the full demo
chain works out of the box). For real Tencent Tianqin cloud generation, edit `config.env` in a text
editor, fill in `TME_APP_ID` / `TME_APP_KEY`, and set `GENERATION_BACKEND` to `tme` — still no
command line needed. If the repo is private and you trust the downloaders, you can also check
`tme_inject` on manual trigger and let Actions inject the keys from repo Secrets.

> 实现要点：后端打包由 `scripts/package/entry.py` + `maestrwave.spec` 完成，前端
> dist 在入口脚本中显式重定向给 `backend.app`（不改动任何业务代码）；CI 里带一次
> 真启动冒烟测试（HTTP 200），确保发布包开箱即用。
> Implementation notes: the backend is packaged by `scripts/package/entry.py` + `maestrwave.spec`,
> and the frontend dist is explicitly redirected to `backend.app` in the entry script (no business
> code changes); CI runs a real-startup smoke test (HTTP 200) to ensure the package works out of the box.

## 版本记录
## Changelog

当前版本 **M7s**。完整的变更历史、版本号规则，以及 M0 到 M6 的阶段记录，都在
[docs/CHANGELOG.md](docs/CHANGELOG.md)。
Current version **M7s**. The full change history, version numbering rules, and the M0–M6 phase
records are in [docs/CHANGELOG.md](docs/CHANGELOG.md).

## 用云端 API 生成（不占本机显存）
## Generate with the cloud API (no local VRAM)

ACE-Step 要在本机跑模型，消费级设备显存吃紧。可以改用腾讯音乐天琴的云端 API：
ACE-Step runs the model locally, and consumer hardware is short on VRAM. You can use Tencent
Music Tianqin's cloud API instead:

```bash
export GENERATION_BACKEND=tme
export TME_APP_ID=你的AppId
export TME_APP_KEY=你的AppKey
uvicorn app:app --host 0.0.0.0 --port 3000
```

密钥只从环境变量读，不写进仓库。配好后「设置」页会显示当前后端、是否就绪，以及能力差异。
Keys are read only from environment variables and never committed to the repo. Once configured, the
"Settings" page shows the current backend, whether it's ready, and the capability differences.

生成时会把「生成」页里的**全部音乐信息**编进请求的 tags，包括正在生成哪件乐器（含自定义乐器名）、它承担的角色、调号、拍号、BPM、片段时长、风格描述，以及已有哪些声部。实际发出的 tags 会打进后端日志，方便核对。
At generation time, **all musical information** from the "Generate" page is encoded into the request
tags — which instrument is being generated (including custom names), its role, key signature, time
signature, BPM, clip length, style description, and which parts already exist. The tags actually sent
are logged in the backend for verification.

### 能力差异（重要）
### Capability differences (important)

天琴是**整曲生成器**，只有文生乐，没有 ACE-Step 的两项能力：
Tianqin is a **whole-piece generator** — text-to-music only, without two of ACE-Step's capabilities:

| | 本机 ACE-Step | 天琴（云端） |
|---|---|---|
| 显存占用 | 本机 16GB+ | 无 |
| 文生乐 | ✅ | ✅ |
| 音频层面协同（lego） | ✅ 真的"听"已有音轨来加声部 | ❌ 降级为共享调号/拍号/速度的文字对齐 |
| 局部重绘（Repaint） | ✅ | ❌ 按钮会自动隐藏 |
| LoKr / LoRA 权重 | ✅ | ❌ |

| | Local ACE-Step | Tianqin (cloud) |
|---|---|---|
| VRAM usage | Local 16GB+ | None |
| Text-to-music | ✅ | ✅ |
| Audio-level collaboration (lego) | ✅ actually "listens" to existing tracks to add parts | ❌ degraded to text alignment on shared key/time/tempo |
| Local repaint | ✅ | ❌ button auto-hides |
| LoKr / LoRA weights | ✅ | ❌ |

也就是说，用天琴时**各声部之间的配合会明显弱于 ACE-Step**。它们各自独立生成，只靠同一套乐理参数和文字描述对齐，而 `lego` 是在音频内容层面协同的。想要最好的配合效果，仍然建议用 ACE-Step（本机或 AutoDL 等租用的 GPU 机器）。
In other words, with Tianqin the **coordination between parts is noticeably weaker than ACE-Step**.
Each part is generated independently, aligned only by the same music-theory parameters and text
descriptions, whereas `lego` collaborates at the audio-content level. For the best ensemble, ACE-Step
(local or rented GPU machines like AutoDL) is still recommended.

天琴返回的是完整曲子的 MP3，后端会用 ffmpeg 转成单声道 WAV 并裁到项目的「乐曲总时长」，保证多轨能对齐循环播放。没装 ffmpeg 时会告警并保留原始音频（浏览器仍能播，但 Python 侧的混音会跳过这条轨）。
Tianqin returns an MP3 of the whole piece; the backend converts it to mono WAV with ffmpeg and trims
it to the project's "total piece duration", so multiple tracks align and loop correctly. If ffmpeg is
missing it warns and keeps the original audio (the browser can still play it, but Python-side mixing
skips this track).

## 写谱演奏模式（M7）
## Score-playback mode (M7)

前两种模式都是**直接产音频**，问题在于这类模型输出的是频谱不是音符：说了 92 BPM 出来的未必真是 92 BPM，多件乐器各生成一次就更对不齐，而这个软件恰好需要「每件乐器一条独立音轨 + 精确的拍网格 + 乐器能按段落进出场」。
The first two modes both **produce audio directly**. The problem is that such models output spectra,
not notes: ask for 92 BPM and you may not get exactly 92 BPM, and generating each instrument
separately aligns even worse — yet this software needs "one independent track per instrument + a
precise beat grid + instruments that enter and exit by section".

第三种模式换个路子：**AI 先写谱，再由采样器演奏**。产物仍然是每件乐器一个 WAV，所以浏览页、输出页、摄像头指挥一行都没改。新建项目时选「写谱演奏」即可。
The third mode takes a different route: **AI writes the score first, then a sampler plays it**. The
output is still one WAV per instrument, so the browse page, output page, and camera conducting all
stay unchanged. Just choose "score playback" when creating a project.

它顺带解决了几件旧问题：各声部长度精确一致、循环无接缝（尾音会叠回开头）、构型里的 `participation` 第一次真正生效（不参与的小节根本不写音符）、Repaint 真的能用（重写那几小节的音符再整轨重渲染）。谱子可以导出成 `.mid` 拿去 MuseScore 或 DAW 里用。
It also solves several old problems: all parts have exactly equal length; loops are seamless (the
tail overlaps the head); `participation` in the configuration finally works (measures that don't
participate get no notes at all); Repaint genuinely works (rewrite those measures' notes, then
re-render the whole track). The score can be exported as `.mid` for MuseScore or a DAW.

### 作曲器
### Composer

| 值 | 说明 |
|---|---|
| `algorithmic` | 纯规则，不联网、不吃显存。调性统一、声部进行平稳、音区不重叠 |
| `llm` | 走已有的 BYOK 通路。任何一步失败都退回 `algorithmic`，并把降级原因记进 take |
| `remote` | 外部符号音乐模型服务。**失败不退回规则作曲**，直接报错。选了模型却拿规则的结果糊弄过去，你就再也判断不了这个模型行不行 |

| Value | Description |
|---|---|
| `algorithmic` | Pure rules, no network, no VRAM. Unified tonality, smooth voice leading, no register overlap |
| `llm` | Uses the existing BYOK path. Any step failing falls back to `algorithmic`, and the fallback reason is recorded in the take |
| `remote` | External symbolic music model service. **On failure it does NOT fall back to rules** — it errors out. If you pick a model but silently get rule-based results, you can never judge whether the model works |

默认 `auto`：配了语言模型就用 `llm`，否则 `algorithmic`。三者都在「设置」页可切，选择存后端偏好文件。
Default is `auto`: use `llm` if a language model is configured, otherwise `algorithmic`. All three are
switchable on the "Settings" page, and the choice is stored in a backend preferences file.

`remote` 的接口契约见 **[docs/SYMBOLIC_COMPOSER_API.md](docs/SYMBOLIC_COMPOSER_API.md)**，一个
`POST /compose_part`，请求给蓝图、乐器音域与已写好的其它声部，响应回一张音符表。任何符号
音乐模型（Anticipatory Music Transformer、MMM 之类）自己包一层薄服务就能接进来，后端零改动。
The `remote` interface contract is in **[docs/SYMBOLIC_COMPOSER_API.md](docs/SYMBOLIC_COMPOSER_API.md)**:
one `POST /compose_part` — the request gives a blueprint, the instrument's range, and the other parts
already written; the response returns a note table. Any symbolic music model (Anticipatory Music
Transformer, MMM, etc.) can be connected with a thin wrapper service, zero backend changes.

和弦走向与段落结构一律本地算：符号模型擅长续写音符，不擅长按段落强度规划全曲结构，而后者
恰好是纯规则做得又快又稳的部分。
Chord progressions and section structure are always computed locally: symbolic models are good at
continuing notes, not at planning whole-piece structure by section intensity — which is exactly what
pure rules do fast and reliably.

服务地址在「设置」页填，**只接受本机或局域网地址**。这条限制的对象是隧道：`tunnel.py` 一开，
后端就暴露在公网，而这个地址决定了后端把整份乐谱 POST 到哪去。要连公网服务请用
`SYMBOLIC_COMPOSER_URL` 环境变量启动。那是启动时的运维决定，远程改不了它。
The service address is entered on the "Settings" page and **accepts only localhost or LAN addresses**.
This restriction targets the tunnel: once `tunnel.py` is on, the backend is exposed to the public
internet, and this address determines where the backend POSTs the entire score. To connect a public
service, start with the `SYMBOLIC_COMPOSER_URL` environment variable. That's an ops decision made at
startup, and it can't be changed remotely.

```bash
export SCORE_COMPOSER=algorithmic   # 强制用本地规则，一次外部调用都不发
```

### 音源
### Sound sources

**开箱即用，不需要装任何东西。** 仓库自带一个 33MB 的管弦乐音源
（`backend/soundfonts/orchestral.sf2`，从 MIT 授权的 FluidR3_GM 裁出来的），
由 `backend/sf2.py` 直接读取播放。那是个纯标准库的 SoundFont 解析器加采样
播放器，没有任何外部可执行文件，源码跑和打包跑完全一致。
**Works out of the box, nothing to install.** The repo bundles a 33MB orchestral soundfont
(`backend/soundfonts/orchestral.sf2`, trimmed from the MIT-licensed FluidR3_GM), read and played
directly by `backend/sf2.py`. It's a pure-stdlib SoundFont parser plus sample player — no external
executables, so source and packaged runs behave identically.

| 音源 | 说明 |
|---|---|
| `sf2`（默认） | 自带的采样播放器，真实乐器录音 |
| `fluidsynth` | 外部合成器，同一份音源，混响更丰富。装了才能选 |
| `builtin` | 纯加法合成，连音源文件都不需要。音色朴素，是最后的保底 |

| Sound source | Description |
|---|---|
| `sf2` (default) | Bundled sample player, real instrument recordings |
| `fluidsynth` | External synthesizer, same soundfont, richer reverb. Selectable only if installed |
| `builtin` | Pure additive synthesis, needs no soundfont file at all. Plain timbre, the last-resort fallback |

在「设置」页可以随时切换，也可以用 `SCORE_RENDERER` 环境变量指定。
`/api/health` 的 `score` 块会如实报告当前实际生效的是哪个。
You can switch at any time on the "Settings" page, or set it via the `SCORE_RENDERER` environment
variable. The `score` block of `/api/health` honestly reports which one is actually in effect.

换自己的音源：把任意 `.sf2` 放进 `backend/soundfonts/` 即可（文件名随意，后端
自己扫）。**只支持未压缩的 `.sf2`**，`.sf3` 的样本是 Ogg Vorbis 压缩的，
纯 Python 解不了。想换个更大的音源再裁一遍：
To use your own soundfont: drop any `.sf2` into `backend/soundfonts/` (any filename; the backend
scans it itself). **Only uncompressed `.sf2` is supported** — `.sf3` samples are Ogg Vorbis-compressed
and can't be decoded in pure Python. To swap in a bigger soundfont and re-trim:

```bash
python3 scripts/trim_soundfont.py 完整音源.sf2 backend/soundfonts/orchestral.sf2
```

发布包里连 fluidsynth 一起带（macOS 实测 19 个文件 5.7MB，CI 会把库路径改写成
`@loader_path` 相对引用，到没装 Homebrew 的机器上也能跑），所以双击启动的用户
三种音源都能选。
The release package bundles fluidsynth too (on macOS, 19 files / 5.7MB measured; CI rewrites library
paths to `@loader_path` relative references so it runs on machines without Homebrew), so double-click
users can choose any of the three sound sources.

**打击乐限定为管弦乐编制**：大鼓、小军鼓、吊镲、三角铁，没有踩镲和嗵鼓，那是爵士鼓组的东西。定音鼓是有音高的乐器，走普通通道写主音／属音，不当鼓组音效。
**Percussion is limited to the orchestral lineup**: bass drum, snare drum, suspended cymbal,
triangle — no hi-hat or toms, which belong to a jazz drum kit. Timpani is a pitched instrument,
written on a normal channel as tonic/dominant, not as drum-kit effects.

## 代码结构
## Code structure

- backend
  - app.py：FastAPI 入口与 REST API
  - app.py: FastAPI entry point and REST API
  - generator.py：ACE-Step 客户端与生成任务封装
  - generator.py: ACE-Step client and generation-task wrapper
  - project.py：项目/乐器/take 数据模型
  - project.py: project/instrument/take data model
  - project_gen.py：分乐器协同生成与 lego 编排逻辑
  - project_gen.py: per-instrument collaborative generation and lego orchestration
  - generation_backend.py：生成后端抽象与能力声明
  - generation_backend.py: generation-backend abstraction and capability declarations
  - tme_backend.py：腾讯音乐天琴云端生成后端
  - tme_backend.py: Tencent Music Tianqin cloud generation backend
  - conduct.py：手机遥控指挥的 WebSocket 中转
  - conduct.py: WebSocket relay for phone-remote conducting
  - netinfo.py：局域网地址探测
  - netinfo.py: LAN address discovery
  - tunnel.py：代管 cloudflared 隧道进程（UI 一键启停）
  - tunnel.py: manages the cloudflared tunnel process (one-click start/stop in the UI)
  - synth.py：程序化音频 fallback
  - synth.py: procedural audio fallback
  - agent.py：对话式 Agent 的上下文拼装（读 docs/USER_GUIDE.md，课程知识由前端传）
  - agent.py: context assembly for the conversational Agent (reads docs/USER_GUIDE.md; lesson knowledge comes from the frontend)
  - config.py：配置项与乐器目录
  - config.py: configuration and the instrument catalog
- frontend
  - src/App.tsx：页面路由与侧栏入口
  - src/App.tsx: page routing and sidebar entry
  - src/pages：「指挥体验」下的文件、构型、生成、浏览、输出五页，「指挥教学」下的 TeachPage，以及不属于任何一级的训练、设置；外加手机端遥控页 RemotePage
  - src/pages: the five "Conducting experience" pages (files, configuration, generate, browse, output), TeachPage under "Conducting tutorial", plus training and settings (which belong to no top level); and the phone remote page RemotePage
  - src/components：波形、侧栏（两级导航）、右侧 Agent 面板、情绪柱状图、摄像头预览、图形拍型示范、讲评报告、二维码等 UI 组件
  - src/components: UI components — waveform, sidebar (two-level nav), right Agent panel, emotion bar chart, camera preview, graphic beat-pattern demo, review report, QR code, etc.
  - src/lib：音频引擎、传感器源、手势解析、摄像头指挥与 DTW 拍型识别（camera/）、教学（teaching/：课程数据、图形拍型、节拍器、录制层、六维评分）、指挥链路（conductLink）与 API 封装
  - src/lib: audio engine, sensor sources, gesture parsing, camera conducting and DTW beat-pattern recognition (camera/), teaching (teaching/: lesson data, graphic beat patterns, metronome, recording layer, six-dimension scoring), the conduct link (conductLink), and API wrappers
  - src/styles/global.css：设计 token（配色、三个字体 token）；canvas 里的字体经 src/lib/canvasFont.ts 读同一份 token
  - src/styles/global.css: design tokens (color palette, three font tokens); canvas fonts read the same tokens via src/lib/canvasFont.ts
- docs
  - CHANGELOG.md：完整版本记录
  - CHANGELOG.md: full version history
  - USER_GUIDE.md：用户使用指南，同时是应用内助手的知识来源
  - USER_GUIDE.md: user guide, also the in-app assistant's knowledge source
  - SYMBOLIC_COMPOSER_API.md：外部符号音乐模型的接入契约
  - SYMBOLIC_COMPOSER_API.md: integration contract for external symbolic music models
  - M6_PLAN.md：M6（两级导航 + 指挥教学）的实施计划与进度
  - M6_PLAN.md: implementation plan and progress for M6 (two-level nav + conducting tutorial)
- scripts
  - dev-certs.sh：生成开发用 HTTPS 证书（iOS 传感器权限需要）
  - dev-certs.sh: generates dev HTTPS certificates (needed for iOS sensor permissions)
- output
  - projects：项目化生成产物
  - projects: project-based generation artifacts
  - sessions：旧版 legacy 生成产物
  - sessions: legacy generation artifacts

## 快速开始
## Quick start

### 1. 后端
### 1. Backend

要求：Python 3.10+，建议在虚拟环境中运行。
Requires Python 3.10+, recommended to run in a virtual environment.

```bash
cd backend
python -m pip install -r requirements.txt
uvicorn app:app --host 0.0.0.0 --port 3000
```

也可以在项目根目录以包模式启动：
You can also start in package mode from the repo root:

```bash
python -m pip install -r backend/requirements.txt
uvicorn backend.app:app --host 0.0.0.0 --port 3000
```

### 2. 前端
### 2. Frontend

```bash
cd frontend
npm install
npm run dev
```

随后访问 Vite 提供的地址即可看到界面。默认开发环境会把 /api、/audio、/project-audio 代理到后端。
Then open the address Vite provides to see the UI. The default dev environment proxies /api, /audio,
and /project-audio to the backend.

### 3. 生成与演绎
### 3. Generation and performance

1. 打开「文件」页，新建项目，填项目名称与乐曲总时长，选生成方式。
   Open the "Files" page, create a project, fill in the name and total piece duration, and choose a generation method.
2. 在「构型」页定段落结构与情绪曲线，增删乐器并给每件定声部。
   On the "Configuration" page, set the section structure and emotion curve, add/remove instruments and assign each a part.
3. 在「生成」页逐件乐器生成音频，同一件可以生成多条 take 再挑一条。
   On the "Generate" page, generate audio per instrument; you can generate multiple takes for one instrument and pick one.
4. 在「浏览」页试听与检查，波形排在一起，可以单独播也可以一起播。
   On the "Browse" page, audition and inspect; waveforms are laid out together and can be played solo or together.
5. 在「输出」页点「开始指挥」。默认用摄像头，想用手机就先去「设置」页换成电脑模式，
   具体见下面「手机指挥」。
   On the "Output" page, click "Start conducting". Camera is the default; to use a phone, switch to
   computer mode on the "Settings" page first — see "Phone conducting" below.

## 手机指挥
## Phone conducting

用什么设备打拍子在「设置」页选（设置 → 指挥 → 指挥方式），输出页上没有这个开关。
Which device you conduct with is chosen on the "Settings" page (Settings → Conducting → Conducting
method); the output page has no such switch.

两种方式解析出来的是同一种「指挥意图」，声部逻辑、力度与速度的算法完全一样，
差别只在手势从哪儿采集。
Both methods resolve to the same "conducting intent" — the part logic, dynamics, and tempo
algorithms are identical; the only difference is where gestures are captured.

| 方式 | 谁采手势 | 谁出声 | 需要什么 |
|------|---------|--------|----------|
| 摄像头模式（默认） | 这台电脑的摄像头 | 这台电脑 | 安全上下文，用 localhost 或 HTTPS 访问 |
| 电脑模式 | 手机 | 这台电脑 | 手机和电脑能互相连上 |

| Method | Who captures gestures | Who produces sound | What's needed |
|------|---------|--------|----------|
| Camera mode (default) | This computer's camera | This computer | A secure context — access via localhost or HTTPS |
| Computer mode | The phone | This computer | The phone and computer can reach each other |

电脑模式的流程：设置页切到电脑模式 → 输出页显示房间码和二维码 → 手机扫码进入遥控界面
→ 电脑点「开始指挥」→ 挥动手机，声音从电脑放出。手机端只发传感器数据，不下载音频。
Computer-mode flow: switch to computer mode on the settings page → the output page shows a room
code and QR code → scan with the phone to open the remote UI → click "Start conducting" on the
computer → wave the phone, and sound comes out of the computer. The phone only sends sensor data;
it doesn't download audio.

手机怎么连过来有两种方式，在「输出」页可以切换。**局域网**延迟最低，iPhone 需装一次证书。
**隧道**零安装，也不要求同一网络。
There are two ways for the phone to connect, switchable on the "Output" page. **LAN** has the lowest
latency, but iPhone needs a one-time certificate. **Tunnel** needs no install and no shared network.

> 早先还有一个「单机模式」，手机自采自放。它已在 M7n 删掉：它和电脑模式做的是同一件事，
> 差别只在声音从哪儿出来，而手机扬声器放十四轨管弦乐本来就不该是这个软件的样子。
> There used to be a "standalone mode" where the phone captured and played by itself. It was removed
> in M7n: it did the same thing as computer mode, differing only in where the sound comes out — and a
> phone speaker playing fourteen orchestral tracks was never what this software should be.

### 方式一：局域网，iOS 必须先启用 HTTPS
### Method 1: LAN, iOS must enable HTTPS first

iOS 只在安全上下文里才允许 `DeviceMotionEvent.requestPermission()`。手机通过局域网 IP 走 http:// 访问时，Safari 连权限弹窗都不会出现。生成证书：
iOS only allows `DeviceMotionEvent.requestPermission()` in a secure context. When the phone visits
http:// via the LAN IP, Safari won't even show the permission prompt. Generate a certificate:

```bash
bash scripts/dev-certs.sh
```

脚本优先用 mkcert（推荐 `brew install mkcert`，签出的证书受系统信任），没有则用 openssl 自签名兜底。证书写到 `frontend/certs/`。
The script prefers mkcert (recommended: `brew install mkcert`; its certificates are trusted by the
system), falling back to an openssl self-signed cert. Certificates are written to `frontend/certs/`.

证书生成后 HTTPS **不会自动启用**。要用 HTTPS 启动（注意在 `frontend/` 目录下）：
HTTPS is **not auto-enabled** after generating the certificate. To start with HTTPS (note: from the
`frontend/` directory):

```bash
cd frontend && npm run dev:https
```

> 换了 Wi-Fi 之后局域网 IP 会变，而证书里签的是生成时那一刻的 IP，手机会因证书不匹配连不上。重跑一次 `scripts/dev-certs.sh` 再重启即可（根证书不用在手机上重装）。「输出」页的局域网模式会自动检测这种情况并给出可复制的命令。
> After changing Wi-Fi, the LAN IP changes, but the certificate signs the IP from the moment of
> generation, so the phone can't connect due to a cert mismatch. Re-run `scripts/dev-certs.sh` and
> restart (no need to reinstall the root cert on the phone). LAN mode on the "Output" page detects
> this automatically and offers a copyable command.

日常桌面开发继续用 `npm run dev`（HTTP）。这样区分是因为：如果「有证书就自动切 HTTPS」，所有 `http://localhost:5173` 的旧地址会静默失效，Safari 只报一句「服务器意外中断了连接」，很难联想到是协议变了。
Everyday desktop development keeps using `npm run dev` (HTTP). The distinction exists because if we
"auto-switch to HTTPS when a cert exists", every old `http://localhost:5173` address would silently
break, and Safari only says "the server unexpectedly dropped the connection" — hard to connect that
to a protocol change.

用 mkcert 时手机还需要安装一次它的根证书（脚本会打印步骤）；用自签名证书则手机首次访问时点「继续访问」即可。
With mkcert, the phone also needs to install its root cert once (the script prints the steps); with a
self-signed cert, just tap "Continue" on first visit.

> 每台要连的 iPhone 都得装一次 mkcert 根证书，它是只有你自己设备信任的私有 CA。
> 要给多个人或多台手机演示的话，下面的隧道方案更合适，公开受信任证书，任何设备零安装。
> Android 上 Chrome 通常不强制 HTTPS，直接用 HTTP 就能拿到传感器数据。
> Every iPhone you want to connect must install the mkcert root cert once — it's a private CA trusted
> only by your own devices. To demo to multiple people or phones, the tunnel approach below is better:
> publicly trusted certs, zero install on any device. On Android, Chrome usually doesn't require HTTPS,
> so plain HTTP already gives you sensor data.

### 方式二：隧道（不用证书，任何手机零安装）
### Method 2: Tunnel (no certificate, zero install on any phone)

「输出」页电脑模式里可以把配对方式从「局域网」切到「隧道」，页面会给出完整步骤。原理是用 cloudflared / ngrok 把本机暴露到公网，它们自带**公开受信任**的 HTTPS 证书，所以任何手机都不用装任何东西，也不要求和电脑在同一个网络。
On the "Output" page in computer mode, you can switch the pairing method from "LAN" to "Tunnel"; the
page gives the full steps. The idea is to expose this machine to the public internet via cloudflared /
ngrok, which come with **publicly trusted** HTTPS certs — so no phone needs to install anything, and it
doesn't need to be on the same network as the computer.

前提是装了 cloudflared，并且 dev server 用 `dev:tunnel` 启动（原因见下）：
Prerequisites: cloudflared installed, and the dev server started with `dev:tunnel` (reason below):

```bash
brew install cloudflared
cd frontend && npm run dev:tunnel
```

然后在「输出」页 → 电脑模式 → 隧道，**点「启动隧道」**即可：后端会代管 cloudflared 进程，拿到域名后自动填进输入框、二维码同步刷新，不需要你另开终端。再点一次「停止隧道」关闭。
Then on the "Output" page → computer mode → tunnel, just **click "Start tunnel"**: the backend manages
the cloudflared process, fills the domain into the input and refreshes the QR code automatically once
it gets the domain — no separate terminal needed. Click "Stop tunnel" again to close.

走隧道时**不需要 mkcert 证书，也不需要 `dev:https`**，TLS 由隧道那一端终结。
Over the tunnel you need **neither the mkcert cert nor `dev:https`** — TLS is terminated at the tunnel
end.

其它情况：
Other cases:

- 没装 cloudflared 时，UI 会显示安装提示和可复制的手动命令，跑完把网址粘进输入框也一样能用（地址存在 localStorage，刷新不丢）。
  - If cloudflared isn't installed, the UI shows an install hint and a copyable manual command; paste the resulting URL into the input and it works the same (the address is stored in localStorage and survives refresh).
- 直接用隧道域名在电脑上打开本页面时，UI 会自动识别并预填。
  - If you open this page on the computer directly via the tunnel domain, the UI recognizes it and pre-fills automatically.
- 后端进程退出时会自动关掉它启动的隧道，避免机器在你不知情的情况下一直暴露在公网。
  - When the backend process exits it closes the tunnel it started, so the machine isn't left exposed to the public internet without your knowledge.

**为什么必须用 `dev:tunnel`**：Vite 会拒绝 Host 头不在允许列表里的请求（防 DNS 重绑定攻击），而 cloudflared 的临时隧道每次启动都是随机域名，没法提前写进配置，成了鸡生蛋。`MW_TUNNEL=1`（即 `dev:tunnel`）按**后缀**放行 `.trycloudflare.com`、`.ngrok-free.app`、`.ngrok.io`、`.ngrok.app`、`.loca.lt`，既不用提前知道具体域名，也保留了对其它域名的防护。
**Why `dev:tunnel` is required**: Vite rejects requests whose Host header isn't in the allow list (DNS
rebinding protection), but cloudflared's ephemeral tunnels get a random domain each launch that can't
be written into config in advance — a chicken-and-egg problem. `MW_TUNNEL=1` (i.e. `dev:tunnel`) allows
**by suffix** `.trycloudflare.com`, `.ngrok-free.app`, `.ngrok.io`, `.ngrok.app`, `.loca.lt` — no need
to know the specific domain ahead of time, while still protecting against other domains.

自建隧道或自有域名用 `MW_ALLOWED_HOSTS=a.example.com,b.example.com npm run dev`；`MW_ALLOWED_HOSTS=*` 可以放行一切，但会关掉 DNS 重绑定防护，只在临时演示时用。
For a self-hosted tunnel or your own domain, use `MW_ALLOWED_HOSTS=a.example.com,b.example.com npm run
dev`; `MW_ALLOWED_HOSTS=*` allows everything but disables DNS rebinding protection — use it only for
temporary demos.

> ⚠️ 隧道地址是**公网可访问**的，拿到链接的人都能接进房间指挥。房间码是 6 位随机码（约 8.8 亿组合，暴力猜不现实），但链接本身别乱发，演示完记得关掉隧道。
> ⚠️ The tunnel address is **publicly accessible** — anyone with the link can join the room and conduct.
> The room code is a 6-digit random code (~880 million combinations, brute force is impractical), but
> don't share the link carelessly, and remember to close the tunnel after the demo.

### 两种方式怎么选
### Choosing between the two methods

| | 局域网 | 隧道 |
|---|---|---|
| 延迟 | 最低 | 多几十毫秒 |
| iPhone 要装证书 | 是（每台一次） | 否 |
| 要求同一 Wi-Fi | 是 | 否 |
| 适合 | 自己开发调试 | 给别人演示、多台手机 |

| | LAN | Tunnel |
|---|---|---|
| Latency | Lowest | A few dozen ms more |
| iPhone needs cert | Yes (once per phone) | No |
| Requires same Wi-Fi | Yes | No |
| Best for | Your own dev/debug | Demos to others, multiple phones |

## 配置说明
## Configuration

后端配置主要在 [backend/config.py](backend/config.py) 中，支持通过环境变量覆盖：
Backend configuration lives mainly in [backend/config.py](backend/config.py), and can be overridden
via environment variables:

- GENERATION_BACKEND：生成后端，local（默认，本机 ACE-Step）/ tme（腾讯音乐天琴云端）/ cloud（预留）
  - GENERATION_BACKEND: generation backend — local (default, on-device ACE-Step) / tme (Tencent Music Tianqin cloud) / cloud (reserved)
- ACESTEP_API_URL：ACE-Step 服务地址，默认 http://localhost:8001
  - ACESTEP_API_URL: ACE-Step service address, default http://localhost:8001
- TME_APP_ID / TME_APP_KEY：天琴的 AppId 与密钥，用 tme 后端时必填
  - TME_APP_ID / TME_APP_KEY: Tianqin AppId and key, required when using the tme backend
- TME_API_URL：天琴接口地址，默认测试环境
  - TME_API_URL: Tianqin API address, default test environment
- PROJECTS_DIR：项目产物目录，默认 output/projects
  - PROJECTS_DIR: project artifact directory, default output/projects
- OUTPUT_DIR：legacy 产物目录，默认 output/sessions
  - OUTPUT_DIR: legacy artifact directory, default output/sessions
- LOKR_WEIGHTS_DIR：LoKr / LoRA 权重目录
  - LOKR_WEIGHTS_DIR: LoKr / LoRA weights directory
- ALLOW_SYNTH_FALLBACK：是否启用程序化占位音频 fallback
  - ALLOW_SYNTH_FALLBACK: whether to enable the procedural placeholder-audio fallback

## 主要 API
## Main APIs

当前主流程以 project API 为主：
The current main flow centers on the project API:

- GET /api/health：检查后端与 ACE-Step 可达性
  - GET /api/health: check backend and ACE-Step reachability
- GET /api/network-info：局域网地址，「输出」页用它生成手机扫码地址
  - GET /api/network-info: LAN address, used by the "Output" page to build the phone QR-code URL
- WS /ws/conduct/{room_id}?role=stage|remote：手机遥控指挥的中转通道
  - WS /ws/conduct/{room_id}?role=stage|remote: relay channel for phone-remote conducting
- GET /api/tunnel、POST /api/tunnel/start、POST /api/tunnel/stop：cloudflared 隧道启停与状态
  - GET /api/tunnel, POST /api/tunnel/start, POST /api/tunnel/stop: cloudflared tunnel start/stop/status
- GET /api/lokr：列出可用 LoKr / LoRA 权重
  - GET /api/lokr: list available LoKr / LoRA weights
- POST /api/projects：创建项目
  - POST /api/projects: create a project
- GET /api/projects：列出项目
  - GET /api/projects: list projects
- POST /api/projects/{project_id}/instruments：新增乐器
  - POST /api/projects/{project_id}/instruments: add an instrument
- POST /api/projects/{project_id}/instruments/{instrument_id}/generate：生成单乐器音频
  - POST /api/projects/{project_id}/instruments/{instrument_id}/generate: generate single-instrument audio
- POST /api/projects/{project_id}/instruments/{instrument_id}/repaint：局部重绘
  - POST /api/projects/{project_id}/instruments/{instrument_id}/repaint: local repaint

仓库里还留着旧版 legacy 生成接口，供兼容与参考。
The repo still keeps the old legacy generation endpoints for compatibility and reference.

## 说明
## Notes

- ACE-Step 服务没启动时，应用仍然走 fallback 流程，前后端联调不受影响。
  - When the ACE-Step service isn't running, the app still goes through the fallback flow, so frontend/backend integration isn't blocked.
- 手机要用传感器就得走 HTTPS，用 mkcert 或隧道把本地服务暴露出去，见上面「手机指挥」。
  - The phone needs HTTPS for sensors; expose the local service with mkcert or a tunnel — see "Phone conducting" above.
- 摄像头指挥不需要联网，手部识别的模型与 WASM 运行时都在本地。
  - Camera conducting needs no network; the hand-recognition model and WASM runtime are both local.

## 相关文档
## Related docs

| 文档 | 内容 |
|---|---|
| [docs/CHANGELOG.md](docs/CHANGELOG.md) | 完整版本记录与版本号规则 |
| [docs/USER_GUIDE.md](docs/USER_GUIDE.md) | 用户使用指南，同时是应用内助手的知识来源 |
| [docs/SYMBOLIC_COMPOSER_API.md](docs/SYMBOLIC_COMPOSER_API.md) | 外部符号音乐模型的接入契约 |
| [docs/M6_PLAN.md](docs/M6_PLAN.md) | M6 阶段（两级导航 + 指挥教学）的实施计划 |

| Document | Contents |
|---|---|
| [docs/CHANGELOG.md](docs/CHANGELOG.md) | Full version history and version numbering rules |
| [docs/USER_GUIDE.md](docs/USER_GUIDE.md) | User guide, also the in-app assistant's knowledge source |
| [docs/SYMBOLIC_COMPOSER_API.md](docs/SYMBOLIC_COMPOSER_API.md) | Integration contract for external symbolic music models |
| [docs/M6_PLAN.md](docs/M6_PLAN.md) | Implementation plan for the M6 phase (two-level nav + conducting tutorial) |

## License

本项目采用 [MIT License](LICENSE)。
This project is licensed under the [MIT License](LICENSE).

Copyright (c) 2026 Diankun Gao, Xingwen Zhao

第三方资产：`backend/soundfonts/orchestral.sf2` 由 [MIT 授权的 FluidR3_GM](https://github.com/urish/fluidsynth-manager/blob/master/fluid/FluidR3_GM.sf2)
裁切而来（见「音源」章节），其许可仍遵循原作品条款。
Third-party asset: `backend/soundfonts/orchestral.sf2` is trimmed from [the MIT-licensed FluidR3_GM](https://github.com/urish/fluidsynth-manager/blob/master/fluid/FluidR3_GM.sf2)
(see the "Sound sources" section), and remains under the original work's license.
