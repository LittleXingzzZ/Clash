# Clash 负载均衡
***
### 以订阅更新方式，使用配置文件预处理插入负载均衡配置

### 第一步：打开 `clash` → `配置`→`配置文件预处理` →中添加如下代码进行匹配 ：

```
parsers:
  - reg: 'slbable$' # 匹配的标签，编辑配置信息里面的URL结尾加上“#” 后面带上reg中的命名 例：#slbable
    yaml:
      append-proxy-groups:
        - name: ⚖️ 负载均衡-散列
          type: load-balance
          url: 'http://www.google.com/generate_204'
          interval: 300
          strategy: consistent-hashing
        - name: ⚖️ 负载均衡-轮询
          type: load-balance
          url: 'http://www.google.com/generate_204'
          interval: 300
          strategy: round-robin
      commands:
        - proxy-groups.⚖️ 负载均衡-散列.proxies=[]proxyNames  # ← 可在proxyNames后面加上 “|”  匹配对应节点，不填则默认匹配全部节点
        - proxy-groups.0.proxies.0+⚖️ 负载均衡-散列
        - proxy-groups.⚖️ 负载均衡-轮询.proxies=[]proxyNames  # 例如 “proxies=[]proxyNames” 后加上 “|HK” 匹配名称带有HK的节点
        - proxy-groups.0.proxies.0+⚖️ 负载均衡-轮询
```

### 第二步：    添加完成后：`在配置中` →`右键自己的订阅配置` → `点击设置` → `在URL中最后输入 “#” ` → `然后接着输入对应的匹配标签` → `例如：#slbable$`→ `然后确定`
### 第三步：    `接着右键订阅配置` → `选择配置文件预处理` → `查看是否已经匹配上了` → `匹配成功后点击` → `更新订阅配置就完成了`
