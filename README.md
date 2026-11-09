# iKuai IP Group Sync

爱快（iKuai）IP 组 / 运营商组自动同步工具：把公开的 IP 段清单自动写入 iKuai 的 IP 分组与自定义运营商，支持定时同步、增量对比、MQTT / Home Assistant。

> 本仓库为**分发仓库**，只提供文档与已构建的镜像，不含源代码。

## 功能

- IP 组同步：国内 IPv4/IPv6、公有云、国家/地区、GitHub 等内置源，支持自定义 URL
- 运营商组同步：电信 / 联通 / 移动 / 教育网 / 其他 ISP / IPv4 国内（写入自定义运营商，仅 IPv4）
- 自动分段：IPv4 每组 999/2500 条、IPv6 每组 999/2000 条，规避 iKuai 单组上限
- 定时同步（按星期多选 + 时分）、手动同步、预检与生成预览
- 多台路由器实例管理，V3（HTTP）/ V4（HTTPS）自动识别
- MQTT + Home Assistant 自动发现（同步状态、日志、触发同步、定时开关）
- 同步历史与日志，失败原因中文化

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

访问 `http://NAS的IP:8788`。

### 方式二：离线导入 tar 包

到 [Releases](https://github.com/ethan-z11/ikuai-ip-group-sync/releases) 下载 `ikuai-ip-group-sync.tar.gz`，然后：

```bash
gunzip -c ikuai-ip-group-sync.tar.gz | docker load
docker compose up -d
```

`docker-compose.yml` 内容同方式一。

## 首次使用

1. 打开 `http://NAS的IP:8788`，在「授权」处填入作者提供的授权码（KEY 与本机安装 ID 绑定）
2. 主页「+ 添加」绑定路由器：填内网 IP，点「测试登录」会自动识别 V3/V4 与协议
3. 「IP 组」「运营商组」页勾选需要的源，点「添加到目标」写入任务
4. 设置定时（可多选星期），保存后即可自动同步

> 说明：运营商组写入的是 iKuai「自定义运营商」，只接受 IPv4 地址段；IPv6 源请加到「IP 组」。

## 授权

- 试用期内可直接使用全部功能；到期后保留基础 IPv4 国内同步
- 授权码格式 `PAY-<安装ID>-<签名>`，与安装 ID 绑定，换机 / 删除 `data-runtime/install.json` 后需要重新获取
- 购买 / 续期请联系作者

## 目录说明

`data-runtime` 为运行时数据目录，请务必持久化挂载，包含：

| 文件 | 作用 |
|---|---|
| `install.json` | 安装 ID 与授权信息 |
| `instances.json` | 路由器实例与任务配置 |
| `history.json` | 同步历史 |
| `logs.jsonl` | 运行日志 |

---

☕ 如果这个工具帮到了你，可以请我喝杯咖啡：[点我下单](https://mall.bilibili.com/neul-next/detailuniversal/detail.html?page=detailuniversal_detail&itemsId=42660432&loadingShow=1&noTitleBar=1#noReffer=true&msource=merchant_share)
