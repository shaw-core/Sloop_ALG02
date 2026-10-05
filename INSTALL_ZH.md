# SLOOP ALG‑02 实验固件

版本：`SLOOP ALG02 TEST`，设备标识 `FM-1_982`。基于 SLOOP 2.2 源码 commit `f2b44c219b8a4ac00bc06dca756cdae8a259dd1a`（https://github.com/isod89/sloop-fm1）。这是独立修改版，不是 SLOOP 作者发布的版本。

**已编译并完成电脑端测试；尚未在 FM‑1 真机验证启动、声音、实时负载或安装。安装页面是实际工作的 USB MIDI 更新器，但模拟协议测试不能保证真机兼容。**

## 功能

保留 SLOOP 的三条合成音轨、鼓轨、现场录音、歌曲、音序、实时效果及网页编辑器。合成引擎从 9 种增加为 11 种，工厂预设从 68 个增加为 80 个；新增预设按类别进入音色浏览器。

| 引擎 | 新预设 | 原理 |
|---|---|---|
| PLUCK | NYLON、STEEL、HARP、MUTED、WIRE、BASS | 分数延迟 Karplus–Strong 拨弦物理建模 |
| ADDIT | PURE、HOLLOW、PRISM、GLASS、ORBIT、METAL | 1–8 分音加法合成、频谱移动与非谐波拉伸 |

新增引擎每条音轨最多 4 声部，三条合成音轨共享上游 8 声部预算。现有引擎 ID 0–8 不变，新增为 9/10。默认关闭实验 SLICE，勿在共用预设/工程时切换该编译选项。鼓机和实时效果继续使用 SLOOP 的实现。

PLUCK 参数：DECAY、DAMP、EXCIT、PICK、BODY、DRIVE、TONE、SEED。BODY 加入受 ADSR 控制的正弦支撑音，不跟随弦的自然衰减；PICK 仅影响 PICK 激励。ADDIT 参数：PART、TILT、EVEN、FOCUS、WIDTH、STRET、MOTION、SPEED。过高分音淡出避免越过 Nyquist。高 FOCUS、窄 WIDTH 可能选不到有效分音而无声。

为适应 SLOOP 的内存，拨弦每声部使用 1024 个 int16 样本。正常音符覆盖 MIDI 0–127，极低音使用 Fs/2、Fs/4、Fs/8 的内部更新率。跨越极大范围的低音滑音会限幅，不保证全音域连续滑音。固定缓冲无动态堆分配。

## GitHub Pages 网页安装

交付包 `github-pages/` 已生成完整静态站点，包含固件、网页安装器、编辑器、对应源码和 SHA‑256 信息，**不需要你再编译**。

1. 在 GitHub 新建一个公开仓库，例如 `fm1-sloop-alg02`。
2. 解压交付包，将 **`github-pages/` 里面的所有内容**上传到仓库根目录。不是上传 ZIP 文件本身，也不是只上传 firmware 目录。确认根目录存在 `index.html`，并保留 `webapp/` 与 `firmware/` 的目录结构。
3. 仓库 Settings → Pages → Build and deployment：选择 **Deploy from a branch**，分支 `main`，目录 `/(root)`，Save。
4. 等待 Pages 部署完成，用电脑 Chrome 或 Edge 打开 `https://你的用户名.github.io/fm1-sloop-alg02/`。首页会进入安装器。

安装前先导出当前音色/工程，并保留官方固件和恢复工具。USB 数据线直接连接电脑，关闭 M‑UPGRADE、DAW 和其他占用 MIDI 的网页。页面显示 `FM-1_982 / SLOOP ALG02 TEST`，允许浏览器 MIDI/SysEx 权限，然后点 INSTALL 并等待完成。安装期间不要断电、拔线、关闭页面或休眠电脑。页面会核对固件产品标识和 SHA‑256；实际升级过程中加载器还会检查 CRC。

不要直接双击本地 HTML 来刷写：`file://` 下的浏览器权限和文件读取行为不同，使用 GitHub Pages 的 HTTPS 地址。这个安装器不是官方 SLOOP 网页的快捷链接，它安装的是随包附带的自定义镜像。没有使用你的 GitHub 账号创建仓库或部署网站。

页面若提示缺少固件文件，多半是上传目录层级错误。若没有 MIDI 支持，使用电脑 Chrome/Edge；手机、Safari、Firefox不作为此包的支持安装环境。

## 其他安装方式与返回官方版本

包内 `source/tools/fm1_install.py` 使用与网页相同的协议。技术人员可在交付包根目录运行：

```bash
python3 -m pip install mido python-rtmidi
python3 source/tools/fm1_install.py --info
python3 source/tools/fm1_install.py firmware/SLOOP_ALG02_UNTESTED.fwsc
```

本包未验证通过 M‑UPGRADE 直接安装。SLOOP 上游说明返回官方固件可使用 M‑UPGRADE 配合官方 FM‑1 固件；这不是对本包异常情况下能保证恢复的承诺。切换固件会替换应用程序，原音色/工程格式和存储不能保证兼容；请先导出需要保留的数据。

## 编译与来源

将 `github-pages/source.zip` 解压到交付包根目录，完整源码在解压后的 `source/`，SDK 最小构建依赖在 `sdk-minimal/`（官方 Apache‑2.0 许可证及来源随附）。编译器使用官方 `jieli-linux-toolchains-20250324.1`，pi32v2 clang 4.0.1。Linux x86‑64 上：

```bash
cd source
sh tools/get_toolchain.sh /your/path/jieli
export JIELI_TOOLCHAIN=/your/path/jieli/toolchain
export AC79_SDK=../sdk-minimal
sh build.sh
sh tests/run_tests.sh
python3 web/make_site.py build/felucca.fwsc ALG02-TEST ../github-pages
```

依赖见 BUILDING.md；本次还使用 NumPy/SciPy 计算新增预设的响度补偿，已有 68 个预设的补偿值未改。

硬件平台仍为 AC791N/WL82/pi32v2。本次链接统计：程序 538,244 bytes；`.data+.bss` 88,320/98,304 bytes，缓冲池 330,464/344,064 bytes。内存布局检查通过；不是实时空闲 RAM 或最坏 CPU 负载测试。PLUCK 有一条音轨的缓冲放在池中，两条放在普通 RAM 中，保留 SLOOP 的录音/效果缓冲。

## 验证和试听

`validation/` 包含最终构建、完整主机测试、新增引擎 ASan/UBSan、封装核对和原 093 作为起点的模拟升级记录。LeakSanitizer 在运行环境中关闭。Linux 无上游使用的 CPU 指令计数接口，静态目标指令预算通过不等于真机 CPU 百分比已测得。

新增预设的 golden 基线是本次建立的；原有 97 个 golden 场景没有改变。109 个 golden 场景、1,024 组新增引擎音高/参数测试、音轨复音与松键测试，以及 SLOOP 的录制、鼓机、punch-in、歌曲、UI 和随机使用测试见最终日志。

`demos/ALG01_all_presets.wav` 文件名沿用渲染脚本，内容由本次 SLOOP 源码重新渲染，为 54 秒的十二个新预设试听。它们是电脑端 DSP 音频，不是 FM‑1 实机录音。

## 许可证与界面

应用源码 GPL‑3.0-only，上游版权与许可证保留。SDK Apache‑2.0，采样 CC0，字体和网页图标字体保留各自许可证。未分发上游保留权利的图标图集，设备界面有文字但不显示该图集；其 UI 测试也按这一配置执行。SLOOP 鼓素材使用上游合成鼓和 CC0 实录，没有换成上一版 ALG‑01 的鼓。

SLOOP 的 DIGITAL 是四运算子 FM，不能直接读取 Baud Girl/DX7 的六运算子音色。沿用 SLOOP 的 USB MIDI；未移植原固件的蓝牙或 USB Audio。
