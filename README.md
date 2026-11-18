# aeplugin_arm_test

在 **Apple Silicon** 上验证加壳后的 macOS 插件能否被系统正常加载。

## 结果：通过 ✅

GitHub Actions（`macos-15` = arm64，macOS 15.8.2）实测：

```
CPU: arm64
payload: Mach-O universal binary with 2 architectures: [x86_64] [arm64]
codesign: Identifier=com.tim.dropletee（ad-hoc）
nm:      000000000000e1e4 T _EffectMain
         000000000000e18c T _PluginDataEntryFunction
LOAD OK  <- arm64 上可以正常加载
symbol EffectMain: OK
symbol PluginDataEntryFunction: OK
```

即：加壳后的插件在 Apple Silicon 上**镜像能加载、入口符号都在**。

## 内容

- `Dropletee.plugin/`：加壳后的插件（stub + 加密载荷 + ad-hoc 签名）
- `.github/workflows/mac-arm64-test.yml`：自动测试
  1. 从 `.plugin` 里解出载荷
  2. `file` / `codesign -dvvv` / `lipo -archs` 检查
  3. `ctypes.CDLL` 真正 dlopen 一次，并检查 `EffectMain` / `PluginDataEntryFunction` 是否存在

## 复现

Actions 页 → `mac-arm64-load-test` → **Run workflow**。

> 注意：`macos-14` 标签在部分账号上不可用（会报
> `configuration 'macos-14-arm64-us-default' is not supported`），所以用 `macos-15`。
