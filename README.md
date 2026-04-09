# 用于让openai gpt系列模型说人话

## 已知问题：
中文windows系统使用codex app会默认以Get-Content -Raw不带指定编码获取skill内容，中文技能会因为乱码而不能使用。
把文件编码修改为utf-8 with bom后会导致skill无法在codex app启动时注册。

## 兼容方案
将AGENT.md的内容追加到你的Users\<用户名>\.codex\AGENT.md中，约束agent使用utf8来读取skill。

## 其他解决方案
使用默认采用utf-8编码的开发平台。
