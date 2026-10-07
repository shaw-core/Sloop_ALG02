# SLOOP ALG-05

### 带 DX7、VA 和多种 Karplus–Strong 引擎的 FM-1 四轨 groovebox 固件

适用于 **M-VAVE FM-1** 的 SLOOP 自定义固件，基于 SLOOP 2.2，扩展拨弦、加法、六算子 DX7、VA 与多种 KS 引擎，并提供 GitHub Pages 网页安装器和音色编辑器。

- 固件界面：`SLOOP ALG05 TEST`；设备标识：`FM-1_985`。
- 三条合成音轨 + 一条鼓轨，16 种合成引擎、93 个工厂音色、37 套鼓组。
- 新增 **PLUCK** 拨弦引擎和 **ADDIT** 加法合成引擎，各含 6 个预设。
- 支持音序、实时叠录、歌曲段落、用户采样、现场效果，以及 32 个设备用户音色槽。

这是独立修改的实验版本，并非 SLOOP 原作者发布的版本。本文对应随包提供的固件及编辑器；使用其他版本时，参数和保存格式可能不同。

## ALG05 修复与操作

### 设备实时波形

HOME 和设备 EDIT 的全部参数页现在显示实际音频输出的实时波形，包括 DX7 算子页、EDIT 1 / 2 与 VOICE 页。此前只有 HOME 接入了示波器，EDIT 使用空图形类型，所以无论怎么改参数都不显示；ALG05 接入 EDIT 的波形绘制，并每两帧刷新一次，刷新不依赖旋钮变化。

按 **EDIT** 进入参数页，弹奏或运行音序，持续发声时调整参数应能看到波形变化。重复短按 EDIT 切换同组页面；不要长按，长按 EDIT 是原有音序擦除层。这里显示的是所有轨道和效果混合后的总输出，并非每个 DX7 算子的独立波形。静音时显示水平线；没有发声时改变参数不会凭空产生波形。保持左右声道平均采样和稳定缓冲快照。电脑测试覆盖真正的 EDIT 绘制路径和不操作旋钮时的持续刷新；LCD、音频中断时序和刷新速度仍需真机确认。

### 在设备上逐个编辑 DX7 算子

1. 先选中使用 DX7 的合成轨与音色，然后短按 **EDIT**。
2. 第一次是 FM 宏页，第二次是 CTRL 宏页；继续短按 EDIT，进入下面的 DX 专用页。其他引擎不会显示这些页面。
3. 每个 DX 专用页的 **旋钮 1（OP）** 都可选择 **OP1–OP6**。旋钮 2–4 修改当前算子，不会改动其他算子。同一轨的算子选择跨页保留。
4. 边弹奏边调节。包络 Rate/Level 改动的完整效果通常需要重新触发音符试听。
5. 修改只在当前音色中生效。保存请用设备 SAVE → USER 页选择 U01–U32、执行 SAVE（连续两次旋钮动作确认），或网页 Quick slot 的 Save / replace slot。需要从工厂入口调用时，再绑定该用户槽。工程保存也会保留六算子数据。

| 页面 | 旋钮 1 | 旋钮 2 | 旋钮 3 | 旋钮 4 |
| --- | --- | --- | --- | --- |
| DX OSC | OP1–OP6 | OUT 输出 0–99 | COARSE 粗调 0–31 | FINE 细调 0–99 |
| DX TUNE | OP1–OP6 | MODE：Ratio / Fixed | DETUNE：−7～+7 | ENABLE：当前算子开关 |
| DX RATE1 | OP1–OP6 | R1 | R2 | R3 |
| DX RATE2 | OP1–OP6 | R4 | — | — |
| DX LEVEL1 | OP1–OP6 | L1 | L2 | L3 |
| DX LEVEL2 | OP1–OP6 | L4 | RSCALE：包络速率缩放 | — |
| DX SENS | OP1–OP6 | AMS：AM 灵敏度 | VEL：力度灵敏度 | BREAK：键盘断点 |
| DX KEY | OP1–OP6 | LDEPTH：左侧深度 | RDEPTH：右侧深度 | — |
| DX CURVE | OP1–OP6 | LCURVE：左侧曲线 | RCURVE：右侧曲线 | — |

Rate / Level 的范围均为 0–99。曲线为 −LIN / −EXP / +EXP / +LIN。BREAK 使用 DX7 原始编号 0–99，对应 MIDI 17–116（0 为 A−1）；不是直接的 MIDI 音符编号。Ratio 模式下 COARSE=0 表示 0.5 倍频；Fixed 模式的粗/细调含义与倍频模式不同。ENABLE 与 CTRL 页 OPS 位掩码是同一组开关。ALGORITHM 旋钮仍用于切换音轨，SELECT 旋钮仍控制速度。

### 修改并替换任何合成器工厂音色

1. Sound 页选择引擎和工厂音色；修改普通参数，DX7 的六算子完整参数就在 Sound 页。
2. Quick slot 选择 U01–U32，填名称，点击 **Save / replace slot** 保存当前声音。
3. 在 **Factory entry** 选择要替换的工厂入口，点击 **Bind slot**，确认。
4. 今后从设备工厂列表或网页选择该入口，会调用绑定用户槽；断电后保留。继续编辑并保存同一用户槽，也会更新工厂替身。
5. 恢复原厂：Quick slot 选该槽，点击 **Unbind slot**。删除用户槽也会解除绑定。

所有 16 个合成引擎的工厂入口都可绑定，但 **最多同时保存 32 个替身**，它们占用现有用户槽，不能同时覆盖全部 93 个工厂预设；浏览器库不受该数量限制。每个槽只绑定一个工厂入口，同一入口不能绑定两个槽。名单保留原厂入口名称，实际声音来自用户槽；替身的名称在用户槽中显示。鼓组不通过此合成器槽协议覆盖，请使用 Drums / Samples 的对应流程。

### 批量导入与替换

1. Library → Import，可以一次选择多个 JSON / SYX 文件；DX7 的 32 音色 SYX 会拆成独立条目。也可从网页外部来源导入。导入仅进入浏览器库。
2. 按引擎分类、搜索和排序；勾选要写入的音色，或点 **Select visible**。**实际批次仅包含当前筛选列表中勾选的条目**，顺序就是当前列表顺序。
3. 点击 **Batch replace selected**，输入起始槽 1–32。比如选择 8 个、起始 5，会连续替换 U05–U12。超过剩余槽位会拒绝，绝不会截断或覆盖绕回。
4. 如要同时替换工厂入口，先勾选 **Bind factory entries** 并选起始工厂入口；会绑定该引擎内连续的入口，数量不足会拒绝。如果目标入口已绑定其他槽，先在 Sound 页取消原绑定。
5. 确认范围后，网页先下载目标槽的 `before-batch-日期.json` 备份，再逐槽写入，进度显示已完成数量。请确认备份下载已保存；它保存音色，不包含工厂绑定关系。
6. 遇到失败立即停止，不自动重试或回滚已经完成的槽。通信中断时最后一个槽结果可能不确定，重连后用 Refresh 检查，再补写。工厂绑定在全部音色写入后逐个执行；如果此阶段失败，音色已经写入，已成功的绑定也保留，剩余项需重新绑定。

### DX7 的完整编辑与音色

Sound 页选 DX7 后自动读取 **145 个标准声音字段**：每个算子的四段 Rate/Level、音量、Ratio/Fixed、Coarse/Fine、Detune、键盘左右深度/曲线/断点、速率缩放、力度与 AM 灵敏度；全局 32 算法、反馈、同步、音高包络、移调、LFO 波形/速度/延迟/PM/AM 和 PM 灵敏度。点 **Apply** 应用当前编辑，Quick replace 才保存到设备槽。切换音色会重新读取并丢弃尚未 Apply 的编辑；改设备宏后可点 Read 刷新详细值。

设备八个宏现在是 ALG / FB / BRIGHT / LFO / PMD / AMD / TRANS / OPS；TRANS 24 表示不移调。设备宏与网页详细数据、导出和保存保持一致。ALG05 新增设备上逐个算子的 21 项声音字段和 ENABLE；全局音高包络、同步与其余 LFO 细节仍使用网页详细编辑。包络、键盘/力度缩放、失谐和部分调制深度仍为近似 DSP，实现不是原版 DX7/Dexed 的逐样本复刻，复音限制仍为每轨 2 音。

内置 DX7 声音改为自制 **FM EPIANO、FM BASS、FM BELL**，保留 SIX SINES 作为参考声音。这些是本项目原创测试音色，不是 Yamaha 工厂库；实际音质需要真机试听。想用经典音色，可从 Yamaha Black Boxes 下载标准 SYX，导入、试听，再单个或批量替换。

### 各引擎参数是否被删减

| 范围 | 核查结论 |
| --- | --- |
| ANALOG / DIGITAL / PHASE / LOFI / SAMPLE / VOICE / TRIO / WHEEL / GRAIN / ADDIT | 引擎源码与 ALG03 前保留版本一致，没有删减原有参数；不是各历史合成器全部功能的模拟器。 |
| PLUCK | 原来的 6 个预设和渲染结果保留；独立 KS 分支使用共享延迟线。 |
| DX7 | ALG03 已有完整网页字段，但入口隐蔽、设备宏有限；ALG04 将详细编辑移到 Sound 页并补宏，ALG05 增加设备算子独立编辑，DSP 仍存在上述近似。 |
| VA | 独立 VA 实现，8 个引擎参数 + 公共包络/滤波/调制/效果；不是移植 Baud Girl VA，因此它的专用参数/音色不一一对应。 |
| KSDRUM / KSWIRE / KSCOMB | 独立参数化 KS 变体，分别控制随机反馈、色散与第二梳状分支；不承诺复刻其他硬件/插件所有参数。 |

参见 [引擎参数审查](docs/ENGINE_PARAMETER_AUDIT.md)。

## ALG05 测试版新增功能

- **DX7**：六算子、32 种连接算法；网页导入标准 Yamaha 单音色和 32 音色库 `.syx`，展开后逐个试听 / 替换。设备每条 DX7 音轨最多 2 音复音，完整外部库留在浏览器端。
- **VA**：SAW / SQR / TRI / SIN / SYNC，五振荡器叠加、失谐、LP12 / LP24 / BP / HP、共振与驱动；每轨最多 4 音。
- **KSDRUM、KSWIRE、KSCOMB**：独立引擎分类，分别采用随机反相反馈、刚性弦色散、第二梳状反馈分支；每轨最多 4 音。与 PLUCK 共用固定延迟线，节省内存。
- **外部预设来源**：网页提供本项目的 16 个引擎分类 JSON 和 DX7 ROM 库直链选择；完整来源与格式说明见 [PRESET_SOURCES.md](docs/PRESET_SOURCES.md)。
- 保留上一版的设备引擎分类、网页单槽替换、个人开机页与 About。

**已支持的参数音色格式是 SLOOP / Felucca JSON、标准 DX7 SYX，以及 FM-1+VA 备份中的 FM 部分。** Caskexe/DX 主要提供 `.sf2` / Arturia `.dx7x`，不属于 DX7 参数文件；Baud Girl 的 VA 专用 SYX、OB-Xf FXP 和 Surge 音色尚未支持转换。WAV 等音频从 Samples 页导入。

### 安装前与测试顺序

1. 用原版编辑器导出用户音色库 / 设备银行 JSON，并备份工程；保留上一版固件。
2. 更新后先检查一个旧用户槽、旧工程的轨道和音序，再试听新增引擎。
3. 用本版本编辑器导入 DX7 SYX，试听一个音色，写入空槽；然后换另一个音色覆盖同一槽。
4. 重启后重新加载该槽，检查 DX7 参数；再测试项目保存和切换。
5. 测试 VA 与三种独立 KS 分类，同时播放鼓轨和其他音轨，检查声音与操作响应。

ALG05 与 ALG04 使用相同的银行 v3 / 工程 FUN6 格式，并自动读取 ALG02 / ALG03 用户银行及 FUN4 / FUN5 工程，保留音色、DX7 扩展数据和音序。新银行记录为 v3，工程为 FUN6；保存后旧 ALG02 / ALG03 固件不能直接读取新格式，回退前应导出 JSON 和工程备份。更换固件并不等于把现有用户槽重新增加一份。

## 项目来源

本项目是在 **[isod89/sloop-fm1](https://github.com/isod89/sloop-fm1)** 的 SLOOP 2.2 基础上修改的版本。SLOOP 将 FM-1 改造为三条合成轨与一条鼓轨组成的现场 groovebox，提供实时录音、步进音序、歌曲段落、采样与网页编辑器。

SLOOP 本身源自 **[Felucca](https://github.com/hugelton/Felucca)**，由 Leo Kuroshita（[@kurogedelic](https://github.com/kurogedelic)）/ [Hügelton Instruments](https://hugelton.com/) 开发。原有合成引擎、音序器、编辑器与安装器的基础来自这些上游项目。

ALG-05 使用的上游基线是提交 [`f2b44c2`](https://github.com/isod89/sloop-fm1/tree/f2b44c219b8a4ac00bc06dca756cdae8a259dd1a)。上游后续更新不会自动包含在这个固件中。

## ALG-05 新增了什么

| 项目 | SLOOP 2.2 基线 | ALG-05 |
| --- | --- | --- |
| 合成引擎 | 9 种 | **16 种**，新增 PLUCK、ADDIT、DX7、VA、KSDRUM、KSWIRE、KSCOMB |
| 工厂音色 | 68 个 | **93 个**，共新增 23 个 |
| 合成音轨 / 鼓轨 | 3 + 1 | 3 + 1 |
| 鼓组 | 37 套 | 37 套，沿用上游 |
| 用户音色槽 | 32 个 | 32 个，沿用上游 |
| 网页安装 | 上游固件安装器 | 随包附带 ALG-05 固件，并校验 SHA-256 |
| 网页编辑 | 上游编辑器 | 支持新增引擎参数与预设 |

- **PLUCK**：分数延迟 Karplus–Strong 拨弦物理建模，可调整激励、阻尼、拨弦位置与支撑音。新增 NYLON、STEEL、HARP、MUTED、WIRE、BASS。
- **ADDIT**：1–8 个正弦分音的加法合成，可调整频谱倾斜、奇偶分音比例、频谱重点、宽度、非谐波拉伸与频谱运动。新增 PURE、HOLLOW、PRISM、GLASS、ORBIT、METAL。
- 新预设加入按类别排列的工厂音色浏览器；原有引擎编号 0–8 保持不变，新引擎编号为 9、10。

原版的 WHEEL 已属于风琴专用加法合成；ADDIT 增加的是可以自由调整频谱与分音运动的另一种加法引擎。原版中的拨弦风格预设，也与 PLUCK 的弦振动模型不同。

### 保留的 9 种合成引擎

| 引擎 | 合成方式 | 典型用途 |
| --- | --- | --- |
| ANALOG | 虚拟模拟减法合成 | 贝斯、主音、铺底、Supersaw |
| DIGITAL | 四运算子 FM，8 种连接算法 | 电钢琴、钟声、FM 贝斯 |
| PHASE | 相位失真 PD | CZ 风格弦乐、共鸣音色 |
| LOFI | 低位深芯片合成与小型波表 | 游戏机音色、8-bit 琶音 |
| SAMPLE | 采样回放 | 钢琴、真实乐器、用户录音 |
| VOICE | 共振峰人声合成 | 元音、合唱、Talkbox |
| TRIO | 三振荡器，环形调制、硬同步与滤波 | 厚实主音、和弦 Stab |
| WHEEL | 音轮风格拉杆风琴，专用加法合成 | 风琴 |
| GRAIN | 基于采样的颗粒合成 | 氛围与变化纹理 |

实时叠录、步进力度与连击、单键和弦、16 个现场效果、DUST / DUCK / FILTER、工程自动保存和用户采样均沿用 SLOOP 的实现。

## 链接导航

### ALG-05 自定义版

项目仓库：[shaw-core/Sloop_ALG02](https://github.com/shaw-core/Sloop_ALG02)。以下为项目的部署地址。需要上传本测试包后，线上页面才会更新为 ALG05。

- [ALG-05 网页安装器](https://shaw-core.github.io/Sloop_ALG02/webapp/installer/)
- [ALG-05 网页音色编辑器](https://shaw-core.github.io/Sloop_ALG02/webapp/editor/#sound)
- [下载随仓库附带的固件](firmware/sloop-ALG05-TEST.fwsc)
- [下载对应版本源码](source.zip)

上面两条文件链接对应交付包的目录结构。源码保留为 ZIP 即可，不影响网页安装与编辑。

### SLOOP 上游与参考文档

- [SLOOP 源码仓库](https://github.com/isod89/sloop-fm1)
- [SLOOP 原版 GitHub Pages / 安装器](https://isod89.github.io/sloop-fm1/)
- [SLOOP 原版网页编辑器](https://isod89.github.io/sloop-fm1/webapp/editor/)
- [SLOOP 原版完整手册](https://github.com/isod89/sloop-fm1/blob/main/SLOOP.md)
- [SLOOP 原版发布页](https://github.com/isod89/sloop-fm1/releases)
- [构建与测试说明](https://github.com/isod89/sloop-fm1/blob/main/BUILDING.md)
- [编辑器 SysEx 协议](https://github.com/isod89/sloop-fm1/blob/main/web/EDITOR_PROTOCOL.md)
- [Felucca 源码仓库](https://github.com/hugelton/Felucca)
- [FM-1-transporter 恢复工具项目](https://github.com/kurogedelic/FM-1-transporter)

**上游安装器安装的是原版 SLOOP，不包含 ALG-05 的 PLUCK / ADDIT。使用新增引擎请打开自己部署的 ALG-05 页面。** 编辑新增音色也建议使用随包提供的编辑器。上游手册用于参考共同功能，新增引擎请以本 README 为准。

## 1. 网页地址与安装

部署后，网站地址通常为：

```text
安装首页：https://shaw-core.github.io/Sloop_ALG02/
音色编辑：https://shaw-core.github.io/Sloop_ALG02/webapp/editor/
```

编辑器是控制 FM-1 的界面，声音由 **FM-1 本机**输出；电脑网页不负责合成声音。

### 已安装固件

直接打开音色编辑页，按第 3 节连接即可，不必重复烧录。

### 首次安装或更新

1. 先导出需要保留的音色、工程和采样，并保留官方固件与恢复工具。
2. 使用电脑上的 Chrome 或 Edge，通过 USB **数据线**连接 FM-1。
3. 关闭 M-UPGRADE、DAW 及其他正在连接 FM-1 的 MIDI 网页。
4. 打开网站安装首页，确认显示的是 `FM-1_985 / SLOOP ALG05 TEST`。
5. 点击 **INSTALL**，允许 MIDI / SysEx 权限，等待页面提示完成并让设备重启。

安装过程中保持供电，不要拔线、关闭页面或让电脑休眠。使用部署后的 HTTPS 页面，不要直接双击本地 HTML 文件安装。

## 2. FM-1 本机的基本操作

SLOOP 将 FM-1 变为四轨 groovebox。**ALGORITHM 在这里用于选音轨**，不是直接选择合成算法。

| 控件 | 操作 |
| --- | --- |
| ALGORITHM | 选择音轨 1–4；1–3 为合成器，4 为鼓机 |
| PRESETS | 切换当前音轨的工厂音色或鼓组 |
| SELECT | 调整速度 |
| PLAY | 开始 / 停止所有音轨播放 |
| REC | 进入录音或结束录音；播放时用于叠录 |
| HOME | 返回音轨总览；长按打开菜单 |
| EDIT / ENV / LFO 等功能键 | 短按进入对应参数页，配合四个旋钮修改 |
| 长按功能键 | 打开该功能的操作层，白键和旋钮的功能会改变；以屏幕提示为准 |
| 长按功能键，再按 HOME | 锁定操作层；其他功能键可退出 |
| 长按 EDIT + OCT− / OCT+ | 撤销 / 重做，支持一级 |
| 长按 REC 约 2 秒 | 清空当前音轨的音序，可撤销 |

切换工厂音色会重新加载音色参数。想保留自己的修改，请先存入用户音色槽或导出。

### 按合成引擎快速找音色

本机：选择音轨 1–3，先短按 EDIT 进入参数页，再短按 SAVE 进入保存页面组。重复短按 SAVE，直到标题显示 **PRESETS**。在 HOME 音轨总览直接短按 SAVE 会打开 SONG，不是音色浏览页。

- **KNOB 2 / BANK**：切换 ALL → ANALOG → DIGITAL → PHASE → LOFI → SAMPLE → VOICE → TRIO → WHEEL → GRAIN → PLUCK → ADDIT → USER。
- **KNOB 1 或 PRESETS**：只浏览当前分类中的音色。
- **ALL**：完整工厂音色列表，加上已使用的用户音色槽。
- **USER**：仅显示已保存的用户槽；空库时不会加载任何音色。
- 分类选择在本次开机中有效，重启回到 ALL；鼓轨继续用 PRESETS 选择鼓组。

选择一个非空分类会加载该分类的第一个音色。切分类前请保存尚未保存的音色修改。

网页：Sound 页的 **Engine** 下拉框就是合成引擎大分类，下面的 Presets 列表显示该引擎的工厂预设。Library 中的引擎筛选用于浏览器音色库，设备槽列表中也显示所属引擎。

### 第一次录一段循环

1. 用 ALGORITHM 选择音轨 4，用 PRESETS 选择喜欢的鼓组。
2. 空音序、停止播放时按 REC，等待第一下演奏开始自由录音。
3. 用白键打鼓；在最后一小节结束、下一拍的“1”上再次按 REC，确定循环长度并开始播放。
4. 播放中再按 REC 可以叠录；再次按 REC 结束叠录，播放继续。
5. 切到音轨 1，选择低音音色，在循环播放时按 REC 录入贝斯。
6. 音轨 2、3 可以继续录和弦、旋律；按 PLAY 停止。

自由录音期间按 PLAY 会取消该次录入，单次自由录音最长约 24 秒。已有音序时，停止状态下按 REC 进入待录，再演奏或按 PLAY 开始。

### 常用演奏操作

| 操作 | 用途 |
| --- | --- |
| 鼓轨上的 16 个白键 | 演奏 16 种鼓声；黑键重复左侧白键的鼓声 |
| 鼓轨长按 OCT− / OCT+ 再打鼓 | 轻击 / 重击 |
| 长按 ARP 再按住音符 | 音符重复；可录入连击 |
| 长按 SEQ | 白键编辑步进；OCT 切换步进页，旋钮修改音序参数 |
| 长按 GLO | 白键 1–4 静音、5–8 独奏；旋钮 1–4 调整四轨音量 |
| 长按 FX | 白键触发现场效果；旋钮 1 为 FILTER、2 为 DUST、3 为 DUCK |
| 长按 SCL | 设置调性、音阶和单键和弦；具体功能见屏幕 |

鼓轨的 F3 为底鼓、A3 为军鼓、B3 为拍手、C4 为闭镲。FILTER 中间关闭、向左低通、向右高通；DUST 添加旧采样器 / 唱片质感；DUCK 让合成音轨随底鼓产生闪避。

## 3. 连接网页音色编辑器

1. 给 FM-1 通电，用 USB 数据线连接电脑，并连接耳机或音箱。
2. 用 Chrome / Edge 打开部署网站的 `/webapp/editor/` 页面。
3. 点击 **Connect**，允许浏览器访问 MIDI，并允许 SysEx。
4. 等待页面读出固件版本和音轨信息。
5. 选择音轨 **1、2 或 3**，再打开 **Sound** 页修改合成音色。

音轨 4 是鼓轨，不适用 PLUCK、ADDIT 等合成器音色的保存操作。网页与设备参数会同步；使用一个编辑器页面连接即可。

### 各个页面有什么用

| 页面 | 用途 |
| --- | --- |
| Sound | 选择引擎、工厂预设；修改音色参数、包络、滤波和调制；保存 / 读取 JSON |
| Sequencer | 修改音序；鼓轨显示多行鼓步进，可设置力度与连击 |
| Tracks | 调整四轨音量、声像、静音等混音参数 |
| Library | 管理浏览器音色库，以及 FM-1 的 32 个用户音色槽 |
| Samples | 管理用户采样；上传 WAV，由网页转换后传入设备 |
| Projects | 管理工程；工程用于保存多音轨创作，区别于单个音色 |
| Settings | 查看设备信息及系统设置 |

## 4. 网页上怎样修改音色

### 从预设开始

1. 选择音轨 1–3 中的一条。
2. 打开 **Sound**，选择引擎，再选择一个工厂预设作为起点。
3. 在 FM-1 上按键试听，也可以让已有音序循环播放。
4. 拖动网页参数控件，设备会实时收到修改。建议每次只改一两个参数，边听边调整。
5. 调好后，按第 6 节保存。不要先切换到另一个工厂预设。

不同引擎显示不同参数；包络、滤波、LFO 等可用项以页面为准。ADSR 通常分别控制起音时间、衰减时间、持续电平和松键后的释放时间。

### PLUCK：拨弦合成

预设：**NYLON、STEEL、HARP、MUTED、WIRE、BASS**。

| 参数 | 作用 |
| --- | --- |
| DECAY | 弦的自然衰减，影响尾音长短 |
| DAMP | 弦的高频阻尼，影响明亮度和尾音音色 |
| EXCIT | 激励类型：NOISE / PICK / SOFT |
| PICK | 拨弦位置，仅在 PICK 激励下生效 |
| BODY | 添加由 ADSR 控制的正弦支撑音，增加音高基音感 |
| DRIVE | 驱动 / 饱和程度 |
| TONE | 音色明暗调整 |
| SEED | 激励噪声的种子，用于改变或复现拨弦质感 |

**示例：把 NYLON 调成短促拨弦。** 先减少 DECAY，再调整 DAMP 让声音柔和；少量添加 DRIVE 增加存在感。想听拨弦位置差异，把 EXCIT 设为 PICK，再调整 PICK，并重新按键触发。

PICK 和 SEED 的变化主要在下一次音符触发时体现。BODY 使用 ADSR，不完全跟随弦的自然衰减：松键前持续的基音可能来自 BODY。

### ADDIT：加法合成

预设：**PURE、HOLLOW、PRISM、GLASS、ORBIT、METAL**。

| 参数 | 作用 |
| --- | --- |
| PART | 分音数量，1–8 个 |
| TILT | 改变频谱倾斜，调整低分音与高分音的比例 |
| EVEN | 调整偶数分音的比例 |
| FOCUS | 频谱重点所在位置 |
| WIDTH | 频谱重点覆盖的宽度 |
| STRET | 拉伸分音频率，产生非谐波、钟声或金属质感 |
| MOTION | 频谱运动幅度 |
| SPEED | 频谱运动速度 |

**示例：从 PURE 制作变化中的音色。** 增加 PART，再调整 TILT 和 EVEN；扩大 WIDTH，逐渐增加 MOTION 与 SPEED。想做金属敲击，可从 GLASS / METAL 开始调整 STRET，并缩短包络。

如果声音突然很小或消失，先扩大 WIDTH、降低 FOCUS，并增加 PART。高 FOCUS 配合很窄的 WIDTH，可能没有覆盖任何有效分音；很高的音符也会减少可用分音。

新增引擎每条合成音轨最多 4 声部，三条合成音轨共享 8 声部预算。多轨同时演奏大和弦时，较早的声音可能被替换。

### DX7：导入、详细编辑与替换

1. 连接安装 ALG05 的 FM-1，在 Library 页按 Import，选择 `.syx` 文件。整个音色库拆为独立条目，保存在浏览器 Library，不会自动改写设备。
2. Library 的引擎筛选选 **DX7**；输入名称搜索，比如 `E.PIANO`。选中条目按 Audition。
3. 常规参数页可改 ALG（1–32）、FB（0–7）、BRIGHT、LFO、PMD、AMD、TRANS 和 OPS（0–63 位掩码，默认全部算子开启）。
4. Sound 页自动展开 **DX7 六算子详细编辑** 并读取当前音色；Read 可刷新。OP1–OP6 每个面板可修改电平、频率模式 / 粗调 / 微调 / 失谐、四段包络、键盘缩放、力度与 AM 灵敏度。下方可修改音高包络、移调和 LFO。
5. 修改后按 Apply 试听。此时只改变当前轨道；按 Quick replace / 快速替换选择 U01–U32，保存到设备。覆盖已用槽时确认。也可将 Library 中的音色直接 To slot。
6. 切换到别的 DX7 音色或轨道后，详细参数自动刷新。未按 Apply 的详细参数还没有发送到设备。
7. 按 Save to library / 保存当前音色到网页库，再 Export，可保留新的 128 字节 DX7 参数。Save to file 也会携带这些参数。保存项目会保留各合成轨的 DX7 数据。

Ratio 模式粗调 0 表示 0.5 倍频；Fixed 模式使用固定频率。Detune 的 7 为中性；Transpose 的 24 为不移调。LFO wave 0–5 依次为 TRI、下降锯齿、上升锯齿、SQR、SIN、S&H；Key curve 0–3 为 −LIN、−EXP、+EXP、+LIN。

DX7 的响度包络与释放由每个算子的包络决定；通用 ENV 页的 ADSR 不会替代算子包络。

这是 DX7 格式兼容引擎，键盘 / 力度缩放、部分调制深度和极高频处理存在近似，不保证与原机 / Dexed 逐样本相同。移植来源和限制见 [DX7_PROVENANCE.md](docs/DX7_PROVENANCE.md)。

### VA：叠加振荡器和同步

| 参数 | 用途 |
| --- | --- |
| WAVE | SAW、SQR、TRI、SIN、SYNC |
| SHAPE | SQR 脉宽；SYNC 从振荡器频率比 |
| DETUNE | 控制五振荡器之间的失谐，单位 ct |
| SUPER | 中心振荡器与五振荡器叠加的混合 |
| FILTER | LP12、LP24、BP、HP |
| CUTOFF / RES | 滤波截止频率与共振 |
| DRIVE | 进入滤波器前的软削波 |

预设：SUPER SAW、SYNC LEAD、VA PAD。SAW / SQR 采用带限振荡器；SYNC 的重置与 TRI 高音仍可能产生混叠。VA 是独立实现，不是 Baud Girl VA 的移植，暂不读取其专用预设。

### PLUCK 以外的三种独立 Karplus–Strong 引擎

| 分类 | 实际算法变化 | 主要控制 | 工厂音色 |
| --- | --- | --- | --- |
| KSDRUM | 延迟线循环中随机反相，产生鼓击 / 非谐波衰减 | NOISE 反相概率、DECAY、DAMP | KS DRUM、DRY TOM |
| KSWIRE | 一阶全通色散分支，补偿低频群延迟 | STIFF 刚度、DECAY、DAMP | STIFF WIRE、DISP BELL |
| KSCOMB | 半周期附近的第二反馈抽头与主反馈混合 | COMB 混合、DECAY、DAMP | COMB BELL、HOLLOW KS |

三类都有 BODY、DRIVE、TONE 和 SEED，且有独立网页 / 设备引擎分类。先加载预设再修改。色散与额外抽头会改变泛音与音高感；KSDRUM 是 KS 随机反馈鼓音近似，不是二维鼓膜物理仿真。PLUCK 保留 NOISE / PICK / SOFT 和原来的 6 个预设。

## 5. 音色到底保存在哪里

| 保存位置 | 操作入口 | 保存后有什么效果 |
| --- | --- | --- |
| FM-1 用户音色槽 | Library → 设备区域 → **Store current sound** | 写入设备用户音色库，脱离电脑也能保留和调用 |
| 浏览器音色库 | Library → **Save current sound** | 保存在当前浏览器的网站数据中；不会自动写入设备用户音色槽 |
| 电脑 JSON 文件 | Sound → **Save to file**，或 Library → **Export** | 下载文件，可备份、分享和再次导入 |
| 完整工程 | Projects | 保存多轨创作；单个音色文件不能代替完整工程备份 |

浏览器音色库使用本地数据库。换电脑、换浏览器、换网站地址或清除网站数据后，不保证能看到原来的音色库；请定期使用 **Export library** 导出备份。

## 6. 把修改好的音色存进 FM-1

### Sound 页一键保存 / 替换

1. 在 Sound 页选择合成音轨并调整音色。
2. 在 **设备用户音色槽 / Device slot** 下拉框选择 U01–U32 的一个槽，列表显示原名称和引擎。
3. 可在 **名称 / Name** 输入新名称；已有槽留空会保留原名称，空槽留空由设备自动命名。
4. 点击 **保存 / 替换此槽 · Save / replace slot**。空槽直接保存，已有音色需要确认覆盖。
5. 等待保存成功提示，再选择下一个槽继续操作。

Quick replace 一次只写选中的槽；Library 的 Batch replace 可批量覆盖，流程见下文。设备名称为最多 12 个可打印 ASCII 字符，建议使用英文、数字或空格；中文名称请保留在浏览器库中。保存成功后数据已经写入设备用户库，无需另外按本机 SAVE。

单槽保存写入用户槽。若要改变工厂预设入口，保存后使用 Bind slot 绑定该工厂预设；之后在设备或网页选择该入口会加载替身。原厂数据留在固件中，Unbind 可恢复。

### 直接保存当前音色

1. 在 Sound 页调好音色，保持当前合成音轨选中。
2. 打开 **Library**。
3. 在 **User presets on the device** 区域选择一个用户槽，共 32 个。
4. 点击 **Store current sound**。
5. 输入名称；若槽已有音色，确认是否覆盖。
6. 等待页面提示保存成功。

以后在设备用户音色库中调用，或在网页选择该槽后点击 **Load**，即可加载到当前合成音轨。

### 将浏览器库中的音色写入设备

1. 在 Library 的浏览器列表选中一个音色。
2. 点击 **Audition** 将它加载到当前合成音轨，再按设备琴键或播放音序试听。这个操作本身不会写入用户槽。
3. 在设备区域选择目标槽。
4. 点击 **To device slot**，已有内容时确认覆盖。

设备区域的 **To library** 则将选中的设备音色复制到浏览器库。**Erase** 会删除选中用户槽，请核对后再确认。

## 7. JSON 导出、导入和分享

### 备份单个音色

- **Sound → Save to file**：导出当前音色快照，文件也包含当前音轨的步进数据。
- **Library → Save current sound**：命名并加入浏览器库；选中后点击 **Export** 导出该条目。
- **Library → Export library**：导出整个浏览器库。
- 设备区域的 **Export bank**：导出设备用户音色库。

### 读取文件

- **Sound → Load file**：将支持的音色 JSON 应用到当前合成音轨；文件中的步进也可能被载入，先备份重要音序。
- **Library → Import**：将支持的 JSON 导入浏览器库，再选中 **Audition** 试听，或 **To device slot** 写入设备。

导入浏览器库不等于已经存入 FM-1。想把文件中的音色永久存到设备，请完成用户槽写入步骤。

文件名或 JSON 内部出现 `felucca` 是沿用上游编辑器格式，并不表示安装了另一套固件。优先在同一版本固件之间交换音色。DIGITAL 是原有四算子 FM；六算子音色导入新的 DX7 引擎。Baud Girl VA 专用文件尚未转换。普通音频 WAV 不是音色 JSON，应到 Samples 页上传。

### 从外部网页导入预设

Library 页的 **导入预设链接 / Import URL** 可读取外部预设：

1. 复制兼容音色 / 音色库 JSON 的 **HTTPS 文件直链**，粘贴到 URL 输入框。
2. 点击 Import URL，等待文件下载和解析。支持的文件类型与 Library → Import 相同，最大 4 MiB。
3. 导入成功后在浏览器库中选中音色，点击 Audition 试听。
4. 在设备区域选择目标用户槽，点击 To device slot 保存 / 替换；或试听后回到 Sound 页使用快捷保存按钮。

网页地址必须返回 JSON 文件内容，不能是文章页、GitHub 文件预览页或 ZIP。GitHub 上的 JSON 可以使用文件的 Raw 直链，例如：

```text
https://raw.githubusercontent.com/用户名/仓库名/main/presets/音色.json
```

如果对方网站没有允许跨站读取（CORS）、需要登录或链接失效，网页会提示失败。先用浏览器下载 JSON，再通过 Library → Import 导入即可。导入只加入浏览器库，不会自行覆盖设备音色。

FM-1+VA 的 `Save a backup` 文件也可从 Import 读取：提取其中的 FM 音色，VA / 未知类型跳过并显示数量。此转换只保留 FM 音色参数，不搬迁原版的效果设置或音序；纯 VA 文件会提示不支持。

**DX7 `.syx` 已支持。** 可使用 [Yamaha Black Boxes](https://yamahablackboxes.com/collection/yamaha-dx7-synthesizer/patches/) 下载的标准单音色 / 32 音色库。Import 选择文件，或粘贴 HTTPS 文件直链。无需改扩展名；ZIP 需先解压，SF2 / DX7X 不支持直接转换。

## 8. 音序、工程与采样

- **Sequencer** 修改当前音轨的步进。鼓轨可编辑 16 种鼓声的力度与 x1–x4 连击；先选力度 / 连击，再编辑格子。
- **Tracks** 用于混音，不必为了改音量重新制作音色。
- **Projects** 用于工程保存与加载。设备会自动保存工作状态，但这不等于把每个修改后的音色写入用户音色槽，也不能代替导出备份。
- **Samples** 支持三个用户采样槽 USR1–USR3，上传 WAV 后转换为设备格式；切片可用于演奏 16 段声音。
- 本机长按 **SAVE** 时，白键 1–4 调用 A–D 段落，5–8 将当前循环保存到对应段落。段落、工程和单个用户音色是不同对象。

### 操作层完整速查

| 长按操作层 | 白键 / 按钮 | 旋钮 |
| --- | --- | --- |
| EDIT | 按键删除相应音符 / 鼓声；播放时删除经过的步进，停止时从整条音序删除；OCT− 撤销，OCT+ 重做 | 1 SHIFT 移位；2 LENGTH 加倍 / 减半；3 TRANSPOSE 移调 |
| ARP | 按住音符按节奏重复；录音时写入连击 | 1 RATE：1/8、1/16、1/32、32T、1/64 |
| SEQ | 16 白键对应步进；空步进按下添加，已有步进短按清除；OCT 或前四个黑键切换第 1–4 页 | 无步进按住：1 音符 / 鼓声、2 DIV、3 SWING、4 LENGTH；按住步进：1 音符 / 鼓声、2 LEVEL、3 RATCHET |
| SCL | 按音符设置三条合成轨的共同根音 | 1 当前轨和弦；2 全局音阶；3 OFF / SNAP / WHITE 键盘映射；4 当前轨移调 |
| GLO | 白键 1–4 静音，5–8 独奏，16 连续轻敲设置速度 | 1–4 分别控制四轨音量 |
| FX | 白键触发下表的现场效果，松键结束 | 1 FILTER、2 DUST、3 DUCK |
| SAVE | 白键 1–4 调用 A–D，5–8 保存 A–D，13 LOOP / SONG，14 SONG REC，16 打开歌曲链 | 歌曲页：1 步骤，2 段落，3 小节数，4 链长度 |

FX 的白键 1–16：LOOP 1/4、LOOP 1/8、LOOP 1/16、LOOP 1/32、STUTTER、REVERSE、TAPE STOP、HALF SPEED、低通扫频、高通扫频、PHONE、BIT CRUSH、ALIAS、GATE、ECHO、TAPE WOBBLE。

### 工程、歌曲与恢复

- **手动保存工程**：SAVE → PROJECT 选择 SLOT 1–4，再执行 SAVE；LOAD 加载该槽。长按 SAVE 加白键 5–8 也能保存对应 A–D。
- **覆盖已有段落**：长按 SAVE 按对应保存键后，按屏幕提示在 3 秒内再按一次确认。
- **自动保存**：停止播放、停止操作约 2.5 秒后保存工作状态，最多每 20 秒一次。关闭电源前先停止并稍等。
- **新建工程**：SAVE → TOOLS → NEW，转到 GO；清空音序并恢复默认音轨配置。先保存需要保留的工程。
- **歌曲录制**：先保存 A–D，长按 SAVE + 白键 14 进入 SONG REC，依次调用 A–D，结束录音后停止播放使其保存。
- **歌曲回放**：长按 SAVE + 白键 13 切至 SONG，再按 PLAY。歌曲页的 OCT+ 连按两次加载段落；OCT− 切换 LOOP / SONG。
- **菜单 / About**：长按 HOME，PRESETS 移动选项，OCT+ 确认，OCT− 返回。About 显示 coreshaw、项目链接、版本、构建日期与上游来源。
- **采样文件**：Samples 支持每槽最多 16 个 WAV，文件名可带根音（如 `KEYS_C4.wav`）；选择 USR1–USR3 上传后，在 SAMPLE 的 SET 选择对应槽。
- **采样切片**：Samples 的 CHOP 打开录音，用 TAP / 空格、Find hits、Grid 或 Equal parts 切片，再 Send to USR1/2/3。上传期间等待完成。
- **MIDI**：USB MIDI 的通道 1–3 对应合成音轨，通道 10 为鼓轨。通过 MIDI 输入的声音仍由 FM-1 输出。
- **更新失败**：若设备仍在更新模式，重新打开本版安装器按提示重试；返回官方固件可参考上游手册与 M-UPGRADE。异常恢复参考 FM-1-transporter 项目，不要把不同固件的音色格式混用。

## 9. 常见问题

| 现象 | 检查方法 |
| --- | --- |
| Connect 找不到 FM-1 | 确认数据线、供电和 USB 连接；关闭 M-UPGRADE、DAW 与其他 MIDI 页面，再重新连接 |
| 浏览器不支持 MIDI / 拒绝权限 | 使用电脑 Chrome / Edge 和 HTTPS 地址，在网站权限中允许 MIDI / SysEx |
| 网页改参数但没有声音 | 音频从 FM-1 输出；检查耳机、音箱、轨道音量、静音状态，并按键触发音符 |
| PLUCK 的 PICK 没变化 | 将 EXCIT 设为 PICK，再重新触发音符 |
| ADDIT 无声 | 扩大 WIDTH、降低 FOCUS、增加 PART，并检查包络、音量与静音 |
| 刷新后浏览器音色库不见了 | 检查浏览器、网站地址是否相同；从备份 JSON 重新 Import |
| 设备重启后找不到修改的音色 | 确认使用了设备区域的 Store current sound / To device slot，而不只是浏览器 Save current sound |
| 下载的 JSON 名称有 felucca | 属于沿用的格式命名，正常；请使用对应版本编辑器读取 |
| 安装页提示固件文件缺失 | 检查仓库目录层级，保留 firmware 和 webapp 的原有路径 |

## 10. 将网站放到 GitHub Pages

1. 解压交付的 `SLOOP_ALG05_GitHub_Pages.zip`。
2. 将里面 **github-pages 文件夹的内容**上传到仓库根目录；根目录应有 `index.html`、`webapp/`、`firmware/`、`source.zip` 和 `.nojekyll`。
3. 将本文件作为根目录的 `README.md` 上传。
4. 在仓库 **Settings → Pages → Build and deployment** 选择 **Deploy from a branch**，选择 `main` 和 `/(root)`，点击 Save。
5. 等待部署完成，打开 Pages 提供的地址；编辑器在该地址后追加 `webapp/editor/`。

**source.zip 不需要解压来部署或使用网页。** 它是对应版本的源码包，只有修改源码、重新编译时才需要解压。上传网站时保留它为 ZIP 文件。上传 ZIP 本身不能代替上传已解压的网页目录。

本仓库的 Pages 地址为 https://shaw-core.github.io/Sloop_ALG02/ 。迁移仓库时请更新本说明中的链接。

## 致谢与许可

感谢 SLOOP 与 Felucca 的作者和贡献者。ALG-05 的主要新增工作是七种扩展引擎、23 个新增预设、DX7 / JSON 外部导入、详细编辑、槽位替换、分类与个人页面，以及可部署的安装包；四轨系统、鼓机、录音、歌曲、基础编辑器与更新协议来自上游。

- DX7 路由、包络、音高包络和 LFO 参考 Google Music Synthesizer for Android（Apache-2.0），保留 [Apache-2.0 许可证](docs/Apache-2.0.txt) 和来源署名。
- 应用源码：**GPL-3.0-only**。上游 [LICENSE](https://github.com/isod89/sloop-fm1/blob/main/LICENSE) 与 [LICENSING.md](https://github.com/isod89/sloop-fm1/blob/main/LICENSING.md) 可供参考；本修改版的具体文件许可见 `source.zip` 内说明。分发修改版时应保留版权、许可证并提供对应源码。
- 上游采样来自 Versilian Studios VSCO-2 CE / VCSL、Sonic Pi 等 CC0 素材；字体、SDK 等保留各自许可证。
- 上游 PHASE 引擎参考 CrispyZebra，VOICE 引擎参考 klattsch。相关作者与许可说明保留在源码中。
- 本交付版本未分发上游保留权利的设备图标图集，因此部分设备界面以文字显示；上游截图仅可作为共同功能参考。
- M-VAVE 与 FM-1 商标归其所有者；本项目不隶属于 M-VAVE，也不代表上游作者发布的官方版本。
- 本版本使用 USB MIDI，未移植官方固件的蓝牙或 USB Audio 功能。

## 开发与验证

源码包内执行 `build.sh` 编译；需要 Jieli 工具链，并设置 `JIELI_TOOLCHAIN` 与 `AC79_SDK`。执行 `tests/run_tests.sh` 做回归；`tools/export_factory_library.py` 从真实固件的预设加载过程生成 16 个引擎分类 JSON；`web/make_site.py` 生成 Pages 目录。

电脑测试包含 ALG02 / ALG03 银行与工程迁移、工厂绑定/解除/重写/跨轨加载、波形绘制、批量预检查/边界/失败停止、原有引擎声音渲染保持、新增引擎渲染、128 个 MIDI 音高、DX7 的 32 算法、VA 的 5 波形与 4 滤波模式、外部文件校验、详细数据导出 / 写入和单槽覆盖。目标代码做 RAM / pool 容量及静态指令成本检查；本环境无法测量实际 MCU 最坏 CPU 负载或 USB / 音频硬件行为，仍需真机测试。
