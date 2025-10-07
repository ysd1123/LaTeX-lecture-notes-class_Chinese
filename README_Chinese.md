# `lecturenotes` 中文使用说明

本仓库基于上游 [`vhbelvadi/LaTeX-lecture-notes-class`](https://github.com/vhbelvadi/LaTeX-lecture-notes-class) 项目进行维护，当前 fork 的额外修改包括：

- 新增 `lecturenotes-chinese.def` 补丁文件，通过 `ctex` 扩展 `lecturenotes.cls` 的中文排版支持，并在中文模式下提供可选的中西文字体及数学字体设置。
- 在 `lecturenotes.cls` 中引入中文语言选项 `chinese`，并为该选项接入补丁文件，同时保持其他语言的行为不变。
- 编写本中文 README，帮助简体中文用户快速上手。

## 安装与编译环境

- 推荐使用 **TeX Live 2025**（Windows 11 环境下测试通过）。
- 编译方式：使用 `latexmk -xelatex`（XeLaTeX 引擎），以便充分利用 `ctex` 与 OpenType 字体。
- 请将 `lecturenotes.cls` 与 `lecturenotes-chinese.def` 放在同一目录（或用户级 TEXMF 树的同一路径）下，以便类文件自动找到补丁文件。

## 启用中文模式

在文档导言区使用：

```latex
\documentclass[chinese]{lecturenotes}
```

启用后，类文件将自动：

1. 载入 `ctex`（使用 `scheme=plain`，保持与原有版式兼容）。
2. 切换为中文日期、章节、定理等标题文本。
3. 关闭原本基于 `babel` 的语种设置，以避免与 `ctex` 冲突。

> **提示**：若需要混排其他语言，可继续使用原有的 `\notes@...` 命令或 `babelbib` 配置（中文模式下默认关闭 `babel`，如需重新启用请手动在导言区加载并调整）。

## 中文模式下的可选字体设置

默认情况下，类文件沿用 `ctex` 与 `lecturenotes` 的默认字体方案，无需额外设置即可编译。若希望自定义字体，可在 `\documentclass` 的可选参数或导言区使用以下接口：

### 通过类选项设置（推荐写法）

```latex
\documentclass[
  chinese,
  zhmainfont=Source Han Serif SC,
  zhsansfont=Source Han Sans SC,
  zhmonofont=Sarasa Mono SC,
  latinfont=TeX Gyre Pagella,
  mathfont=STIX Two Math
]{lecturenotes}
```

各选项含义如下：

- `zhmainfont`：正文中文主字体（`\setCJKmainfont`）。
- `zhsansfont`：无衬线中文字体（`\setCJKsansfont`）。
- `zhmonofont`：等宽中文字体（`\setCJKmonofont`）。
- `latinfont`：西文主字体（`\setmainfont`）。
- `mathfont`：数学字体（`\setmathfont`，需与 `unicode-math` 兼容）。

### 通过导言区命令设置（可与类选项混合使用）

```latex
\notesetCJKmainfont{FZShuSong-Z01}
\notesetCJKsansfont{FZHei-B01}
\notesetCJKmonofont{Fira Code}
\notesetLatinfont{Times New Roman}
\notesetMathfont{XITS Math}
```

这些命令在 `\begin{document}` 前调用即可，若同时设置了类选项，以最后一次赋值为准。

> **注意**：请确保系统已正确安装所指定的字体名称，并使用 XeLaTeX 编译；如字体缺失，编译将报错。

## 与上游仓库同步的注意事项

- `lecturenotes.cls` 仅加入了最少量的钩子（加载补丁文件、条件关闭 `babel` 等），以降低与上游未来变更产生冲突的风险。
- 中文相关逻辑集中在独立的 `lecturenotes-chinese.def` 中，后续跟进上游更新时只需关注补丁文件是否仍然适配。

## 常见问题

1. **启用中文后仍出现乱码？**
   - 确认是否使用 `latexmk -xelatex` 或直接 `xelatex` 编译；`pdflatex` 无法处理 UTF-8 中文。
2. **自定义字体不生效？**
   - 检查字体名称是否与系统中安装的字体完全一致，必要时可使用 `fc-list`（或 Windows 字体设置界面）确认。
   - 若类选项与命令同时设置，后执行的设置会覆盖先前设置。
3. **需要混排英文或其他语言？**
   - 直接在正文中书写英文或公式即可；若需额外的多语言断词，可手动加载 `babel` 并调整顺序，但建议仅在熟悉 `ctex` 与 `babel` 协同配置的情况下尝试。

## 参考

- [CTeX 宏集手册](https://ctex.org/documents)
- [XeLaTeX 字体管理指南（TeX Live 文档）](https://tug.org/texlive/doc/texlive-en/texlive-en.html)
- LaTeX 社群常见问题讨论（TeX Stack Exchange、CTeX 论坛等）

如在使用过程中发现问题，欢迎在仓库的 Issue 区反馈。
