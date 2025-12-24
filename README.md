# Golden Quant System

> **注意**: 项目名称、域名等配置可通过 `deploy.env` 自定义，详见[部署配置指南](docs/DEPLOYMENT_CONFIG.md)。

开源量化投资研究系统，用于量化交易和分析。

## 🏗️ 项目结构

```
golden-quant-system/
├── frontend/                # 前端项目 (React + TypeScript)
│   ├── src/                # 源代码
│   │   ├── components/     # 组件库
│   │   ├── pages/          # 页面
│   │   ├── services/       # API 服务
│   │   ├── store/          # 状态管理
│   │   └── utils/          # 工具函数
│   └── package.json        # 前端依赖配置
├── backend/                # 后端项目 (Python FastAPI)
│   ├── src/               # 源代码
│   │   ├── api/           # API 路由
│   │   ├── core/          # 核心配置
│   │   ├── models/        # 数据模型
│   │   ├── services/      # 业务服务
│   │   └── utils/         # 工具函数
│   ├── docker/            # Docker 配置
│   └── requirements.txt   # Python 依赖
├── backtest-engine/       # 回测引擎（独立组件）
├── strategy-template/     # 策略仓库模板
├── deploy.example.env     # 部署配置模板
└── docs/                  # 文档
```

## 🛠️ 技术栈

### 前端技术栈
- **框架**: React 18.2.0 + TypeScript
- **UI 组件**: Ant Design Pro
- **状态管理**: Redux Toolkit
- **路由**: React Router v6
- **国际化**: i18next
- **构建工具**: Vite
- **图表库**: ECharts
- **HTTP 客户端**: axios

### 后端技术栈
- **框架**: Python FastAPI 0.104.1
- **ORM**: SQLAlchemy 2.0+
- **主数据库**: PostgreSQL 15
- **时序数据库**: ClickHouse 23.8
- **缓存**: Redis 7
- **消息队列**: Kafka + Celery
- **认证**: JWT + OAuth2.0

### 基础设施
- **容器化**: Docker + Docker Compose
- **编排**: Kubernetes
- **监控**: Prometheus + Grafana
- **日志**: Grafana Loki + Promtail
- **版本控制**: GitLab
- **回测引擎**: Backtrader（支持扩展其他引擎）

### 运行环境
- **Node.js**: 20.19.4+
- **Python**: 3.12.7

## 🚀 快速开始

### 环境要求
- Node.js 20.19.4+
- Python 3.12.7
- Docker & Docker Compose
- Git

### 1. 克隆项目

```bash
git clone https://github.com/chatwater/golden-quant-system.git
cd golden-quant-system
```

### 2. 后端设置

**方式一：一键启动（推荐）**

```bash
cd backend

# 创建虚拟环境（首次）
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 启动本地开发环境（Docker基础服务 + 本地FastAPI）
./scripts/dev.sh

# 其他启动选项：
# ./scripts/dev.sh docker   # 完整Docker环境
# ./scripts/dev.sh services # 仅启动基础服务
```

**方式二：分步启动**

```bash
cd backend

# 创建虚拟环境
python -m venv venv
source venv/bin/activate  # Windows: .\venv\Scripts\activate

# 安装依赖
pip install -r requirements.txt

# 启动基础服务
./scripts/dev.sh services

# 在另一个终端启动 FastAPI
source venv/bin/activate
uvicorn src.main:app --reload --host 0.0.0.0 --port 8000
```

### 3. 前端设置

```bash
cd frontend

# 安装依赖
npm install

# 启动开发服务器
npm run dev
```

### 4. 访问服务

- **前端**: http://localhost:5173
- **后端 API**: http://localhost:8000
- **API 文档**: http://localhost:8000/api/v1/docs

## 📦 生产环境部署

### 快速部署

```bash
# 1. 创建部署配置
cp deploy.example.env deploy.env

# 2. 修改配置（项目名、域名、密码等）
vim deploy.env

# 3. 生成配置文件
./scripts/generate-deploy-config.sh

# 4. 构建前端
cd frontend && npm run build

# 5. 启动服务
cd ../backend && ./scripts/prod.sh start
```

### 多客户部署

项目支持为不同客户自定义品牌配置，只需修改 `deploy.env` 中的：

```bash
PROJECT_NAME_EN="Your Company System"     # 项目名称
PROJECT_NAME_ZH="您的公司系统"              # 中文名称
DOMAIN_FRONTEND="app.yourcompany.com"     # 前端域名
THEME_PRIMARY_COLOR="1890ff"              # 主题色
COPYRIGHT_OWNER="您的公司名"               # 版权信息
```

详细说明请参考 [部署配置指南](docs/DEPLOYMENT_CONFIG.md)。

## 🔬 回测引擎

### 本地运行回测

```bash
cd backtest-engine

# 创建虚拟环境
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 运行示例策略
python runner.py \
    --engine=backtrader \
    --config=../strategy-template/config.xml \
    --strategy=../strategy-template/strategy.py \
    --start=20230101 \
    --end=20231231 \
    --output=./results \
    --data-source=csv
```

详细说明请参考：
- [回测引擎](backtest-engine/README.md)
- [策略模板](strategy-template/README.md)

## 🌐 服务访问地址

### 开发环境

| 服务 | 地址 |
|------|------|
| 前端 | http://localhost:5173 |
| 后端 API | http://localhost:8000 |
| API 文档 | http://localhost:8000/api/v1/docs |
| PostgreSQL | localhost:5432 |
| ClickHouse | localhost:8123 (HTTP) / localhost:9000 (TCP) |
| MinIO Console | http://localhost:9003 |
| Redis | localhost:6379 |
| Prometheus | http://localhost:9090 |
| Grafana | http://localhost:3001 |

### 生产环境

通过 `deploy.env` 配置域名后访问。

## 🏗️ 系统架构

### 核心组件
- **Web 应用**: React 前端 + FastAPI 后端
- **数据存储**: PostgreSQL (关系型) + ClickHouse (时序) + Redis (缓存)
- **消息系统**: Kafka (事件流) + Celery (任务队列)
- **监控体系**: Prometheus (指标) + Grafana (可视化) + Loki (日志)

### 业务模块
- **用户管理**: 认证、授权、权限控制
- **市场数据**: 实时行情、历史数据、技术指标
- **策略管理**: 策略开发、参数优化、因子分析
- **回测分析**: 历史回测、业绩归因、风险分析
- **组合管理**: 组合列表、风险管理、持仓分析
- **交易执行**: 订单管理、风控系统、交易监控
- **系统管理**: 用户管理、日志审计、系统监控

## 📝 配置说明

项目采用**集中配置管理**，所有部署相关配置都在 `deploy.env` 中：

- **品牌配置**: 项目名称、标语、版权、主题色、Logo
- **域名配置**: 前端、API、监控等域名
- **数据库配置**: PostgreSQL、Redis、ClickHouse、MinIO
- **安全配置**: JWT 密钥、管理员账号

详细说明请参考 [部署配置指南](docs/DEPLOYMENT_CONFIG.md)。

## 🤝 贡献指南

我们欢迎所有形式的贡献！请查看 [CONTRIBUTING.md](CONTRIBUTING.md) 了解详细的贡献指南。

### 快速开始贡献

1. Fork 项目
2. 创建功能分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改（遵循 Conventional Commits 规范）
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 创建 Pull Request

### 提交规范

我们使用 [Conventional Commits](https://www.conventionalcommits.org/) 规范：

- `feat`: 新功能
- `fix`: 修复问题
- `docs`: 文档更新
- `style`: 代码格式
- `refactor`: 重构
- `test`: 测试相关
- `chore`: 构建/工具相关

## 📄 许可证

本项目采用**自定义双许可模式**：

- **个人使用**：完全免费，可用于学习、研究等非商业目的
- **商业使用**：需要获得许可并支付费用

详细的许可条款请查看 [LICENSE](LICENSE) 文件。

## 📚 相关文档

- [部署配置指南](docs/DEPLOYMENT_CONFIG.md) - 多客户部署配置说明
- [贡献指南](CONTRIBUTING.md) - 如何为项目做贡献
- [更新日志](CHANGELOG.md) - 版本更新记录
- [回测引擎说明](docs/BACKTEST_ENGINES.md) - 支持的回测引擎介绍
- [回测引擎](backtest-engine/README.md) - 回测执行组件
- [策略模板](strategy-template/README.md) - 策略仓库模板

---

**Golden Quant System** - 开源量化投资研究平台
