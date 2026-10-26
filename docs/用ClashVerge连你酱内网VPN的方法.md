# 用Clash Verge连你酱内网VPN的方法

准备材料：[EZ4Connect](https://github.com/chenx-dust/EZ4Connect)，Clash Verge（废话，没有的话你还会来看这个教程吗）

### EZ4Connect侧

首先打开EZ4Connect，左上角「配置」→「新建配置」，命个名（推荐叫CMU之类的），然后「使用配置引导」

服务器协议选EasyConnect，下一步

地址和端口，地址写 `vpn.cmu.edu.cn`，端口就 `443` 不用变，然后下一步

认证方式选用户名和密码，下一步

帐号凭据，用户名就是你学号，密码就是你[智慧校园](https://s.cmu.edu.cn/)的密码，然后保存

保存完了之后，主页左上角「连接服务器」，会弹一个收验证码的窗口，打开你手机看一下验证码，填进去，左上角变成「已连接」了就行

完事了之后就可以关掉EZ4Connect了（注意是叉掉窗口后台运行，不要真给退了）

### Clash侧

左边「订阅」→「全局扩展脚本」

↓ 这一坨
```javascript
function main(config, profileName) {
  config.proxies = config.proxies || [];
  const node_name = "cmu_ez4c";
  if (!config.proxies.some(p => p.name === node_name)) {
    config.proxies.push({
      name: node_name,
      type: "http",
      server: "127.0.0.1",
      port: 11081,
    });
  }
  const cmu_rule = "DOMAIN-SUFFIX,cmu.edu.cn,cmu_ez4c";
  config.rules = config.rules || [];
  config.rules = config.rules.filter(r => r !== cmu_rule);
  config.rules.unshift(cmu_rule);

  return config;
}
```
↑ 粘贴进去，如果你之前在这写过东西（function里不是一行return config）那就丢给AI（deepseek chatgpt二选一，豆包千问之类的杂鱼禁用）问怎么合并。记得保存

保存完了理论上就行了
