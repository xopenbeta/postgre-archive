# postgre-archive

为 macOS / Linux / Windows 预编译的 PostgreSQL 二进制包，支持 x86_64 和 arm64。

## 触发构建

```bash
git tag v202604171218
git push
git push origin v202604171218
```

构建完成后，GitHub Release 页面会自动发布全部支持版本在各平台（macOS/Linux/Windows）与两种目标架构（x86_64/arm64）上的二进制包。

## 支持版本

| 系列 | 最新版本 | 类型 |
|------|---------|------|
| 15 | 15.12 | Stable |
| 16 | 16.8 | Stable |
| 17 | 17.4 | Stable |

## 构建环境

| 平台 | 架构 | Runner |
|------|------|--------|
| macOS | x86_64 | `macos-14` (Rosetta + x86_64 Homebrew) |
| macOS | arm64 | `macos-14` |
| Linux | x86_64 | `ubuntu-24.04` |
| Linux | arm64 | `ubuntu-22.04-arm` |
| Windows | x86_64 | `windows-2022` + MSYS2 `UCRT64` |
| Windows | arm64 | `windows-11-arm` + MSYS2 `CLANGARM64` |

说明：macOS x86_64 产物在 Apple Silicon runner 上通过 Rosetta 与 x86_64 Homebrew 依赖链构建。

## 构建配置

- **SSL**: OpenSSL 3.x
- **Readline**: 启用
- **ICU**: 禁用（`--without-icu`）
- **依赖**: `openssl@3`、`readline`、`zlib`、`flex`、`bison`、`pkg-config`
- **Linux 打包**: `.tar.gz`
- **Windows 打包**: `.zip`
- **源码**: 从 [ftp.postgresql.org](https://ftp.postgresql.org/pub/source/) 下载

## 使用方法

从 [Releases](../../releases) 页面下载对应平台和架构的压缩包：

```bash
# macOS / Linux 解压
tar xzf postgresql-VERSION-PLATFORM-ARCH.tar.gz
cd postgresql-VERSION-PLATFORM-ARCH

# 初始化数据目录（首次使用）
mkdir data
./bin/initdb -D "$(pwd)/data"

# 启动服务
./bin/pg_ctl -D "$(pwd)/data" -l "$(pwd)/postgresql.log" start

# 连接（默认监听 5432）
./bin/psql -h 127.0.0.1 -p 5432 -U "$USER" postgres

# 关闭服务
./bin/pg_ctl -D "$(pwd)/data" stop
```

## 校验文件完整性

每个压缩包附带 SHA256 校验文件：

```bash
# macOS / Linux
shasum -a 256 -c postgresql-VERSION-PLATFORM-ARCH.tar.gz.sha256

# Windows (Git Bash / MSYS2)
sha256sum -c postgresql-VERSION-windows-ARCH.zip.sha256
```
