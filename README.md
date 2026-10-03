# 味之轻舟 - 用户端微信小程序（graduation-weixin）

![项目](https://img.shields.io/badge/项目-味之轻舟-E95F3C?style=flat-square) ![角色](https://img.shields.io/badge/角色-用户端小程序-07C160?style=flat-square) ![uni-app](https://img.shields.io/badge/uni--app-框架-2F8BEB?style=flat-square) ![微信小程序](https://img.shields.io/badge/微信小程序-MiniProgram-07C160?logo=wechat&logoColor=white&style=flat-square) ![uni-ui](https://img.shields.io/badge/uni--ui-组件库-FF6B6B?style=flat-square)

> 🚀 **「味之轻舟」** —— 一个完整的餐饮外卖点餐系统毕业设计项目，由 **后端服务 + 管理端 + 微信小程序** 三端组成。

## 🌐 项目生态（相关仓库）

本仓库是「味之轻舟」系统的**用户端（微信小程序）**。完整系统由以下三个仓库组成，点击链接可跳转：

| 模块 | 说明 | 技术栈 | 仓库 |
| :---: | --- | --- | :---: |
| ⚙️ 后端服务 | 系统核心（RESTful API + WebSocket） | Spring Boot / MyBatis / MySQL / Redis / JWT | [`graduation-backend`](https://github.com/buqingli666/graduation-backend) |
| 📊 管理端 | 商家后台（订单 / 菜品 / 统计） | Vue2 / TypeScript / Element UI / ECharts | [`graduation-vue`](https://github.com/buqingli666/graduation-vue) |
| 🛒 **用户端** | 小程序点餐端 | uni-app / 微信小程序 / uni-ui | [`graduation-weixin`](https://github.com/buqingli666/graduation-weixin) ⬅️ **当前** |

---

## 一、项目简介

本仓库是「味之轻舟」系统的用户端微信小程序，对接后端服务（`graduation-backend`）的 `/user/**` 接口。用户可通过小程序完成：

- **微信登录**（`wx.login` 获取 code → 后端换取 openid 完成登录注册）。
- **浏览点餐**：查看分类、菜品、套餐，加入购物车。
- **下单支付**：选择收货地址、填写备注、提交订单、调用微信支付。
- **地址管理**：新增 / 编辑 / 删除收货地址，设置默认地址。
- **订单管理**：查看当前订单、历史订单、订单详情、再来一单。
- **消息提示**：来单 / 催单等提示。

> 📌 说明：本仓库为 **uni-app 编译后的微信小程序产物**（包含 `common/runtime.js`、`common/vendor.js`、`common/main.js` 等 webpack 打包文件）。请使用 [微信开发者工具](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html) 导入并运行调试。

## 二、技术栈

| 分类 | 说明 |
| --- | --- |
| 开发框架 | uni-app（Vue 语法，编译为微信小程序） |
| 运行平台 | 微信小程序 |
| UI 组件 | uni-ui（uni-icons、uni-nav-bar、uni-popup、uni-easyinput、uni-list、uni-badge、uni-transition 等） |
| 网络请求 | 基于 `uni.request` / `wx.request` 封装 |
| 支付 | 微信小程序支付 |
| 云能力 | `.cloudbase`（腾讯云 CloudBase 容器配置） |

## 三、项目结构

```
graduation-weixin/
├── app.js                        # 小程序入口（加载打包后的运行时与业务代码）
├── app.json                      # 小程序全局配置（页面路由、窗口样式）
├── app.wxss                      # 全局样式
├── pages/                        # 业务页面
│   ├── index/                    # 首页（菜品/套餐浏览、分类）
│   ├── order/                    # 下单页（购物车、确认订单）
│   ├── details/                  # 订单详情
│   ├── pay/                      # 支付页
│   ├── success/                  # 支付成功页
│   ├── address/                  # 收货地址列表
│   ├── addOrEditAddress/         # 新增 / 编辑收货地址
│   ├── remark/                   # 订单备注
│   ├── my/                       # 个人中心
│   ├── historyOrder/             # 历史订单
│   └── nonet/                    # 无网络提示页
├── components/                   # 自定义 / 第三方组件
│   ├── empty                     # 空状态
│   ├── reach-bottom              # 上拉加载
│   ├── uni-icons / uni-nav-bar / uni-popup / uni-phone / uni-piker / uni-status-bar
├── common/                       # webpack 打包运行时（runtime/vendor/main）
├── static/                       # 静态图片资源
├── uni_modules/                  # uni-app 插件模块（uni-ui 组件）
├── node-modules/                 # 依赖
├── .cloudbase/                   # 腾讯云 CloudBase 容器配置
├── project.config.json           # 微信开发者工具项目配置
├── project.private.config.json   # 项目私有配置
└── sitemap.json                  # 小程序索引规则
```

### 页面路由（app.json）

```json
"pages": [
  "pages/index/index",
  "pages/order/index",
  "pages/details/index",
  "pages/pay/index",
  "pages/success/index",
  "pages/nonet/index",
  "pages/address/address",
  "pages/remark/index",
  "pages/my/my",
  "pages/addOrEditAddress/addOrEditAddress",
  "pages/historyOrder/historyOrder"
]
```

## 四、快速开始

### 环境要求

- [微信开发者工具](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html)（稳定版）
- 一个微信小程序账号（或使用「测试号」），并配置 **服务器域名**（生产环境需在公众平台配置 request 合法域名）

### 1. 配置后端接口地址

由于本仓库为编译产物，接口地址在打包时已写入代码。如需修改指向本地后端，请：

- 在 uni-app 源工程中修改接口 `baseUrl`（指向 `http://localhost:8080`）后重新编译；
- 或在微信开发者工具中关闭「不校验合法域名」（本地调试）。

### 2. 导入项目

1. 打开微信开发者工具，选择「导入项目」。
2. 选择本仓库根目录作为项目目录。
3. 填写 **AppID**（可使用测试号，或替换为您自己的 AppID）。
4. 导入后在「详情 → 本地设置」中勾选 **不校验合法域名、web-view（业务域名）、TLS 版本以及 HTTPS 证书**（仅本地调试）。

### 3. 启动后端服务

确保后端服务 `graduation-backend`（端口 8080）已正常运行，并已正确配置小程序的 `appid` / `secret` 与微信支付参数。

### 4. 编译预览

在微信开发者工具中点击「编译」即可在模拟器中预览，或点击「预览」扫码在真机调试。

## 五、业务流程

```
微信登录(wx.login)
   └─> 浏览首页(分类/菜品/套餐)
         └─> 加入购物车
               └─> 确认订单(选择收货地址 + 备注)
                     └─> 微信支付
                           ├─ 成功 -> success 页
                           └─ 查看订单/历史订单/订单详情
```

## 六、相关仓库

本项目为「味之轻舟」系统的用户端，配套仓库：

- 后端服务：[graduation-backend](https://github.com/buqingli666/graduation-backend)
- 管理端（Vue）：[graduation-vue](https://github.com/buqingli666/graduation-vue)

## 七、注意事项

1. 本仓库为 **uni-app 编译产物**，若需深度二次开发，建议基于 uni-app 源工程进行修改后重新编译。
2. 生产上线前，请在微信公众平台配置 **request 合法域名** 与 **微信支付商户号** 等参数。
3. 小程序涉及微信支付，需完成微信支付商户接入与后端 `notifyUrl` 回调联调。
