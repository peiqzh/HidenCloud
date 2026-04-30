# Streamlit 应用自动唤醒工具

本工具通过 GitHub Actions 定时或手动触发，配合 Playwright 自动化脚本，访问 Streamlit 应用并处理其休眠（Sleep）状态，确保应用保持活跃或快速唤醒。

> ⭐ 觉得有用？给个 Star 支持一下！  
> 官网：[https://streamlit.io](https://share.streamlit.io/)

## 前置要求

- GitHub 仓库拥有 Actions 权限（公开或私有仓库均可）
- 目标 Streamlit 应用部署在可公开访问的 URL（如 Streamlit Community Cloud）
- 如需使用 API 方式调用，需要 GitHub 个人访问令牌（具备 `repo` 和 `workflow` 权限）

---

## 使用方法

### 方式一：GitHub 网页手动触发

1. **进入仓库页面**  
   打开你的 GitHub 仓库，点击顶部的 **Actions** 标签页。

2. **选择工作流**  
   在左侧工作流列表中找到 **“Streamlit 自动唤醒”**，点击进入该工作流的详情页面。

3. **手动触发**  
    - 点击右上角的 **“Run workflow”** 下拉按钮。  
    - 在弹出的对话框中，可以填写以下参数：  
        - **目标链接**：需要唤醒的 Streamlit 应用 URL（留空则使用工作流中配置的默认链接）。  
    - 点击绿色的 **“Run workflow”** 按钮即可启动一次唤醒任务。

    执行完成后，可以在同一工作流页面查看运行日志。

### 方式二：API 调用

通过 GitHub REST API 触发工作流，适合集成到其他自动化系统（如 CI/CD、定时脚本等）。

#### 请求示例

```bash
curl -X POST \
  https://api.github.com/repos/你的用户名/仓库名/actions/workflows/Streamlit-Keep.yml/dispatches \
  -H "Authorization: token ghp_你的令牌" \
  -d '{"ref":"main","inputs":{"target_url":"https://你的应用.streamlit.app"}}'
```

| 参数               | 类型   | 必填 | 说明                         |
| ------------------ | ------ | ---- | ---------------------------- |
| `ref`              | string | 是   | 分支名称，如 `main`          |
| `inputs.target_url`| string | 是   | 目标 Streamlit 应用 URL      |

#### 获取令牌

- 在 GitHub 的 `Settings → Developer settings → Personal access tokens → Tokens (classic)` 中生成令牌。
- 所需权限：`repo`（完整仓库控制）或至少 `workflow` 权限。

---

## 注意事项

1. **Streamlit Cloud 休眠机制**  
   Streamlit Community Cloud 的免费层会在应用无访问流量约 30-50 分钟后自动休眠。本工具通过定期访问并点击唤醒按钮来保持活跃，但请遵守 Streamlit 服务条款，避免过度频繁调用。

2. **GitHub Actions 使用限制**  
   - 公开仓库：几乎无限制  
   - 私有仓库：每月 2000 分钟免费（具体取决于订阅计划）  
   请根据唤醒频率合理配置定时任务，避免超出额度。

3. **日志调试**  
   脚本中多处使用 `sys.stdout.flush()` 确保日志实时输出。在 Actions 页面可实时查看打印信息，便于排查问题。

---

## ⚠️ 免责声明

本项目仅供学习研究使用。使用本脚本产生的任何后果由使用者自行承担。请遵守 [Streamlit](https://streamlit.io/) 的服务条款。
