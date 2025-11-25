---
title: :"Mono font for Chinese and English in vscode"
author: 丛朝
date: 2025-11-25
lastmod: 2025-11-25
tags: 
type: "post"
draft: false
isCJKLanguage = true
tags: ["work", "eyecandy"]
categories: ["Life", "Tutorials"]
description: "No more misalignment"
---


## Abstract

This document provides a solution for configuring VSCode to use monospace fonts where Chinese (CJK) characters occupy exactly twice the width of English characters, ensuring proper alignment in mixed-language text.

## Introduction

When working with mixed Chinese and English text in VSCode, proper character alignment is crucial for code comments, documentation, and text formatting. True monospace fonts should maintain a 2:1 width ratio between full-width CJK characters and half-width Latin characters.

## Methods and Results

### Recommended Fonts

The following fonts provide excellent CJK and Latin character width ratio (2:1):

1. **Sarasa Mono SC (更纱黑体)** - Recommended
   - Download: <https://github.com/be5invis/Sarasa-Gothic/releases>
   - Best overall support for Chinese and English

2. **Noto Sans Mono CJK SC**
   - Download: <https://github.com/googlefonts/noto-cjk/releases>
   - Google's open-source solution

3. **Source Code Pro + Source Han Sans**
   - Adobe's professional font family

4. **JetBrains Mono** (with CJK fallback)
   - Download: <https://www.jetbrains.com/lp/mono/>

### VSCode Configuration

Add the following to your VSCode `settings.json`:

```json
{
  "editor.fontFamily": "'Sarasa Mono SC', 'Noto Sans Mono CJK SC', 'Source Han Sans', 'Microsoft YaHei Mono', monospace",
  "editor.fontSize": 14,
  "editor.fontLigatures": false,
  "editor.renderWhitespace": "all"
}
```

#### Step-by-Step Setup

1. **Install the font** (using Sarasa Mono SC as example):

   ```bash
   # For Ubuntu/Debian
   wget https://github.com/be5invis/Sarasa-Gothic/releases/latest/download/sarasa-gothic-ttf.zip
   unzip sarasa-gothic-ttf.zip -d sarasa-fonts
   sudo mkdir -p /usr/share/fonts/truetype/sarasa
   sudo cp sarasa-fonts/*.ttf /usr/share/fonts/truetype/sarasa/
   sudo fc-cache -f -v
   ```

2. **Open VSCode Settings**:
   - Press `Ctrl+Shift+P` (or `Cmd+Shift+P` on macOS)
   - Type "Preferences: Open Settings (JSON)"
   - Add the font configuration above

3. **Verify Installation**:

   ```bash
   fc-list | grep -i sarasa
   ```

### Validation Test

```text
Test alignment with various characters:
// English: ABCDEFGHIJKLMNOPQRSTUVWXYZ
// Chinese: 中文测试字符串对齐检查验证
// Mixed:   A中B文C测D试E字F符G串H对I齐
// Numbers: 0123456789 零一二三四五六七八九
```

Expected result: Each Chinese character should align exactly with two English characters.

### Visual Verification

Create a test file with the following content:

```
|--En--|--En--|
|---中---|---文---|
ABC中文XYZ
12一二34
```

If properly configured:

- "中" should align with "En--"
- Each Chinese character occupies 2 English character widths

## Discussion

### Why 2:1 Ratio Matters

In traditional terminal and programming environments, CJK characters are treated as "full-width" (占两个字符宽度), while ASCII characters are "half-width" (占一个字符宽度). This convention:

- Ensures proper alignment in ASCII art and tables
- Maintains readability in code comments
- Follows East Asian Width (EAW) specifications in Unicode Standard Annex #11

### Common Issues

1. **Font fallback problems**: If primary font lacks CJK support, VSCode may use system default, breaking alignment
   - Solution: Use font family list with CJK-aware fonts

2. **Font ligatures**: Can disrupt character width calculations
   - Solution: Set `"editor.fontLigatures": false`

3. **Variable-width fonts**: Proportional fonts don't maintain fixed ratios
   - Solution: Only use fonts explicitly labeled as "Mono" or "Monospace"

### Alternative Fonts Comparison

| Font | CJK Support | License | Width Ratio | Readability |
|------|-------------|---------|-------------|-------------|
| Sarasa Mono SC | ✅ Excellent | SIL OFL 1.1 | Perfect 2:1 | ⭐⭐⭐⭐⭐ |
| Noto Sans Mono CJK | ✅ Excellent | SIL OFL 1.1 | Perfect 2:1 | ⭐⭐⭐⭐ |
| Source Code Pro | ⚠️ Needs fallback | SIL OFL 1.1 | With fallback | ⭐⭐⭐⭐ |
| Consolas | ❌ Poor | Proprietary | Inconsistent | ⭐⭐ |

## Acknowledgements

Thanks to the open-source font community, especially:

- Be5invis for Sarasa Gothic project
- Google Fonts for Noto CJK family
- Adobe for Source Han Sans/Code Pro

## References

1. Sarasa Gothic GitHub Repository: <https://github.com/be5invis/Sarasa-Gothic>
2. Unicode Standard Annex #11 - East Asian Width: <https://www.unicode.org/reports/tr11/>
3. VSCode Font Configuration Documentation: <https://code.visualstudio.com/docs/getstarted/settings>
4. Noto CJK Fonts: <https://github.com/googlefonts/noto-cjk>
