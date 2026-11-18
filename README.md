# aeplugin_arm_test

在 Apple Silicon（macos-14）上验证加壳后的 macOS 插件能否被系统加载。

- `Dropletee.plugin/`：加壳后的插件（stub + 加密载荷 + ad-hoc 签名）
- `.github/workflows/mac-arm64-test.yml`：自动测试流程
  （解出载荷 → 检查签名/架构 → `ctypes.CDLL` 真正加载一次）

在仓库 **Actions** 页选 `mac-arm64-load-test` → **Run workflow** 即可。
