# iKuai IP Group Sync

爱快（iKuai）IP 组 / 运营商组自动同步工具：把 IP 段清单自动写入 iKuai 的 IP 分组与自定义运营商，支持定时同步、增量对比、MQTT / Home Assistant。

> 本仓库为**分发仓库**，只提供文档与已构建的镜像，不含源代码。

## 功能

- IP 组同步：国内 IPv4/IPv6、公有云、国家/地区、GitHub 等内置源，支持自定义 URL
- 运营商组同步：电信 / 联通 / 移动 / 教育网 / 其他 ISP / IPv4 国内（写入自定义运营商，仅 IPv4）
- 自动分段：IPv4 、IPv6 
- 定时同步（按星期多选 + 时分）、手动同步、预检与生成预览
- 多台路由器实例管理，V3（HTTP）/ V4（HTTPS）自动识别
- MQTT + Home Assistant 自动发现（同步状态、日志、触发同步、定时开关）
- 试用全功能7天，7天后恢复最基础的单组默认国内ipv4和单组默认国内ipv6功能，仍然可以作为防御关。
- [请作者和咖啡](https://mall.bilibili.com/neul-next/detailuniversal/detail.html?page=detailuniversal_detail&itemsId=42660432&loadingShow=1&noTitleBar=1#noReffer=true&msource=merchant_share)可以恢复全功能，私聊客服告知客服你的ID，24小时内客服发送key。
- 选择PRO版增加adg接管功能，接管后pro项目接管维护，从而实现设备实现国内外分流目的。
  

## 部署

### 方式一：Docker Compose（在线拉取镜像）

```yaml
services:
  ikuai-ip-group-sync:
    image: ghcr.io/ethan-z11/ikuai-ip-group-sync:latest
    container_name: ikuai-ip-group-sync
    restart: unless-stopped
    ports:
      - "8788:8788"
    volumes:
      - ./data-runtime:/app/runtime
```

```bash
mkdir -p data-runtime
docker compose up -d
```

访问 `http://IP:8788`。


## 首次使用

1. 打开 `http://IP:8788`
2. 主页「+ 添加」绑定路由器：填内网 IP，点「测试登录」会自动识别 V3/V4 与协议
3. 「IP 组」「运营商组」页勾选需要的源，点「添加到目标」写入任务
4. 设置定时（可多选星期），保存后即可自动同步

> 说明：运营商组写入的是 iKuai「自定义运营商」，只接受 IPv4 地址段；IPv6 源请加到「IP 组」。

## 授权

- 试用期内可直接使用全部功能；到期后保留基础 IPv4 国内同步
- 授权码格式 `PAY-<安装ID>-<签名>`，与安装 ID 绑定，换机 / 删除 `data-runtime/install.json` 后需要重新获取
- 购买 / 续期请联系作者

## 目录说明

`data-runtime` 为运行时数据目录，请务必持久化挂载


---

☕ 如果这个工具帮到了你，可以请我喝杯咖啡：[点我下单](https://mall.bilibili.com/neul-next/detailuniversal/detail.html?page=detailuniversal_detail&itemsId=42660432&loadingShow=1&noTitleBar=1#noReffer=true&msource=merchant_share)
