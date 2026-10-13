# 心与星寻 · 经典款（H5 网页版）

这是**线上 https://heart-and-stars.app.workbuddy.host/ 当前这一版**的完整网页文件
（v0.9.0），**原样提取、未改一个字节**，可直接上传 GitHub。

> 「经典款」= 原来的 H5 网页版这条产品线；不含后来那个 Streamlit 版本的任何内容。

---

## 一、这个文件夹里是什么

```
index.html          ← 入口
assets/             ← 打包后的 JS / CSS（16 个文件）
static/             ← 图片、头像、图标、manifest（33 个文件）
.nojekyll           ← 关掉 GitHub Pages 的 Jekyll 处理（重要，见下）
README.md / .gitignore
```

纯静态文件，**不需要构建、不需要安装依赖**，共约 1.2 MB。

---

## 二、⚠️ 部署前必须知道的一件事：它要放在「域名根目录」

这份构建用的是 **history 路由 + 根路径 base**，所有资源都是
`/assets/...`、`/static/...` 这种**绝对路径**。

意思是：它必须部署在 `https://某个域名/` 这个**根**上，
不能放在 `https://某个域名/子目录/` 里——放子目录会白屏。

**能用的方式（都免费）：**

| 方式 | 地址形态 | 说明 |
|---|---|---|
| 自定义域名 | `https://xinyuxingxun.cn/` | ✅ 最佳。有域名就用这个 |
| GitHub Pages「用户站点」 | `https://用户名.github.io/` | ✅ 仓库名必须叫 `用户名.github.io` |
| Vercel / Netlify / EdgeOne Pages | `https://xxx.vercel.app/` | ✅ 默认就是根目录，拖进去即可 |
| GitHub Pages「项目站点」 | `https://用户名.github.io/仓库名/` | ❌ **子目录，会白屏** |

> 如果只能用「项目站点」，就得改构建配置重新打包（在工程里设置 `base: './'`
> 再用 history 换成 hash 路由），不是改几个文件能解决的。建议直接走上面三种之一。

---

## 三、传到 GitHub

### 第 1 步：建仓库

打开 https://github.com/new ，仓库名填 **`heart-stars-classic`**，
选 **Public**，**不要**勾 Add a README，点 Create。

### 第 2 步：上传（二选一）

**方式 A：网页拖拽（最简单）**
1. 进入刚建好的空仓库页面
2. 点 **uploading an existing file**
3. 把本文件夹里的**全部内容**（`index.html`、`assets/`、`static/`、
   `.nojekyll`、`README.md`、`.gitignore`）一起拖进去
4. 下方点 **Commit changes**

> Windows 资源管理器默认不显示以 `.` 开头的文件。
> 打开「查看 → 显示 → 隐藏的项目」才能看到 `.nojekyll`，**它一定要传**。

**方式 B：命令行**
```bash
# 在本文件夹里执行
git init
git add .
git commit -m "心与星寻 经典款 H5"
git branch -M main
git remote add origin https://github.com/你的用户名/heart-stars-classic.git
git push -u origin main
```

---

## 四、上线（三条路，选一条）

### 路线 1：Vercel（最省事，推荐）

1. https://vercel.com 用 GitHub 账号登录
2. **Add New → Project** → 选 `heart-stars-classic` 仓库 → **Deploy**
3. 完事，拿到 `https://heart-stars-classic.vercel.app/`（就是根目录 ✅）

### 路线 2：Netlify

1. https://app.netlify.com → **Add new site → Import an existing project**
2. 选 GitHub 仓库 → **Build command 留空** → **Publish directory 填 `.`** → Deploy

### 路线 3：GitHub Pages（仓库名必须是 `用户名.github.io`）

1. 新建仓库时**仓库名就填 `你的用户名.github.io`**（例如 `dw.github.io`）
2. 把文件传进去
3. **Settings → Pages → Source** 选 `Deploy from a branch`，
   **Branch** 选 `main`、目录 `/ (root)`，Save
4. 等 1–2 分钟，访问 `https://你的用户名.github.io/`

> 如果你已经有自定义域名，Vercel / Netlify / GitHub Pages 都能绑定，
> 绑上之后就是根目录，完美适配。

---

## 五、已经帮你处理好的一个坑

**头像清单会被吞掉**：`static/avatars/_manifest.json` 以 `_` 开头，
而 GitHub Pages 默认会跑 Jekyll，**Jekyll 会忽略所有 `_` 开头的文件**，
头像就会加载不出来。文件夹里已放入 `.nojekyll`（空文件，存在即生效）关掉 Jekyll。

> Vercel / Netlify 不跑 Jekyll，这个文件对它们无影响，留着也无害。

---

## 六、为什么登录 / 对话在别的域名上可能用不了

网页本身能正常打开、能翻页、能看形象。

但**登录、AI 对话、记忆同步**这些要连云端服务，而云端网关**按来源域名白名单放行**：
目前只允许 `heart-and-stars.app.workbuddy.host`。换到别的域名后，
这些联网功能需要先把新域名加进白名单才能用（纯前端改不了）。

所以：**这个文件夹是「网页文件本身」的完整备份与迁移包**；
真要整套换域名上线，还需要在云端放行新域名。

---

## 七、常见问题

**Q：打开是空白 / 一直转圈**
→ 十有八九是放到了子目录。见第二节，必须放域名根目录。

**Q：图片 / 头像加载不出来**
→ `.nojekyll` 没传（GitHub Pages 把下划线文件吞了）。

**Q：能改内容吗**
→ 这个文件夹是**打包后的成品**。要改功能请回主工程 `heart-stars/`
   （uni-app 源码），改完重新构建，再把新的 `dist/build/h5/` 覆盖过来。

**Q：它和「经典款管理」什么关系**
→ 经典款是给用户用的网页；经典款管理是给你看的后台。两个独立部署，互不影响。
