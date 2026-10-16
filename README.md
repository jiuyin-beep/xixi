# NFC 音频贴片 —— 部署指南

目标：把一段音频变成一个**网址**，写入 NFC 贴片，手机贴近即可打开并播放。

> 原理：NFC 贴片容量只有几百字节，存不下音频，只能存一个网址。音频放在公网（GitHub Pages），网址写进贴片。

---

## 第一步：放入音频文件

1. 把音频文件放到本目录，命名为 `audio.mp3`（与 `index.html` 同级）。
   - 推荐格式：**mp3** 或 **m4a**（iOS / Android 都能播放）。
   - 不要用 `ogg`（iPhone 不识别）；`wav` 可以但体积大、加载慢。
2. 如果文件名不是 `audio.mp3`，编辑 `index.html` 顶部的 `AUDIO_SRC = "..."` 改成实际文件名。
3. （可选）想循环播放，把 `index.html` 顶部的 `const LOOP = false` 改成 `true`。

## 第二步：发布到 GitHub Pages

1. 打开 https://github.com 注册/登录，点右上角 **+ → New repository**。
2. 仓库名随意，比如 `nfc-audio`，选 **Public**（公开，免费），点 **Create repository**。
3. 把本目录三个文件（`index.html`、`audio.mp3`、`README.md`）上传到该仓库：
   - 网页方式：仓库页点 **Add file → Upload files**，把文件拖进去后 **Commit changes**。
   - 或用命令行（见下方"命令行上传"）。
4. 打开仓库 **Settings → Pages**：
   - **Source** 选 **Deploy from a branch**；
   - **Branch** 选 **main**、目录选 **/ (root)**，保存。
5. 稍等 1～2 分钟，Pages 页面顶部会出现网址，形如：
   `https://你的用户名.github.io/nfc-audio/`
6. 用手机浏览器打开这个网址，确认能播放音频。✅ 这就是要写进贴片的网址。

### 命令行上传（可选，需安装 Git）

```bash
git init
git add index.html audio.mp3 README.md
git commit -m "nfc audio"
git branch -M main
git remote add origin https://github.com/你的用户名/nfc-audio.git
git push -u origin main
```

## 第三步：把网址写进 NFC 贴片

1. 手机安装 **NFC Tools**（iOS 商店 / 应用市场均有，免费）。
2. 打开 NFC Tools → **写** → **添加记录** → 选择 **URL / URI**。
3. 输入网址（形如 `https://你的用户名.github.io/nfc-audio/`），确认。
4. 点 **写**，把手机背面贴近 NFC 贴片，几秒即写入完成。
5. 测试：手机贴近贴片 → 自动弹出浏览器 → 打开页面播放音频。

## 常见问题

- **贴片后手机没反应**：确认手机 NFC 已开启；贴片贴近手机背面顶部（iPhone 是顶部）。
- **打开网页但不自动播放**：手机浏览器普遍禁止"无操作自动播放声音"，页面会显示大播放按钮，点一下即可。这是系统限制，无法绕过。
- **想换音频**：替换 `audio.mp3`，再 Commit 上传，网址不变，贴片无需重写。
- **贴片容量**：NTAG213 ≈ 144 字节可用、NTAG215 ≈ 504、NTAG216 ≈ 888。你的网址一般只有几十字节，最小贴片也够用。
