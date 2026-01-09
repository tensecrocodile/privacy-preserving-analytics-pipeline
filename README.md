# Privacy-Preserving Analytics Pipeline

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.9+](https://img.shields.io/badge/python-3.9+-blue.svg)](https://www.python.org/downloads/)
[![FastAPI](https://img.shields.io/badge/FastAPI-latest-green.svg)](https://fastapi.tiangolo.com/)

An enterprise-grade, privacy-preserving analytics platform featuring differential privacy, federated learning, advanced tokenization, role-based access control (RBAC), and comprehensive audit logging for secure data analysis and compliance.

## Table of Contents

- [Features](#features)
- [Architecture](#architecture)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [API Documentation](#api-documentation)
- [Privacy & Security](#privacy--security)
- [Configuration](#configuration)
- [Deployment](#deployment)
- [Contributing](#contributing)
- [License](#license)

## Features

### Core Privacy Features

- **Differential Privacy**: Laplace and Gaussian mechanisms with adaptive epsilon budgeting
- **Tokenization & Redaction**: Format-preserving encryption (FPE), PII masking, field-level anonymization
- **Federated Learning Support**: Decentralized model training without centralizing raw data
- **Homomorphic Encryption Ready**: Support for encrypted computation pipelines
- **Data Minimization**: Automatic PII detection and redaction at ingestion

### Access Control & Governance

- **Role-Based Access Control (RBAC)**: Fine-grained user permissions (Admin, Analyst, Viewer, Auditor)
- **Attribute-Based Access Control (ABAC)**: Context-aware data access policies
- **Data Classification**: Automatic and manual sensitivity classification
- **Audit Trail**: Tamper-resistant, append-only event logging
- **Legal Hold**: Prevent deletion of legally sensitive data

### Analytics & Insights

- **Privacy-Preserving Aggregation**: Count, sum, mean with differential privacy
- **Synthetic Data Generation**: Create realistic datasets preserving statistical properties
- **De-Identified Analytics**: Analyst-friendly aggregate views without raw data
- **Behavioral Analytics**: User activity patterns with privacy guarantees
- **Compliance Reporting**: SOC 2, GDPR, CCPA compliance dashboards

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                      Client Applications                     │
└────────────┬─────────────────────────────────────────────────┘
             │
┌────────────▼────────────────────────────────────────────────┐
│              FastAPI Gateway (Rate Limited)                 │
│              - Input Validation & Schema Check              │
│              - JWT Authentication & RBAC                    │
└────────────┬─────────────────────────────────────────────────┘
             │
┌────────────▼────────────────────────────────────────────────┐
│           Ingestion Pipeline (Privacy Layer 1)              │
│  ┌──────────────────────────────────────────────────────┐   │
│  │ • PII Detection (Regex, ML-based)                     │   │
│  │ • Format Validation & Normalization                   │   │
│  │ • Tokenization (Deterministic Hashing/FPE)            │   │
│  │ • Data Classification (Sensitivity Tagging)           │   │
│  └──────────────────────────────────────────────────────┘   │
└────────────┬─────────────────────────────────────────────────┘
             │
┌────────────▼────────────────────────────────────────────────┐
│              Storage Layer (Encryption at Rest)             │
│  ┌──────────────────────────────────────────────────────┐   │
│  │ • Tokenized Data Store (PostgreSQL)                  │   │
│  │ • De-Identified Analytics Cache (Redis)              │   │
│  │ • Audit Log Store (Append-Only)                      │   │
│  │ • Key Management Service (Encrypted Keys)            │   │
│  └──────────────────────────────────────────────────────┘   │
└────────────┬─────────────────────────────────────────────────┘
             │
┌────────────▼────────────────────────────────────────────────┐
│           Analytics & Query Layer (Privacy Layer 2)          │
│  ┌──────────────────────────────────────────────────────┐   │
│  │ • Differential Privacy Engine (Budget-aware)         │   │
│  │ • Secure Aggregation (Encrypted Computation)         │   │
│  │ • Noise Addition (Laplace/Gaussian)                  │   │
│  │ • De-Identification Enforcement                      │   │
│  └──────────────────────────────────────────────────────┘   │
└────────────┬─────────────────────────────────────────────────┘
             │
┌────────────▼────────────────────────────────────────────────┐
│          Dashboard & Reporting (Limited View)               │
│  ┌──────────────────────────────────────────────────────┐   │
│  │ • Analytics Dashboard (React/Vue.js)                 │   │
│  │ • Compliance Reports                                 │   │
│  │ • Data Usage Metrics                                 │   │
│  │ • Privacy Score Card                                 │   │
│  └──────────────────────────────────────────────────────┘   │
└───────────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────────┐
│                    Monitoring & Compliance                     │
│  • Audit Logger (Immutable Event Stream)                      │
│  • Privacy Metrics (Epsilon Budget Tracking)                  │
│  • Anomaly Detection (Unusual Access Patterns)                │
│  • Compliance Scanner (GDPR/HIPAA/CCPA Checks)               │
└───────────────────────────────────────────────────────────────┘
```

## Installation

### Prerequisites

- Python 3.9+
- PostgreSQL 12+
- Redis 6.0+
- Docker & Docker Compose (optional)

### Clone & Install

```bash
git clone https://github.com/tensecrocodile/privacy-preserving-analytics-pipeline.git
cd privacy-preserving-analytics-pipeline

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### Using Docker Compose

```bash
docker-compose up -d
```

This will start:
- FastAPI application (port 8000)
- PostgreSQL (port 5432)
- Redis (port 6379)
- Prometheus (port 9090)
- Grafana (port 3000)

## Quick Start

### 1. Initialize Database

```bash
cd scripts
python init_db.py
```

### 2. Set Environment Variables

```bash
cp .env.example .env
# Edit .env with your configuration
```

### 3. Start the Server

```bash
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

### 4. Access the API

- API Docs: http://localhost:8000/docs
- ReDoc: http://localhost:8000/redoc

## API Documentation

### Event Ingestion

```bash
curl -X POST http://localhost:8000/api/v1/events \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -d '{
    "user_id": "user@example.com",
    "action": "login",
    "timestamp": "2024-01-15T10:30:00Z",
    "metadata": {
      "ip_address": "192.168.1.1",
      "device": "mobile"
    }
  }'
```

### Query Analytics (De-Identified)

```bash
curl -X GET "http://localhost:8000/api/v1/analytics/aggregate?metric=login_count&period=daily" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

### Audit Logs

```bash
curl -X GET "http://localhost:8000/api/v1/audit?user_id=admin&action=data_access&limit=100" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

## Privacy & Security

### Differential Privacy Implementation

The platform uses Laplace and Gaussian mechanisms for differentially private aggregation:

- **Epsilon (ε)**: Controls privacy-utility trade-off (lower = more private)
- **Delta (δ)**: Probability of privacy guarantee failure
- **Adaptive Budgeting**: Tracks ε consumption per user/department

```python
from app.privacy import DifferentialPrivacyEngine

engine = DifferentialPrivacyEngine(epsilon=1.0, delta=0.01)
noisy_count = engine.add_laplace_noise(true_count=1000, sensitivity=1)
```

### Tokenization Strategy

- **Deterministic**: Same input → Same token (allows joins)
- **Format-Preserving**: Email → Email format, Phone → Phone format
- **Reversible (Admin Only)**: Encryption key rotation monthly

### Access Control Model

```python
class UserRole(Enum):
    ADMIN = "admin"           # Full access, can decrypt
    ANALYST = "analyst"       # De-identified views only
    VIEWER = "viewer"         # Read-only, limited metrics
    AUDITOR = "auditor"       # Audit logs only
```

## Configuration

Edit `config/settings.py`:

```python
DATABASE_URL = "postgresql://user:password@localhost/privacy_db"
REDIS_URL = "redis://localhost:6379/0"

# Privacy Settings
DIFFERENTIAL_PRIVACY_EPSILON = 1.0
DIFFERENTIAL_PRIVACY_DELTA = 0.01

# Tokenization
TOKENIZATION_KEY = os.getenv("TOKENIZATION_KEY")
FORMAT_PRESERVING_ENCRYPTION = True

# RBAC
ENFORCE_RBAC = True
ADMIN_ONLY_DECRYPTION = True

# Audit
AUDIT_LOG_RETENTION_DAYS = 2555  # ~7 years
IMMMUTABLE_LOGS = True
```

## Project Structure

```
privacy-preserving-analytics-pipeline/
├─ app/
│  ├─ main.py                  # FastAPI application
│  ├─ api/                     # API routes
│  │  ├─ events.py            # Event ingestion
│  │  ├─ analytics.py        # Analytics queries
│  │  ├─ audit.py            # Audit logs
│  │  └─ compliance.py       # Compliance reports
│  ├─ privacy/                 # Privacy modules
│  │  ├─ __init__.py
│  │  ├─ differential_privacy.py
│  │  ├─ tokenization.py
│  │  ├─ pii_detector.py
│  │  ├─ anonymization.py
│  │  └─ federated_learning.py
│  ├─ models/                 # Data models & ORM
│  │  ├─ event.py
│  │  ├─ user.py
│  │  ├─ audit_log.py
│  │  └─ analytics_cache.py
│  ├─ security/               # Auth & RBAC
│  │  ├─ auth.py
│  │  ├─ rbac.py
│  │  └─ encryption.py
│  └─ utils/                  # Utilities
│     ├─ logger.py
│     ├─ validators.py
│     └─ metrics.py
├─ frontend/                  # React/Vue Dashboard
│  ├─ src/
│  ├─ public/
│  └─ package.json
├─ scripts/
│  ├─ init_db.py
│  ├─ seed_data.py
│  └─ run_tests.sh
├─ tests/                     # Unit & integration tests
│  ├─ test_privacy.py
│  │  ├─ test_differential_privacy.py
│  │  ├─ test_tokenization.py
│  │  └─ test_pii_detection.py
│  ├─ test_api.py
│  ├─ test_rbac.py
│  └─ test_audit.py
├─ docker-compose.yml
├─ Dockerfile
├─ requirements.txt
├─ .env.example
├─ config/
│  └─ settings.py
├─ docs/
│  ├─ ARCHITECTURE.md
│  ├─ PRIVACY.md
│  ├─ DEPLOYMENT.md
│  └─ API.md
├─ .gitignore
├─ LICENSE
└─ README.md
```

## Deployment

### Kubernetes

```bash
kubectl apply -f k8s/
```

See `docs/DEPLOYMENT.md` for detailed Kubernetes manifests.

### AWS (RDS + ElastiCache)

```bash
bash scripts/deploy_aws.sh
```

### GCP (Cloud SQL + Memorystore)

```bash
bash scripts/deploy_gcp.sh
```

## Testing

```bash
# Run all tests
pytest tests/ -v --cov=app

# Run specific test suite
pytest tests/test_privacy.py -v

# Run with coverage report
pytest tests/ --cov=app --cov-report=html
```

## Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

Please ensure:
- Code follows PEP 8 style guide
- Tests are included and pass
- Documentation is updated

## Security & Compliance

- **SOC 2 Type II**: Compliant data handling and audit logging
- **GDPR**: Right to be forgotten, data portability, consent management
- **HIPAA**: Encryption, access controls, audit trails
- **CCPA**: Privacy notice, opt-out mechanisms, data minimization

## Roadmap

- [ ] Homomorphic Encryption integration
- [ ] Secure Multi-Party Computation (SMPC)
- [ ] Advanced Synthetic Data Generation (CTGAN)
- [ ] GraphQL API with field-level privacy
- [ ] Real-time Privacy Budget Dashboard
- [ ] Automated Compliance Scanning (CIS Benchmarks)

## License

MIT License - see [LICENSE](LICENSE) file for details.

## Support & Contact

For issues, questions, or feature requests:
- GitHub Issues: [Report a Bug](https://github.com/tensecrocodile/privacy-preserving-analytics-pipeline/issues)
- Discussions: [Ask a Question](https://github.com/tensecrocodile/privacy-preserving-analytics-pipeline/discussions)
- Email: support@tensecrocodile.dev

---

**Made with ♥ by Tensecrocodile for Privacy & Data Security**
