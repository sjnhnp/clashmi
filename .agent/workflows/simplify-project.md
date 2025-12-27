---
description: ClashMi 项目精简改造计划 - 打造轻量级内核外壳UI
---

# ClashMi 精简改造计划

## 目标
将 ClashMi 改造为轻量级 Mihomo 内核外壳 UI，保留核心代理功能，移除非必要的附加功能。

## 改造原则
1. 保留：连接/断开、订阅管理、代理选择、基础设置
2. 移除：备份同步、自动更新、配置覆写、二维码扫描（可选）
3. 精简：多语言仅保留中英文、减少依赖包

---

## 第一阶段：移除备份同步功能 ✅ 已完成

### 已删除的文件
- [x] `lib/screens/backup_and_sync_icloud_screen.dart`
- [x] `lib/screens/backup_and_sync_webdav_screen.dart`
- [x] `lib/screens/backup_helper.dart`
- [x] `lib/app/utils/webdav_utils.dart`
- [x] `lib/app/utils/icloud_utils.dart`
- [x] `lib/app/utils/backup_and_sync_utils.dart`

### 已移除的依赖 (pubspec.yaml)
- [x] `webdav_client_plus` - 已注释
- [x] `icloud_storage` - 已注释

### 已清理的引用
- [x] group_helper.dart - 移除导入和简化 showBackupAndSync 函数
- [ ] 清理相关国际化字符串 (可选，暂缓)

---

## 第二阶段：移除自动更新功能

### 要删除的文件
- [ ] `lib/app/modules/auto_update_manager.dart`
- [ ] `lib/screens/version_update_screen.dart`

### 要移除的依赖
- [ ] `app_installer`

### 要清理的引用
- [ ] 从 main.dart 移除自动更新初始化
- [ ] 从关于页面移除更新检查

---

## 第三阶段：移除 Profile Patch (配置补丁) 功能

### 要删除的文件
- [ ] `lib/app/modules/profile_patch_manager.dart`
- [ ] `lib/screens/profiles_patch_board_screen.dart`
- [ ] `lib/screens/profiles_patch_board_screen_widgets.dart`
- [ ] `lib/screens/profile_patch_settings_edit_screen.dart`
- [ ] `lib/screens/add_profile_patch_by_import_from_file_screen.dart`
- [ ] `lib/screens/add_profile_patch_by_url_screen.dart`

### 要清理的引用
- [ ] profile_manager.dart 中的 patch 相关逻辑
- [ ] 设置页面中的 patch 入口

---

## 第四阶段：精简覆写配置功能 ✅ 已完成

### 已修改的文件
- [x] `lib/app/local_services/vpn_service.dart`
  - `_prepareConfig()` 函数简化，始终使用 overwrite = false
  - 移除对 ProfilePatchManager 的依赖
  - `core_path_patch` 设为空字符串
  - `prepare` 始终为 true

- [x] `lib/screens/group_helper.dart`
  - 注释掉 profile_patch_manager 和 profiles_patch_board_screen 导入
  - 简化 showClashSettings 中的 patch 相关逻辑
  - getPatchContent 始终使用 false（不覆写模式）

### 改造说明
- 用户的订阅配置现在直接传递给 Mihomo 内核
- 应用不再对配置进行复杂的覆写处理
- 保留 ProfilePatchManager 模块文件但不再使用（向后兼容）

---

## 第五阶段：移除二维码扫描功能（可选）

### 要删除的文件（如确定移除）
- [ ] `lib/screens/add_profile_by_scan_qrcode_screen.dart`
- [ ] `lib/screens/qrcode_screen.dart`
- [ ] `lib/app/utils/qrcode_utils.dart`

### 要移除的依赖
- [ ] `zxing2`
- [ ] `qr_code_scanner_plus`
- [ ] `screen_capturer`

---

## 第六阶段：清理语言文件 ✅ 已完成

### 保留
- [x] `zh-CN.i18n.json`
- [x] `en.i18n.json`
- [x] `strings_zh_CN.g.dart`
- [x] `strings_en.g.dart`

### 已删除
- [x] `ar.i18n.json`
- [x] `fa.i18n.json`
- [x] `ru.i18n.json`
- [x] `strings_ar.g.dart`
- [x] `strings_fa.g.dart`
- [x] `strings_ru.g.dart`

### 已更新
- [x] `strings.g.dart` - 移除对 ar, fa, ru 的引用

---

## 第七阶段：优化依赖

### 检查并移除未使用依赖
- 运行 `flutter pub deps --no-dev` 分析依赖
- 移除不再需要的包

---

## 执行记录

| 日期 | 阶段 | 状态 | 备注 |
|------|------|------|------|
| 2025-12-27 | 第一阶段 | ✅ 完成 | 备份同步功能移除 |
| 2025-12-27 | 第四阶段 | ✅ 完成 | 覆写配置功能精简，始终使用不覆写模式 |
| 2025-12-27 | 第六阶段 | ✅ 完成 | 语言文件精简到中英文 |
