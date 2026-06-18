# CRBMiner

**Cereblix (CRB) 官方 NeuroMorph CPU 矿工** · 矿池 **[pool.tutuit.xyz](https://pool.tutuit.xyz)**

CRBMiner 是专为 Cereblix（CRB / NeuroMorph `nm/1`）优化的 CPU 矿工。**开箱即用**——以 root 运行时自动启用 **1 GiB 大页 / performance governor / NUMA 本地数据集**，无需手动调优。在未手动调优的环境下，比同样未调优的 SRBMiner **快约 10–25%**。

> ⚠️ 抽水：内置 **3% 开发者手续费**（流向 Cereblix 运营方钱包，与上游 XMRig 的 donate 机制同性质）。

---

## 下载（仅 Linux x86-64）

→ **[最新版本 Releases](https://github.com/scashcc/crbminer/releases/latest)**

```bash
wget https://github.com/scashcc/crbminer/releases/latest/download/crbminer
chmod +x crbminer
```

- **SHA-256**：`5e5c5931603b694f2bcad5baec5a91bace0a42bf92e9ec5059f601e576d665b0`
- 构建环境：Ubuntu 22.04 / glibc ≥ 2.35，动态链接。缺库先装：
  ```bash
  sudo apt install -y libuv1 libhwloc15 libssl3
  ```

## 运行

把 `YOUR_crb1_address` 换成你自己的 CRB 钱包地址：

```bash
sudo ./crbminer -o hk.tutuit.xyz:2100 -a nm/1 -u YOUR_crb1_address -p x --cpu-no-yield
```

- **`sudo`（root）必需**：自动锁大页、设 governor、NUMA 绑核。普通用户也能挖，但吃不到自动调优。
- 算法固定 `nm/1`（NeuroMorph）。
- 线程默认 = 全部逻辑核。**大核 EPYC 保持全 SMT 最佳，别加 `-t`。**

### 矿池节点（就近选，端口相同）

| 区域 | 地址 |
|---|---|
| 🇭🇰 香港 | `hk.tutuit.xyz` |
| 🇰🇷 韩国 | `ko.tutuit.xyz` |
| 🇯🇵 日本 | `jp.tutuit.xyz` |
| 🇺🇸 美国 | `us.tutuit.xyz` |

端口：`2100` / `2200` = PPLNS，`2500` / `2600` = SOLO（高算力选高端口）。

### 大页（可选，进一步榨性能）

EPYC 等大核机器，提前预留 1 GiB 大页让数据集吃满：

```bash
echo 8    | sudo tee /sys/kernel/mm/hugepages/hugepages-1048576kB/nr_hugepages
echo 4096 | sudo tee /proc/sys/vm/nr_hugepages
```

没预留会自动回退 2 MiB 大页，不报错。

---

## 性能

- **AMD EPYC（Zen4，如 9654）**：与 SRBMiner 同档，实测略超。
- **开箱即用**（自动大页 / governor / NUMA）对比**未手动调优**的 SRBMiner：快约 **10–25%**。

## 许可证 / 源码

CRBMiner 基于 **GPLv3** 的 `xmrig`（NeuroMorph 分叉）。完整对应上游源码见 **https://cereblix.com/xmrig-cereblix-src.tar.gz**；本项目的修改版源码可向运营方索取（在本仓库开 issue 即可）。

---

**English** — CRBMiner is the official NeuroMorph CPU miner for Cereblix (CRB). Run as root for automatic 1 GiB hugepages / performance governor / NUMA tuning; out of the box it runs ~10–25% faster than an un-tuned SRBMiner. Built-in 3% dev fee. Linux x86-64 only (glibc ≥ 2.35). GPLv3 (xmrig fork) — corresponding source at https://cereblix.com/xmrig-cereblix-src.tar.gz.
