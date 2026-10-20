# SkillHub - LazyCat App

[SkillHub](https://github.com/iflytek/skillhub)（科大讯飞开源的自托管 Agent 技能注册中心：技能包发布/版本管理、RBAC 治理、审计日志、安全扫描）的懒猫微服（LazyCat）打包。

## 模式

- **镜像模式（mirror）**：`ghcr.io/iflytek/skillhub-{server,web,scanner}` 经 `ghcr.nju.edu.cn` 加速拉取，`require_digest_match: true` 校验镜像内容与上游一致。
- **自动跟随上游**：每日检查上游 semver tag（server/web/scanner 三镜像同步），发现新版本自动构建发布。
- **喵喵商店发布**：仅私有商店（MiaoMiao private store），不发布官方商店。

## 架构（与上游 compose.release.yml 一致）

| 服务 | 说明 |
| --- | --- |
| web | 前端（nginx，入口 :80） |
| server | Java 后端 API（:8080） |
| skill-scanner | 技能安全扫描（可选 LLM 辅助） |
| postgres:16-alpine | 主数据库 |
| redis:7-alpine | 队列/缓存（AOF 持久化） |

## 说明

- **默认管理员**：`admin`，密码为稳定随机值（`BOOTSTRAP_ADMIN_PASSWORD`），首次登录后建议修改
- **登录方式**：已启用密码直登（`SKILLHUB_AUTH_DIRECT_ENABLED=true`）；GitHub/GitLab/飞书/钉钉 OAuth2 需要外部 IdP 回调，未启用
- **存储**：`SKILLHUB_STORAGE_PROVIDER=local`（单机本地存储，数据在 `/lzcapp/var/skillhub/storage`）；如需 S3/OSS 可后续加环境变量
- **安全扫描**：可选配置 LLM API Key/端点/模型（部署参数），留空则仅规则扫描
- `SKILLHUB_PUBLIC_BASE_URL` 自动指向懒猫子域名

## 所需 Secrets

| Secret | 说明 |
| --- | --- |
| `APPSTORE_URL` / `APPSTORE_TOKEN` | 喵喵商店 API 地址与发布令牌 |
| `APP_ID` | 可选 |
| `PRIVATE_STORE_GROUP_CODES` | 可选，私有分组码 |

## 许可

Apache-2.0（上游）
