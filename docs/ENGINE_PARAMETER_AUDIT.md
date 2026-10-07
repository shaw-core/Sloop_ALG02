# ALG05 参数审查

对本地保留的 ALG02/SLOOP 源码与当前引擎逐文件比较：eng_analog.c、eng_digital.c、eng_phase.c、eng_lofi.c、eng_sample.c、eng_formant.c、eng_trio.c、eng_drawbar.c、eng_grain.c、eng_additive.c 相同。共同参数通过固件 DESC 枚举，不依赖网页写死每个引擎的控件。

PLUCK 的原预设回归音频未变。新增 KS 变体独立选引擎，复用内存不是减少参数。

DX7 标准单音色 155-byte VCED 转为 128-byte packed voice；网页展示 126 算子字段 + 19 全局字段。名称和算子开关不计在 145 中。公共 ADSR/滤波/效果并不是 DX7 算子四段包络的替代品。完整 DX7 字段可编辑，不代表 DSP 与 Yamaha 一致：包络曲线、键盘/力度缩放、失谐与调制深度近似；不含外部 DX7 性能控制器配置、多音模式等完整键盘操作系统。每轨 2 音，总共享声音预算 8。

VA 是本项目独立振荡器/滤波器代码，非 Baud Girl VA 二进制移植；其 preset 二进制字段尚未转换。SLOOP 的引擎名表示一种合成方式，不能当作对相应经典合成器完整参数的复刻承诺。

参考：
- https://baudgirl.com/work/FM-1+VA/manual （2026-10-06 版本96，手册说明按引擎独立参数/包络/滤波）
- https://github.com/isod89/sloop-fm1
- https://github.com/google/music-synthesizer-for-android （DSP 来源与 Apache-2.0 见 DX7_PROVENANCE.md）
- https://yamahablackboxes.com/collection/yamaha-dx7-synthesizer/patches/

验证：122 个回归渲染无非预期变化；新增自制 DX7 电钢/贝斯/铃音的 golden 已更新，SIX SINES 不变。原 10 个引擎代码对比一致。完整网页字段测试、参数/音高边界、银行 v1/v2→v3、工程 FUN4/FUN5→FUN6、波形 framebuffer 与工厂替身测试通过。硬件音色/实时性与 LCD 仍需 FM-1 实机测试。

ALG05：设备 EDIT 页接入真实输出示波器；增加 OP1–OP6 的逐算子编辑，21 项标准算子参数与 ENABLE。包络速率/电平、粗细调、模式、失谐、键盘缩放、力度与 AM 灵敏度写入原始 packed voice；修改保留邻近位字段及其他算子。设备 UI 不提供全部全局字段，网页仍提供 145 项详细字段。真实 EDIT 绘制路径、无旋钮操作时的波形刷新、六算子全部字段、保存/加载以及其他引擎隐藏 DX 专用页均有测试。DSP 渲染代码未修改。
