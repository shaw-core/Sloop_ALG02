# 外部音色来源与兼容性

ALG03 开发版：网页音色库保存在浏览器端。选中一个音色，用 **Audition / 试听** 临时加载；用 **To slot / 写入槽位** 选择 U01–U32，替换一个设备音色。导入整套库不会自动写入设备。覆盖已有槽位前会确认。电脑回归已通过；正式使用前仍需硬件试听。

| 引擎 / 来源 | 文件格式 | 当前导入状态 |
|---|---|---|
| DX7：[Yamaha Black Boxes](https://yamahablackboxes.com/collection/yamaha-dx7-synthesizer/patches/) | Yamaha 单音色 / 32 音色 `.syx` | ALG03 网页解析；检查长度、7-bit 数据和 Yamaha 校验和 |
| DX7：[DX7 patch library / Dexed_cart](https://github.com/visualizersdotnl/Yamaha-DX7-patch-library) | `.syx` | 同上；下载具体 SYX 文件，ZIP 需先解压 |
| DX7：[Bobby Blues](http://bobbyblues.recup.ch/yamaha_dx7/dx7.html) | 历史音色库和说明 | 来源参考；下载后按实际格式导入 |
| [Caskexe/DX](https://github.com/Caskexe/DX) | `.sf2`、Arturia `.dx7x` | 不支持直接导入 DX7 引擎；SF2 是采样库，DX7X 是 Arturia 格式 |
| ANALOG、FM、PHASE、CHIP、SAMPLE、VOICE、TRIO、WHEEL、GRAIN：[SLOOP](https://github.com/isod89/sloop-fm1)、[Felucca](https://github.com/hugelton/Felucca) | 网页编辑器导出的音色 / 音色库 JSON | 原生导入，按引擎名称和参数标签匹配；不支持的引擎跳过 |
| PLUCK、ADDIT、DX7、VA、KSDRUM、KSWIRE、KSCOMB：本项目导出的 JSON | SLOOP / Felucca 音色库 JSON | 原生导入；DX7 需保留 `xpatch` 128 字节数据 |
| FM：[Baud Girl FM-1+VA backup](https://baudgirl.com/work/FM-1+VA/presets) | 231 字节的专用预设帧 `.syx` | 提取备份中的 FM 部分到 DX7；VA / 未知标记跳过；不转换原版效果 / 音序 |
| VA：[Baud Girl VA pack](https://baudgirl.com/work/FM-1+VA/presets) | 专用 `.syx`，16 音色 | 尚未支持转换；不能发送给 SLOOP 的 DX7 导入通路 |
| VA：[OB-Xf patches](https://github.com/surge-synthesizer/OB-Xf/tree/main/assets/installer/Surge%20Synth%20Team/OB-Xf/Patches) | `.fxp` | 参数结构不同；作为手动音色设计参考，尚未支持转换 |
| VA / 数字合成：[Surge XT](https://surge-synthesizer.github.io/) | Surge patch | 参数结构不同；尚未支持转换 |
| SAMPLE、GRAIN：[Freesound](https://freesound.org/) | WAV / 音频采样 | 用 Samples 页上传；不是 Sound 页的参数音色导入。下载时查看各文件的使用许可 |

## 最简单的 DX7 导入方法

1. 在 Yamaha Black Boxes 页选择 Factory patches 的 **Download rom1a.syx**，或其他音色库。
2. 打开本项目 [网页编辑器](https://shaw-core.github.io/Sloop_ALG02/webapp/editor/#sound)，连接运行 ALG03 的设备。
3. 在 Sound 页按 **Import** 选择 SYX 文件，音色库会展开为 32 个独立音色。
4. 将引擎筛选切换为 **DX7**，搜索名称，例如 `E.PIANO`。
5. 选中一个音色，先 **Audition**，然后 **To slot** 选择设备槽位。已有音色会被替换，不会增加设备的槽位数量。
6. 保存项目也会保存当前轨道的 DX7 数据。想保存整个外部库，用 **Export library** 导出 JSON。

也可以将 HTTPS SYX 文件直链贴入 **Import URL**。网站页面链接、GitHub `blob` 页面、ZIP 下载链接均不是音色文件直链。GitHub 文件请使用 Raw 链接；跨站读取被网站禁止时，下载文件后用 Import。

Yamaha Black Boxes ROM1A 直链：
https://yamahablackboxes.com/patches/dx7/factory/rom1a.syx

网页不把原始外部 SysEx 直接转发给 FM-1。它先解析音色，再使用本项目协议写入选定的一个槽位；整个库留在网页端。

## 其他引擎如何获取可替换音色

- 从同一版本的 SLOOP / Felucca 编辑器导出 JSON，或使用本项目提供的按引擎分类的 JSON 音色库。导入后可逐个替换，不要求整库驻留设备。
- 用本项目编辑器设计 PLUCK / ADDIT / VA 音色，保存至网页 Library 后导出 JSON，即可放在自己的 GitHub Pages 上作为外部音色源。
- Baud Girl、OB-Xf、Surge 等引擎的预设不能仅改扩展名导入。转换需要明确参数映射，有些振荡器、调制和效果在 FM-1 上无法等价表达。网页不承诺跨引擎转换后声音完全相同。
- SAMPLE / GRAIN 使用外部音频时，先在 Samples 页写入 USR1–USR3，再编辑和保存指向这些采样槽的 JSON 音色。JSON 不包含音频本体，分享时需同时提供 WAV。

## 来源与许可

SLOOP / Felucca 项目源代码与工厂音色定义按各项目 GPL-3.0 声明使用并保留署名。Caskexe 仓库声明 Unlicense，但其中音色源自多个历史档案；它的格式与 DX7 参数导入不同。其他档案在此提供来源链接；不将其全部复制进本项目发布包。具体音色的再分发许可需按原来源说明处理。

核对日期：2026-10-05。此表区分已实现解析的格式与参考来源；链接不表示当前发布的 ALG02 已有 DX7 引擎。

FM-1+VA 备份帧布局交叉核对来源：[benny-sparra/fm1-dx7-patch-importer](https://github.com/benny-sparra/fm1-dx7-patch-importer)，特别是 `src/lib/fm1-va-preset-message.ts` 和 `docs/fm1-research.md`。VA 数据在该研究中仍被标记为未完整映射，因此不将它当作 DX7 音色播放。
