# 用于让openai gpt系列模型说人话

## 已知问题：
中文windows系统使用codex app会默认以Get-Content -Raw不带指定编码获取skill内容，中文技能会因为乱码而不能使用。
把文件编码修改为utf-8 with bom后会导致skill无法在codex app启动时注册。

## 兼容方案
已在正文中添加了英文版。  
将AGENT.md的内容附加到Users\<用户名>\.codex\AGENT.md中效果更佳。

## 其他解决方案
使用默认采用utf-8编码的开发平台。
