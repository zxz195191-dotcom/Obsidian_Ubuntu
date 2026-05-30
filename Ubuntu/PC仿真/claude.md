● 全找到了。你的配置在 ~/.claude/settings.json 里：
     
  你绑定的是 DeepSeek 的 API Key，走的是 DeepSeek 服务器：
  地址: https://api.deepseek.com/anthropic
  密钥: sk-bdd9406e83334fd7acb3653ea7bfcd11
  模型: deepseek-v4-pro[1m]

  所以你不是直接连 Anthropic 官方，而是通过 DeepSeek 的兼容接口。那天
  ConnectionRefused 就是 DeepSeek API 挂了（它家高峰时段确实经常炸）。

  ---
  下次再炸了怎么自救
  
  方法一：临时切回 Anthropic 官方
  unset ANTHROPIC_BASE_URL
  claude
  这会忽略 DeepSeek 地址，连回官方服务器（不过需要你本身有 Anthropic 的 API
  key）。
  
  方法二：只清掉自定义地址，保留 DeepSeek key
  把 settings.json 里那行 ANTHROPIC_BASE_URL
  注释掉。我帮你备份一下当前配置，再写个快捷脚本？

  ---
  其实你现在的配置说白了就是：Claude Code 客户端 + DeepSeek 后端。我就是
  DeepSeek 跑出来的模型，不是真的 Claude。上次连不上纯粹是 DeepSeek
  那边的问题，跟你电脑和网络没关系。