# ALG03 网页安装

解压外层 ZIP，把 github-pages/ 内的文件上传到 GitHub 仓库根目录。source.zip 保持压缩，不必解压。

Settings → Pages → Deploy from a branch → main / (root)。部署后打开 webapp/installer/ 安装 FM-1_983，再打开 webapp/editor/#sound。

先导出用户音色 / 工程备份。Sound → Import 导入 DX7 SYX / JSON → 选中音色 Audition → To slot 选择 U01–U32。Quick replace 保存当前音色，覆盖会确认。

KSDRUM、KSWIRE、KSCOMB 是独立分类，原有 PLUCK 保留。Baud Girl VA 专用预设暂未转换，FM-1+VA 备份只提取 FM 音色。详细操作见 README.md。
