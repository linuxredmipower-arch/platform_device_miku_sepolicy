# Miku UI SePolicy — marble 适配版

Miku UI 官方 `platform_device_miku_sepolicy` 的 fork，含 marble（POCO F5, SM7475/ukee）专属适配。

## 平台声明

- **平台**: Android 16（Blooming_v2 = android-16.0.0_r4 + LineageOS 23.2, userdebug）
- **适配分支**: `miku-a16`（当前线）
- **版本标记**: tag `a16`（2026-09-27 打标）
- **基线**: Miku UI `Blooming_v2`
- **A15 线**: `miku-a15` 已冻结（A15 出包验证通过）。两线**已分叉**——`miku-a16` 自上游另开，不是从 `miku-a15` 接续（领先 4 个提交 / 落后 5 个），所以 A15 那版 README 声明没有跟过来

## 分支

- `miku-a16` — Android 16 适配分支（当前线，上游 `Blooming_v2` 基线）
- `miku-a15` — Android 15 适配分支（已冻结，r8 开机修复线）
- 上游跟踪：`Blooming_v2`（Miku-UI 官方）

## 相对上游的适配（全部独立 commit，可回退）

| commit | 内容 |
|--------|------|
| `9ef560e` | **A15 配方移植**（9 新增 + 5 修改文件，对应 A15 补丁 #20/#28/#32/#33/#34）；sysfs 类型改 `vendor_` 前缀（A16 无 M4DEFS 映射机制） |
| `2910155` | **`202504.ignore.cil`**：`xtra_control_prop` 是本仓库自加的 property，上游 compat 只到 202404 → treble 校验失败。按 34.0/202404 同款 `new_objects` ignore 模式补 202504 映射 |

关键文件布局：`common/{public,private}` → system_ext；`common/{dynamic,vendor}` → BOARD_VENDOR_SEPOLICY_DIRS。

> ⚠️ **A16 差异**：A15 期 sysfs 类型的 vendor 化靠 `qcom/sepolicy.mk` 的 `BOARD_SEPOLICY_M4DEFS` 映射完成，**A16 已无该机制**——类型改为直接以 `vendor_` 前缀命名。另：`rw_dir_file` 宏在 trunk_staging 已删（需展开为 allow）。

## 使用

```shell
git remote add miku-fork https://github.com/linuxredmipower-arch/platform_device_miku_sepolicy.git
git fetch miku-fork miku-a16 && git checkout miku-a16
```
