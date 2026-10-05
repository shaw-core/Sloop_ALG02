# SLOOP ALG-02

### 为 M-VAVE FM-1 增加拨弦与频谱加法合成的四轨 groovebox 固件

适用于 **M-VAVE FM-1** 的 SLOOP 自定义固件，基于 SLOOP 2.2 增加拨弦与加法合成引擎，并提供 GitHub Pages 网页安装器和音色编辑器。

- 固件界面：`SLOOP ALG02 BANK TEST`；设备标识：`FM-1_982`。
- 三条合成音轨 + 一条鼓轨，11 种合成引擎、80 个工厂音色、37 套鼓组。
- 新增 **PLUCK** 拨弦引擎和 **ADDIT** 加法合成引擎，各含 6 个预设。
- 支持音序、实时叠录、歌曲段落、用户采样、现场效果，以及 32 个设备用户音色槽。

这是独立修改的实验版本，并非 SLOOP 原作者发布的版本。本文对应随包提供的固件及编辑器；使用其他版本时，参数和保存格式可能不同。

## 本轮测试版：界面与音色管理

本轮保持 **11 个引擎、80 个工厂音色**，不新增 DX7、VA 或 Karplus–Strong 模型。更新内容：

- Sound 页新增用户音色槽快捷保存 / 替换。
- 设备 PRESETS 浏览页新增 ALL / 引擎 / USER 分类。
- Library 支持从外部 HTTPS JSON 直链导入预设。
- coreshaw 专属开机页及 About，显示项目地址并保留上游致谢。

**DX7 `.syx` 单音色与 32 音色库仍不能导入或演奏。** 六运算子核心、新 VA 与更多 KS 类型属于后续阶段，等待本轮真机测试后再加入。外部预设链接必须返回本编辑器支持的 JSON，并非任何合成器的预设都能直接转换。

### 建议先测试这五项

1. 重启，检查开机页的 coreshaw 署名与项目地址。
2. 长按 HOME → 用 PRESETS 选 ABOUT → OCT+ 进入；OCT− 返回。
3. 进入 SAVE 的 PRESETS 浏览页，用 KNOB 2 切换引擎分类，用 KNOB 1 / PRESETS 浏览该类音色。
4. 在网页修改一个音色，保存到空用户槽，再修改并替换同一槽；重启后加载检查。
5. 导出音色 JSON，重新导入浏览器库并试听；有兼容 JSON 直链时测试 URL 导入。

## 项目来源

本项目是在 **[isod89/sloop-fm1](https://github.com/isod89/sloop-fm1)** 的 SLOOP 2.2 基础上修改的版本。SLOOP 将 FM-1 改造为三条合成轨与一条鼓轨组成的现场 groovebox，提供实时录音、步进音序、歌曲段落、采样与网页编辑器。

SLOOP 本身源自 **[Felucca](https://github.com/hugelton/Felucca)**，由 Leo Kuroshita（[@kurogedelic](https://github.com/kurogedelic)）/ [Hügelton Instruments](https://hugelton.com/) 开发。原有合成引擎、音序器、编辑器与安装器的基础来自这些上游项目。

ALG-02 使用的上游基线是提交 [`f2b44c2`](https://github.com/isod89/sloop-fm1/tree/f2b44c219b8a4ac00bc06dca756cdae8a259dd1a)。上游后续更新不会自动包含在这个固件中。

## ALG-02 新增了什么

| 项目 | SLOOP 2.2 基线 | ALG-02 |
| --- | --- | --- |
| 合成引擎 | 9 种 | **11 种**，新增 PLUCK、ADDIT |
| 工厂音色 | 68 个 | **80 个**，新增 12 个 |
| 合成音轨 / 鼓轨 | 3 + 1 | 3 + 1 |
| 鼓组 | 37 套 | 37 套，沿用上游 |
| 用户音色槽 | 32 个 | 32 个，沿用上游 |
| 网页安装 | 上游固件安装器 | 随包附带 ALG-02 固件，并校验 SHA-256 |
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

### ALG-02 自定义版

项目仓库：[shaw-core/Sloop_ALG02](https://github.com/shaw-core/Sloop_ALG02)。以下为本版本的实际部署地址。

- [ALG-02 网页安装器](https://shaw-core.github.io/Sloop_ALG02/)
- [ALG-02 网页音色编辑器](https://shaw-core.github.io/Sloop_ALG02/webapp/editor/)
- [下载随仓库附带的固件](firmware/sloop-ALG02-BANK-TEST.fwsc)
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

**上游安装器安装的是原版 SLOOP，不包含 ALG-02 的 PLUCK / ADDIT。使用新增引擎请打开自己部署的 ALG-02 页面。** 编辑新增音色也建议使用随包提供的编辑器。上游手册用于参考共同功能，新增引擎请以本 README 为准。

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
4. 打开网站安装首页，确认显示的是 `FM-1_982 / SLOOP ALG02 TEST`。
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

一次只写选中的槽，不批量覆盖。设备名称为最多 12 个可打印 ASCII 字符，建议使用英文、数字或空格；中文名称请保留在浏览器库中。保存成功后数据已经写入设备用户库，无需另外按本机 SAVE。

这是替换用户槽，不是改写固件内的 80 个工厂预设。请先把工厂音色加载到当前音轨，再保存为自己的用户音色。

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

文件名或 JSON 内部出现 `felucca` 是沿用上游编辑器格式，并不表示安装了另一套固件。优先在同一版本固件之间交换音色。SLOOP 的 DIGITAL 为四运算子 FM，不能直接导入 Baud Girl / DX7 六运算子的音色文件；普通音频 WAV 也不是音色 JSON，应到 Samples 页上传。

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

**DX7 网站常见的 `.syx` 文件暂不支持。** 这类文件将在后续六运算子引擎完成后接入，不能改扩展名为 `.json` 来导入。

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

1. 解压交付的 `SLOOP_ALG02_Bank_UI_GitHub_Pages.zip`。
2. 将里面 **github-pages 文件夹的内容**上传到仓库根目录；根目录应有 `index.html`、`webapp/`、`firmware/`、`source.zip` 和 `.nojekyll`。
3. 将本文件作为根目录的 `README.md` 上传。
4. 在仓库 **Settings → Pages → Build and deployment** 选择 **Deploy from a branch**，选择 `main` 和 `/(root)`，点击 Save。
5. 等待部署完成，打开 Pages 提供的地址；编辑器在该地址后追加 `webapp/editor/`。

**source.zip 不需要解压来部署或使用网页。** 它是对应版本的源码包，只有修改源码、重新编译时才需要解压。上传网站时保留它为 ZIP 文件。上传 ZIP 本身不能代替上传已解压的网页目录。

本仓库的 Pages 地址为 https://shaw-core.github.io/Sloop_ALG02/ 。迁移仓库时请更新本说明中的链接。

## 致谢与许可

感谢 SLOOP 与 Felucca 的作者和贡献者。ALG-02 的主要新增工作是 PLUCK / ADDIT 引擎、12 个预设、对应网页支持及可部署的固件安装包；四轨系统、鼓机、录音、歌曲、基础编辑器与更新协议来自上游。

- 应用源码：**GPL-3.0-only**。上游 [LICENSE](https://github.com/isod89/sloop-fm1/blob/main/LICENSE) 与 [LICENSING.md](https://github.com/isod89/sloop-fm1/blob/main/LICENSING.md) 可供参考；本修改版的具体文件许可见 `source.zip` 内说明。分发修改版时应保留版权、许可证并提供对应源码。
- 上游采样来自 Versilian Studios VSCO-2 CE / VCSL、Sonic Pi 等 CC0 素材；字体、SDK 等保留各自许可证。
- 上游 PHASE 引擎参考 CrispyZebra，VOICE 引擎参考 klattsch。相关作者与许可说明保留在源码中。
- 本交付版本未分发上游保留权利的设备图标图集，因此部分设备界面以文字显示；上游截图仅可作为共同功能参考。
- M-VAVE 与 FM-1 商标归其所有者；本项目不隶属于 M-VAVE，也不代表上游作者发布的官方版本。
- 本版本使用 USB MIDI，未移植官方固件的蓝牙或 USB Audio 功能。
