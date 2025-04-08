---
title: "git-lfs-故障排除"
description: ""
toc: false
---

以下是克隆或推送 LFS 文件失败时出现的一些常见问题。

# 克隆存储库或提取 LFS 文件失败，并显示 “Forbidden” 错误或其他身份验证错误
1. 验证您使用的是 GitHub 用户名和个人令牌，而不是 GitHub 密码。
1. 验证如果您要从文本文件复制个人令牌，则是否不包含额外的空格或制表符。 您可以将令牌粘贴到文本编辑器中，以验证其前面或结尾没有任何多余的空格。
1. 验证 Password Manager 中的个人令牌是否正确。 如果使用 Windows，则通常可以在“Windows 凭据管理器”中找到这些凭据，其名称包含存储库根目录下的“.lfsconfig”文件中的 URL。在撰写本文时，o3de 存储库条目的名称类似于“git：https：//d3df09qsjufr6g.cloudfront.net/api/v1”。 请记住，我们对每个存储库使用不同的 LFS URL。
1. 通过在命令窗口中运行以下命令，验证是否可以使用 GitHub 的 API 进行身份验证。 在命令中替换您的 username 和 token 以及您尝试访问的存储库的 URL。
    ```
    curl -H <GitHub username>:<GitHub token> https://api.github.com/repos/o3de/o3de
    ```
    如果您收到 404 响应，则您的凭证无效或您没有权限。 
    如果您收到 200 响应，则可以在返回的 JSON 中找到您的权限。
1. 通过在命令窗口中运行以下命令来验证您没有超出 GitHub API 速率限制。 在命令中替换您的 username 和 token。
    ```
    curl -H <GitHub username>:<GitHub token> https://api.github.com/rate_limit
    ```
    如果您没有剩余资源，则需要等待 '`reset`' 时间戳字段中设置的时间。
1. 检查 HTTP 标头的内容，以排查您发送到服务的凭证和响应的问题。 在命令窗口中启用其他日志记录
    ```
    set GIT_TRACE=1
    set GIT_CURL_VERBOSE=1
    ```
    启用此附加日志记录后，再次尝试 git作，您将能够看到您发送的确切身份验证标头和给出的响应。 请记住，凭证通常是 base64 编码的，因此您可能需要对其进行解码。 您可能还希望将输出通过管道传输到日志文件，以便于搜索。

# 推送 LFS 文件失败，并显示“禁止”错误或其他身份验证错误
1. 确认您没有尝试直接推送到 o3de 父存储库，这是不允许的。 您应该只将更改推送到主仓库的 fork。
1. 如果您尝试推送到您的复刻，请验证您是否已按照存储库根目录的“`.lfsconfig`”文件中的说明进行作，具体而言，您是否包含了复刻的名称。
1. 请参阅上述克隆或推送故障排除步骤，其中涵盖了更多身份验证故障排除步骤。

# 某些文件下载失败，并显示“Smudge error”消息
1. 当文件下载失败并出现污迹错误时，可能的原因是它从未上传。 首先，请求原始用户使用命令重新上传所有 LFS 文件
    ```
    git lfs push origin --all
    ```
    如果您知道文件的特定对象 ID（在错误消息中），则可以使用此命令上传单个缺失的文件
    ```
    git lfs push origin --object-id <object-id>
    ```
2. 上传文件后，运行以下命令从 'origin' 远程获取它们。
    ```
    git lfs pull origin
    ```
    如果文件仍未显示在工作区中，这可能是因为仅下载了这些文件。 使用以下命令刷新特定文件，将 '<branch>' 替换为您所在分支的名称，并将 'path/to/files' 替换为您丢失的文件的路径，无论是单个文件还是多个文件
    ```
    git checkout origin/<branch> path/to/files
    ```
