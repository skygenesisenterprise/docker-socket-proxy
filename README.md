<div align="center">

# 🚀 Docker Socket Proxy

**🔒 Security-Enhanced Docker Socket Access - Protect Your Infrastructure**

A lightweight HAProxy-based proxy that restricts Docker API access with granular permissions

[🚀 Quick Start](#-quick-start) • [✨ Features](#-features) • [📖 Docs](#-documentation) • [🤝 Contributing](#-contributing)

[![GitHub stars](https://img.shields.io/github/stars/skygenesisenterprise/docker-socket-proxy?style=social)](https://github.com/skygenesisenterprise/docker-socket-proxy/stargazers) [![GitHub forks](https://img.shields.io/github/forks/skygenesisenterprise/docker-socket-proxy?style=social)](https://github.com/skygenesisenterprise/docker-socket-proxy/network) [![GitHub issues](https://img.shields.io/github/issues/skygenesisenterprise/docker-socket-proxy)](https://github.com/skygenesisenterprise/docker-socket-proxy/issues) [![License](https://img.shields.io/badge/license-MIT-blue)](https://github.com/skygenesisenterprise/docker-socket-proxy/blob/main/LICENSE)

</div>

---

## 🌟 Why Docker Socket Proxy?

**Tired of exposing your entire Docker socket?** Docker Socket Proxy provides **granular access control** to Docker's API, preventing security disasters while enabling necessary functionality.

### 🎯 **The Problem**
- Docker socket access = **root privileges** on your host
- Services need socket access but shouldn't have full control
- Traditional solutions: all-or-nothing access

### ✅ **The Solution**
- **Fine-grained permissions** - Allow only what each service needs
- **HTTP 403 Forbidden** for unauthorized requests
- **Zero-trust security** - Block dangerous operations by default
- **Production-ready** - Used in enterprise environments

---

## 🚀 Quick Start

### 🐳 One-Command Setup

```bash
# Run the secure proxy
docker run -d --privileged \
  --name dockerproxy \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -p 127.0.0.1:2375:2375 \
  tecnativa/docker-socket-proxy
```

### 🔧 Connect Your Services

```bash
# Point your Docker client to the proxy
export DOCKER_HOST=tcp://localhost:2375

# Test the connection
docker version
```

**🎉 Your Docker socket is now protected!**

### 📋 Prerequisites

- **Docker** 17.03+ 🐳
- **Privileged container access** (required for socket binding)

---

## ✨ Features

### 🛡️ **Security First**

#### 🔒 **Granular Access Control**
- **Block dangerous operations** - Prevent `exec`, `run`, `pull` etc.
- **Allow specific APIs** - Enable only what services need
- **Environment-based config** - Simple variable toggles
- **HTTP 403 responses** - Clear security boundaries

#### 🚫 **Default Deny Policy**
- **Read-only by default** - Only `GET`/`HEAD` allowed initially
- **Critical ops blocked** - `POST`, `PUT`, `DELETE` restricted
- **Container ops protected** - Start/stop/kill operations controlled
- **Network security** - Isolated from public networks

### ⚙️ **Flexible Configuration**

#### 🎛️ **Environment Variables**
```bash
# Essential services
CONTAINERS=1    # Container management
IMAGES=1        # Image operations
NETWORKS=1      # Network control

# Advanced features
EXEC=1          # Container exec access
BUILD=1         # Build permissions
POST=1          # Write operations
```

#### 🔧 **Custom Socket Paths**
```bash
# Support different Docker installations
SOCKET_PATH=/var/run/balena-engine.sock  # balenaOS
SOCKET_PATH=/var/run/docker.sock         # Standard Docker
```

### 📊 **Production Ready**

#### 🚀 **Performance Optimized**
- **HAProxy core** - Battle-tested load balancer
- **Minimal resource usage** - Lightweight Alpine Linux
- **Connection pooling** - Efficient socket handling
- **Logging support** - Configurable log levels

#### 🐳 **Container Native**
- **Docker Hub** - Official images available
- **GitHub Registry** - Alternative distribution
- **Multi-arch support** - Linux containers
- **Version pinning** - Stable releases

---

## 🛠️ Tech Stack

### 🏗️ **Core Architecture**

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   Your Service  │───▶│  Docker Proxy    │───▶│  Docker Socket  │
│                 │    │  (HAProxy)       │    │  (/var/run/)    │
└─────────────────┘    └──────────────────┘    └─────────────────┘
                              │
                              ▼
                       ┌──────────────────┐
                       │  Access Control  │
                       │  Rules Engine    │
                       └──────────────────┘
```

### 📦 **Components**

**Frontend**: HAProxy 3.2.4 - High-performance proxy
**Base Image**: Alpine Linux - Minimal, secure foundation
**Configuration**: Template-based - Environment-driven rules
**Security**: Rule-based filtering - HTTP status enforcement

### 🔧 **Development Tools**

- **Python 3.8+** - Test suite and tooling
- **Poetry** - Dependency management
- **pytest** - Comprehensive testing
- **Black + flake8** - Code quality
- **pre-commit** - Automated quality checks

---

## 📖 Documentation

### 🚀 **Getting Started**

- [📚 Complete Documentation](https://github.com/Tecnativa/docker-socket-proxy/tree/main#docker-socket-proxy)
- [🎯 Quick Start Guide](#-quick-start)
- [⚙️ Configuration Guide](#grant-or-revoke-access-to-certain-api-sections)
- [🔧 Development Setup](#development)

### 🏗️ **Architecture**

- [📐 System Design](#how)
- [🔌 API Permissions](#grant-or-revoke-access-to-certain-api-sections)
- [🗄️ Configuration Reference](#usage)
- [🔒 Security Best Practices](#security-recommendations)

### 🧪 **Development**

- [👨‍💻 Contributing Guide](#-contributing)
- [🧪 Testing Guide](#testing)
- [📝 Code Standards](#code-style-guidelines)
- [🚀 Deployment Guide](#usage)

---

## 💻 Development

### 🎯 **Available Commands**

```bash
# 🚀 Development
poetry install          # Install dependencies
poetry run pytest       # Run all tests
poetry run pytest tests/test_service.py::test_function_name  # Single test

# 🏗️ Code Quality
poetry run black .      # Format code
poetry run isort .      # Sort imports
poetry run flake8       # Lint code
pre-commit run --all-files  # Full quality check

# 🧪 Testing
poetry run pytest --prebuild  # Build and test
poetry run pytest -n auto     # Parallel testing
```

### 📋 **Development Setup**

1. **Clone & Setup**
   ```bash
   git clone https://github.com/skygenesisenterprise/docker-socket-proxy.git
   cd docker-socket-proxy
   poetry install
   ```

2. **Run Tests**
   ```bash
   # Basic testing
   poetry run pytest

   # With image prebuild
   poetry run pytest --prebuild

   # Custom image
   DOCKER_IMAGE_NAME=my-custom poetry run pytest --prebuild
   ```

3. **Code Quality**
   ```bash
   # Format and lint
   poetry run black .
   poetry run isort .
   poetry run flake8
   ```

---

## 🗺️ Roadmap

### 🎯 **Current Release**

- ✅ **Core proxy functionality** - HAProxy-based filtering
- ✅ **Environment configuration** - Variable-driven permissions
- ✅ **Comprehensive testing** - Python test suite
- ✅ **Docker integration** - Official images and registries

### 🚀 **Future Enhancements**

- 🔄 **Advanced authentication** - API key and JWT support
- 📊 **Metrics and monitoring** - Prometheus integration
- 🔍 **Audit logging** - Detailed access logs
- 🌐 **Multi-socket support** - Multiple Docker instances
- 📱 **Web UI** - Configuration interface

---

## 🤝 Contributing

We welcome contributions to make Docker Socket Proxy even better!

### 🎯 **How to Contribute**

1. **🍴 Fork** the repository
2. **🌿 Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **💻 Code** your enhancement
4. **🧪 Test** thoroughly (`poetry run pytest`)
5. **📝 Commit** with clear messages
6. **🚀 Push** to your branch
7. **🔄 Open** a Pull Request

### 🏆 **Contribution Types**

| Type | Description | Examples |
|------|-------------|----------|
| 🐛 **Bug Fixes** | Fix reported issues | Security patches, test failures |
| ✨ **Features** | New functionality | New API permissions, auth methods |
| 📚 **Docs** | Improve documentation | Guides, API docs |
| 🧪 **Tests** | Add test coverage | New test cases, fixtures |
| 🔒 **Security** | Security enhancements | Input validation, access controls |

### 🎁 **Contributor Benefits**

- 🏅 **Recognition** - Listed in contributors
- 📖 **Early Access** - Try new features first
- 🎯 **Impact** - Help secure Docker deployments worldwide

---

## 📞 Support & Community

### 💬 **Get Help**

- 📖 [Documentation](#docker-socket-proxy) - Comprehensive guides
- 🐛 [GitHub Issues](https://github.com/skygenesisenterprise/docker-socket-proxy/issues) - Bug reports
- 💡 [Discussions](https://github.com/skygenesisenterprise/docker-socket-proxy/discussions) - Questions
- 📧 [Email Support](mailto:support@skygenesisenterprise.com) - Direct help

### 🐛 **Bug Reports**

Found a security issue or bug?

1. 🔍 **Search** existing issues first
2. 📝 **Create** detailed issue with:
   - Docker version and OS
   - Proxy configuration
   - Steps to reproduce
   - Expected vs actual behavior
3. 🏷️ **Label** appropriately (security issues get priority!)

---

## 🏆 Sponsors & Partners

**Special thanks to our sponsors who make secure Docker deployments possible:**

[![Tecnativa](https://img.shields.io/badge/skygenesisenterprise%20Solutions-blue)](https://www.skygenesisenterprise.com)

**🤝 Support open-source security - become a [sponsor](https://github.com/sponsors/skygenesisenterprise)!**

---

## 📄 License

This project is licensed under the **MIT** - see the [LICENSE](LICENSE) file for details.

```
MIT License

Copyright (c) 2025 Sky Genesis Enterprise

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.
```

---

## 🙏 Acknowledgments

- 🚀 **[Sky Genesis Enterprise](https://www.skygenesisenterprise.com)** - Development & maintenance
- 👥 **Open Source Community** - Security research and feedback
- 🏛️ **HAProxy Project** - Core proxy technology
- 🐳 **Docker Community** - Platform and ecosystem

---

<div align="center">

## 🚀 **Ready to Secure Your Docker Socket?**

[⭐ Star This Repo](https://github.com/skygenesisenterprise/docker-socket-proxy) • [🐳 Try Docker Hub](https://hub.docker.com/r/skygenesisenterprise/docker-socket-proxy) • [📖 Read Documentation](#docker-socket-proxy) • [🐛 Report Issues](https://github.com/skygenesisenterprise/docker-socket-proxy/issues)

---

**Made with 🛡️ by the [Sky Genesis Enterprise](https://www.skygenesisenterprise.com) team**

*Securing Docker deployments, one socket at a time*

</div>