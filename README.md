# OpenAB OpenCode Zeabur Template

這個專案提供一份可部署到 Zeabur 的模板，讓你用 [OpenAB](https://github.com/openabdev/openab) 搭配 OpenCode，建立可在 Discord 中互動的 AI Bot。

模板檔案在 [openab-opencode.yaml](/Users/chenchungnien/code/openab-opencode-zeabur/openab-opencode.yaml:1)。

## 這個模板會做什麼

- 在 Zeabur 部署 `ghcr.io/openabdev/openab-opencode:0.7.7`
- 讓 Discord 訊息透過 OpenAB 接到 OpenCode ACP
- 使用 `/home/node` 作為持久化儲存
- 支援用 `OPENCODE_MODEL` 指定預設模型
- 支援用 `OPENROUTER_API_KEY` 直接提供 OpenRouter 憑證

## 需要準備的東西

部署前請先準備：

- `DISCORD_BOT_TOKEN`
- 選填：`OPENAB_ALLOWED_CHANNELS`
- 選填：`OPENAB_ALLOWED_USERS`
- 選填：`OPENCODE_MODEL`
- 選填：`OPENROUTER_API_KEY`

如果你要使用 OpenRouter，建議直接填：

- `OPENCODE_MODEL=openrouter/...`
- `OPENROUTER_API_KEY=你的 key`

這樣通常不需要再手動執行 `opencode auth login`。

## Discord 建議權限

在 Discord Developer Portal 建立 Bot 後，建議勾選這些權限：

- `檢視頻道`
- `傳送訊息`
- `建立公開討論串`
- `在討論串中傳送訊息`
- `讀取訊息歷史記錄`
- `新增反應`
- `管理訊息`

另外也要在 Bot 設定中啟用 `Message Content Intent`。

## 部署方式

1. 到 Zeabur 建立專案
2. 用 [openab-opencode.yaml](/Users/chenchungnien/code/openab-opencode-zeabur/openab-opencode.yaml:1) 部署模板
3. 填入需要的變數
4. 等服務啟動完成
5. 在 Discord 中 `@` Bot 開始對話

如果 `DISCORD_BOT_TOKEN` 留空，服務會進入 sleep，不會直接崩潰。

## 之後怎麼換模型或 API Key

如果你使用的是現在這份模板，之後大多數調整都可以直接在 Zeabur 完成。

### 更換 OpenRouter API Key

1. 到 Zeabur 更新 `OPENROUTER_API_KEY`
2. 重啟 service

### 更換模型 ID

1. 到 Zeabur 更新 `OPENCODE_MODEL`
2. 重啟 service

啟動時模板會自動把 `/home/node/opencode.json` 同步成新的 model，不需要再手動刪檔。

## 持久化檔案

- `/home/node/.config/openab/config.toml`：OpenAB 執行設定
- `/home/node/opencode.json`：OpenCode 設定，包含預設模型

如果要重建 OpenAB 設定，可以刪除：

```bash
rm /home/node/.config/openab/config.toml
```

之後重啟 service 即可。

## 常見情況

### Discord 出現 `Connection Lost`

優先檢查：

- `OPENCODE_MODEL` 是否為正確的 provider/model 格式
- 如果使用 OpenRouter，`OPENROUTER_API_KEY` 是否已填入
- service 是否已重啟

### 改了變數但沒生效

Zeabur 更新環境變數後，請記得手動重啟 service。

## 授權與來源

- OpenAB: [openabdev/openab](https://github.com/openabdev/openab)
- OpenCode 文件: [opencode.md](https://github.com/openabdev/openab/blob/main/docs/opencode.md)
- 此 repo 主要維護 Zeabur 模板與部署文件
