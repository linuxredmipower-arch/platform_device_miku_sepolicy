# Miku UI SePolicy — marble 适配版

Miku UI 官方 `platform_device_miku_sepolicy` 的 fork，含 marble（POCO F5, SM7475/ukee）专属适配。

## 分支

- `miku-a15` — Android 15 适配分支（本仓库主分支，上游 miku-a15 基线）
- 用户 fork：`origin_miku` = linuxredmipower-arch/platform_device_miku_sepolicy

## 相对上游的适配（全部独立 commit，可回退）

| commit | 内容 |
|--------|------|
| `c701cff` | **LOS HAL domain 恢复（r8 开机修复核心）**：IR HAL → AOSP `hal_ir_default`（file_contexts + lirc0 + vendor_ir_prop）；health → 新建 `hal_lineage_health_default`（common+qcom 合并）；touch → 官方 service_contexts 8 条 + service.te 4 type |
| `703c6e6` | touch HAL binder_call（修 add_service -129 SIGABRT） |
| `82893c4` | hal_lineage_powershare_default type（AIDL 转换后官方类型消失的补全） |

关键文件布局：`common/{public,private}` → system_ext；`common/{dynamic,vendor}` → BOARD_VENDOR_SEPOLICY_DIRS。

> ⚠️ trunk_staging 差异：`rw_dir_file` 宏已删（需展开为 allow）；sysfs 目录写全局禁止（domain.te:1482）——health 用只读。类型 vendor 化由 `qcom/sepolicy.mk` 的 BOARD_SEPOLICY_M4DEFS 映射。

## 使用

```shell
git remote add origin_miku https://github.com/linuxredmipower-arch/platform_device_miku_sepolicy.git
# 编译前确认切到 miku-a15 且包含 c701cff
```
