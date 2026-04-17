# Repository Guidelines

## 项目结构与模块组织
- `app/` 是唯一的 Android 应用模块；核心代码位于 `app/src/main/java/com/kooo/evcam`。
- 业务按领域拆分到 `camera/`、`recording/`、`remote/`、`dingtalk/`、`feishu/`、`telegram/`、`transfer/`、`service/` 等包。
- 资源文件位于 `app/src/main/res`，本地单元测试在 `app/src/test`，设备测试在 `app/src/androidTest`。
- 根目录 `assets/` 通过 `app/build.gradle.kts` 中的 `assets.srcDir("../assets")` 打包进 APK；原生库位于 `app/src/main/jniLibs/arm64-v8a`。

## 构建、测试与开发命令
- `gradlew.bat assembleDebug`：构建调试包。
- `gradlew.bat assembleRelease`：构建签名 Release 包。
- `gradlew.bat installDebug`：安装调试包到已连接设备。
- `gradlew.bat test`：运行 JUnit4 本地单元测试。
- `gradlew.bat connectedAndroidTest`：运行 AndroidX/Espresso 设备测试。
- `release.bat`：读取 `versionName`、构建 Release、创建 Git Tag，并尝试发布 GitHub Release。运行前先提交代码并确认 `gh` 已登录。
- 本地构建环境按 README 使用 JDK 17+；源码保持 Java 11 兼容。

## 编码风格与命名规范
- 使用 4 空格缩进，沿用现有 Java 风格，不混入 Kotlin 风格写法。
- 类名使用 `UpperCamelCase`，方法和字段使用 `lowerCamelCase`，资源文件使用 `snake_case`，例如 `fragment_playback_new.xml`。
- 新功能优先放入已有领域包，避免把摄像头、上传、远程控制逻辑继续堆在根包。
- 新增抛错信息、日志提示和面向用户的文案统一使用中文；仅在相机生命周期、线程切换或远程命令链路等不直观位置补充简洁注释。

## 测试指南
- 纯逻辑改动优先补 `app/src/test/.../*Test.java`；依赖 `Context`、UI 或设备能力的测试放入 `app/src/androidTest`。
- 仓库当前仅有示例测试，没有强制覆盖率门槛；涉及相机编排、录制分段、上传、存储清理时，至少补充针对性测试或在 PR 中写清真机验证结果。
- 运行设备测试时，请记录机型、Android 版本、摄像头数量或所用车型配置。

## 提交与 Pull Request 规范
- 现有提交历史以中文短句为主，也接受 `feat:` 这类前缀；推荐格式：`fix: 修复补盲鱼眼矫正失效` 或直接使用清晰中文动宾短句。
- 一次提交只处理一类问题；版本号、签名、发布脚本调整应单独提交。
- PR 至少包含：变更摘要、影响模块、执行过的命令、真机验证结论。涉及界面、悬浮窗、预览布局时附截图或录屏。

## 安全与配置提示
- 不要提交真实机器人 Token、`local.properties`、临时日志或设备专属配置。
- `keystore/` 与 `app/build.gradle.kts` 中的签名配置属于发布链路，非必要不要改动；如需调整，请在 PR 中说明原因、风险和回滚方式。
