# postgre-archive

为 macOS 预编译的 PostgreSQL 二进制包，支持 Intel (x86_64) 和 Apple Silicon (arm64)。

## 触发构建

```bash
git tag v202604171200
git push
git push origin v202604171200
```

构建完成后，GitHub Release 页面会自动发布全部支持版本在 arm64 与 x86_64 两个目标架构上的二进制包。

## 支持版本

| 系列 | 最新版本 | 类型 |
|------|---------|------|
| 15 | 15.12 | Stable |
| 16 | 16.8 | Stable |
| 17 | 17.4 | Stable |

## 构建环境

| 架构 | Runner | 最低 macOS |
|------|--------|-----------|
| x86_64 (Intel) | `macos-14` | 12.0 Monterey |
| arm64 (Apple Silicon) | `macos-14` | 12.0 Monterey |

说明：由于 GitHub Actions 已不再稳定提供 `macos-13`，x86_64 产物改为在 `macos-14` Apple Silicon runner 上通过 Rosetta 与 x86_64 Homebrew 依赖链进行交叉编译。

## 构建配置

- **SSL**: OpenSSL 3.x
- **Readline**: 启用
- **ICU**: 禁用（`--without-icu`）
- **依赖**: `openssl@3`、`readline`、`zlib`、`flex`、`bison`、`pkg-config`
- **源码**: 从 [ftp.postgresql.org](https://ftp.postgresql.org/pub/source/) 下载

## 使用方法

从 [Releases](../../releases) 页面下载对应架构的压缩包：

```bash
# 解压
tar xzf postgresql-VERSION-macos-ARCH.tar.gz
cd postgresql-VERSION-macos-ARCH

# 初始化数据目录（首次使用）
mkdir data
./bin/initdb -D "$(pwd)/data"

# 启动服务
./bin/pg_ctl -D "$(pwd)/data" -l "$(pwd)/postgresql.log" start

# 连接
./bin/psql -h 127.0.0.1 -p 5432 -U "$USER" postgres

# 关闭服务
./bin/pg_ctl -D "$(pwd)/data" stop
```

## 校验文件完整性

每个压缩包附带 SHA256 校验文件：

```bash
shasum -a 256 -c postgresql-VERSION-macos-ARCH.tar.gz.sha256
```
