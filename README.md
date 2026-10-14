# Miku UI SePolicy — marble 适配版

Miku UI 官方 `platform_device_miku_sepolicy` 的 fork，含 marble（POCO F5, SM7475/ukee）专属适配。

## 平台声明

- **平台**: Android 17（Cinderella = android-17.0.0_r1，LineageOS 24.0 基线铺底）
- **适配分支**: `miku-a17`（当前线）
- **基线**: Miku UI `Cinderella`
- **A16 线**: `miku-a16`（Blooming_v2 基线，2026-09-27 收工、刷机验证通过）；**A17 自上游另开，不从 A16 接续**
- **A15 线**: `miku-a15` 已冻结

## 分支

- `miku-a17` — Android 17 适配分支（当前线，上游 `Cinderella` 基线）
- `miku-a16` — Android 16 适配分支（Blooming_v2）
- `miku-a15` — Android 15 适配分支（已冻结）
- 上游跟踪：`Cinderella`（Miku-UI 官方）

## 相对上游的适配（独立 commit，可回退）

| commit | 内容 |
|--------|------|
| `d068839` | **A15 LOS HAL 配方重放**（13 文件：IR / touch / health / powershare；对应 A15 补丁 #20/#28/#32/#33/#34）。与 A16 版（`9ef560e`）差异：`202504.ignore.cil` 上游已自带（A16 的 `2910155` 无需重放）；冲突处 property.{te,contexts} 取双方内容（上游 Light + 我们 IR） |

关键文件布局：`common/{public,private}` → system_ext；`common/{dynamic,vendor}` → BOARD_VENDOR_SEPOLICY_DIRS。

> ⚠️ **A16/A15 差异备注**：A15 期 sysfs 类型的 vendor 化靠 `BOARD_SEPOLICY_M4DEFS` 映射完成，A16 起无该机制——类型直接以 `vendor_` 前缀命名；`rw_dir_file` 宏在 trunk_staging 已删（需展开为 allow）。A17 如有新变化，在适配时更新本节。

## 使用

```shell
git remote add miku-fork https://github.com/linuxredmipower-arch/platform_device_miku_sepolicy.git
git fetch miku-fork miku-a17 && git checkout -B miku-a17 miku-fork/miku-a17
```
