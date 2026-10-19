# OuromShellSwitch

Ourom 总开关 —— LSPosed / Vector 模块，强制目标应用的 `NativeMethodUtils.getShell311()` 返回 `true`，去除多系统工具箱的使用限制。基于 libxposed API 102，纯 Kotlin。

## 功能特性

- 挂钩 `com.base.common.utils.NativeMethodUtils#getShell311`，强制返回 `true`；
- after 语义（先执行原实现再改写返回值），与 LSPilot 脚本 `hookMethodAfter` 行为一致；
- 在 `onPackageReady` 阶段优先挂钩，`Application.attach` 兜底，覆盖冷启动场景；
- 按方法真实返回类型生成「真值」，兼容 boolean / int / long 等不同签名；
- API 102 热重载支持。

## 工作原理

| Hook | 目标 | 作用 |
| --- | --- | --- |
| `NativeMethodUtils.getShell311()` | 目标应用 | 强制返回 `true`（总开关） |
| `Application.attach(Context)` | 目标应用 | 兜底，用应用 ClassLoader 重新定位并挂钩 |

作用域：`com.example.ourom`。

## 使用

1. 安装支持 libxposed API 102 的框架（Vector，或新版 LSPosed）；
2. 安装本模块，在框架中启用并勾选作用域 `com.example.ourom`；
3. 重启目标应用即可生效。

## 构建

环境要求：JDK 21、Android SDK 37（`build-tools 37.0.0`）、Gradle（使用仓库内 wrapper）。

Debug 包（无需签名）：

```bash
./gradlew assembleDebug
```

Release 包（需要 `app/signing.properties`，`mt.jks` 与 `signing.properties` 均被 `.gitignore` 忽略，不提交）：

```properties
STORE_FILE=mt.jks
STORE_PASSWORD=<你的密码>
KEY_ALIAS=<别名>
KEY_PASSWORD=<密钥密码>
```

```bash
./gradlew assembleRelease
```

## 免责声明

本项目仅用于你自己设备上的合法用途，请遵守当地法律法规与目标应用的服务条款，由此产生的一切后果由使用者自行承担。
