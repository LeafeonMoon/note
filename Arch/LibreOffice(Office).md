# LibreOffice
[參考](https://wiki.archlinux.org/index.php/LibreOffice)
[TOC]

## 安装
```bash
# libreoffice-fresh  或 libreoffice-still
# libreoffice-fresh，最新版本。
# libreoffice-still，還在維護的版本。
sudo pacman -S  libreoffice-fresh
```

## 語言安裝
安裝默認語言是 Afrikaans，因爲 LibreOffice 提供的語言包按字母排序，Afrikaans 在首位。  
```bash
# 英文，libreoffice-fresh-en-gb 或 libreoffice-still-en-gb
sudo pacman -S libreoffice-fresh-en-gb
# 簡體中文，libreoffice-fresh-zh-cn 或 libreoffice-still-zh-cn
sudo pacman -S libreoffice-fresh-zh-cn
# 繁體中文，or libreoffice-fresh-zh-tw 或  libreoffice-still-zh-tw
sudo pacman -S libreoffice-fresh-zh-tw

```