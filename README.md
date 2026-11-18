# english-shadow-checkin

「英语跟读打卡」小程序的数据后端。应用本体是一个单文件 H5 页面，数据存在这台设备的浏览器里；只有在点「上传本机打卡」时才写入本仓库。

## 文件

- `data/checkins.json` —— 打卡数据库，所有人共用这一份。

## 数据结构

```json
{
  "version": 1,
  "updatedAt": "2026-11-18T02:00:00.000Z",
  "owner": "u_xxxxxx",
  "records": [
    {
      "uid": "u_xxxxxx",
      "nick": "王小明",
      "date": "2026-11-18",
      "unit": "U1",
      "done": 6,
      "total": 6,
      "avg": 88,
      "ts": 1763380000000
    }
  ]
}
```

字段说明：

- `uid` 每台设备随机生成一次，用作记录归属
- `date` 本地日期，格式 `YYYY-MM-DD`
- `done` / `total` 当天已练句数 / 总句数，`done == total` 才算当天打卡
- `avg` 当天各句得分的平均值（0-100）
- `ts` 记录更新时间戳，合并时用于取较新的一条

## 写入方式

应用通过 GitHub Contents API 读写本仓库的 `data/checkins.json`，需要一枚 Fine-grained PAT，权限只勾选本仓库的 `Contents: Read and write`。令牌只保存在使用者自己的设备浏览器里，不会写进代码或仓库。

## 多人使用

同学 / 家人各自在应用「设置」里填同一个仓库地址和各自的令牌，打卡后上传，彼此就能在「榜单」页看到打卡天数和平均分。不建议多人共用同一枚令牌。
