

# 运行
终端进入该项目
1. yarn
2. yarn tauri dev # 本地开发
3. yarn tauri build #本地打包，根据当前系统(windows, mac, linux)打包对应的安装包，在windows虚拟机中需要安装rust环境
mac中打包-参考链接： https://github.com/lencx/tauri-tutorial/discussions/5
# 提交项目：
参考链接1： https://github.com/lencx/tauri-tutorial/discussions/8
参考链接2： https://blog.csdn.net/mouday/article/details/129811003
1. git add .
2. git commit -m "feat: .."
(如果不想发版本，只是更新代码的话，执行1，2，git push)
3. git tag v0.1.0 // 提交版本号
4. git push --tag // 推送到仓库并自动根据 .github/workflows/release.yml中的配置构建相应版本的包，会打出三个平台(windows, mac, linux)的安装包

## 通过gitaction打包出来的 .dmg安装包，在mac中安装时提示 （因为无法验证开发者，开发者身份不明的问题）
## 在mac环境中执行 yarn tauri build 打出来的包，没有这个问题，可以直接在mac中安装并运行
解决：在使用GitHub Actions构建Tauri应用的dmg安装包时,出现无法验证开发者身份的问题,主要是因为dmg缺少开发者签名所致。可以通过以下步骤在打包时给dmg添加开发者签名来解决:

1. 获取苹果开发者ID证书
登录Apple Developer网站,在Account下的Certificates, IDs & Profiles中创建一个新的Mac App Distribution证书。
下载并安装到钥匙串访问中的登录钥匙串。
2. 创建签名文件
将证书导出为 .p12 文件,并提取私钥到一个变量中。
使用私钥生成签名文件:
`openssl x509 -in cert.pem -inform pem -signkey key.pem -out sig.pem`
3. 在GitHub Actions中使用签名文件
设置ACTIONS_CERTIFICATE和ACTIONS_CERTIFICATE_PASSWORD环境变量。
安装app-notarize cargo crate。
在构建Tauri应用后,使用cargo app-notarize给dmg添加签名:
`cargo app-notarize target/release/app.dmg --certificate "$ACTIONS_CERTIFICATE" --password "$ACTIONS_CERTIFICATE_PASSWORD"`
这将给构建好的Tauri dmg文件添加开发者签名,解决无法验证开发者身份的问题。









# Tauri + Vue 3 + TypeScript

This template should help get you started developing with Vue 3 and TypeScript in Vite. The template uses Vue 3 `<script setup>` SFCs, check out the [script setup docs](https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup) to learn more.

## Recommended IDE Setup

- [VS Code](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) + [Tauri](https://marketplace.visualstudio.com/items?itemName=tauri-apps.tauri-vscode) + [rust-analyzer](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer)

## Type Support For `.vue` Imports in TS

Since TypeScript cannot handle type information for `.vue` imports, they are shimmed to be a generic Vue component type by default. In most cases this is fine if you don't really care about component prop types outside of templates. However, if you wish to get actual prop types in `.vue` imports (for example to get props validation when using manual `h(...)` calls), you can enable Volar's Take Over mode by following these steps:

1. Run `Extensions: Show Built-in Extensions` from VS Code's command palette, look for `TypeScript and JavaScript Language Features`, then right click and select `Disable (Workspace)`. By default, Take Over mode will enable itself if the default TypeScript extension is disabled.
2. Reload the VS Code window by running `Developer: Reload Window` from the command palette.

You can learn more about Take Over mode [here](https://github.com/johnsoncodehk/volar/discussions/471).
