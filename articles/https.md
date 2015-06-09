使用 HTTPS 保护 Docker Socket 进程
===

默认情况下，Docker 使用的是无网络环境的 Unix Socket。它可以使用 HTTP Socket 进行通信。

如果你需要 Docker 使用安全的方式来进行网络通信，你可以使用 `tlsverify` 标识来启用 TLS，并使用 `tlscacert` 标识来指定 CA 证书。

>警告：使用 TLS 和管理 CA 证书是一个高级话题。在投入生成环境使用之前，你需要自行熟悉了解 OpenSSL，X509 和 TLS 。

>警告：这些 TLS 指令只能生成在 Linux 上工作的证书，Mac OSX 附带的 OpenSSL 版本与 Docker 所需要使用的证书不兼容。