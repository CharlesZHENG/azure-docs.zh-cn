---
title: "Azure Batch CLI 入门 | Microsoft 文档"
description: "Azure CLI 中用于管理 Azure Batch 服务资源的 Batch 命令简介"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: fcd76587-1827-4bc8-a84d-bba1cd980d85
ms.service: batch
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 05/11/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 5bbeb9d4516c2b1be4f5e076a7f63c35e4176b36
ms.openlocfilehash: 19014e65920b16d2efbaa475b7c17b2a4e3a8471
ms.contentlocale: zh-cn
ms.lasthandoff: 06/13/2017


---
# <a name="manage-batch-resources-with-azure-cli"></a>使用 Azure CLI 管理 Batch 资源

Azure CLI 2.0 是 Azure 的新命令行体验，用于管理 Azure 资源。 它可以在 macOS、Linux 和 Windows 上使用。 Azure CLI 2.0 已经过优化，可以从命令行管理 Azure 资源。 可以使用 Azure CLI 管理 Azure Batch 帐户，以及管理池、作业、任务等资源。 对于通过 Batch API、Azure 门户和 Batch PowerShell cmdlet 执行的任务，许多都可以使用 Azure CLI 来编写脚本。

本文概述如何将 [Azure CLI 2.0 版](https://docs.microsoft.com/cli/azure/overview)与 Batch 配合使用。 请参阅 [Azure CLI 2.0 入门](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)，大致了解如何将 CLI 与 Azure 配合使用。

Microsoft 建议使用最新版的 Azure CLI，即 2.0 版。 有关 2.0 版的详细信息，请参阅 [Azure Command Line 2.0 now generally available](https://azure.microsoft.com/blog/announcing-general-availability-of-vm-storage-and-network-azure-cli-2-0/)（Azure 命令行 2.0 现已公开发布）。

## <a name="set-up-the-azure-cli"></a>设置 Azure CLI

若要安装 Azure CLI，请执行[安装 Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli.md) 中概述的步骤。

> [!TIP]
> 建议经常更新 Azure CLI 安装，利用服务更新和增强功能。
> 
> 

## <a name="command-help"></a>命令帮助

在每个命令后面追加 `-h` 即可在 Azure CLI 中显示该命令的帮助文本。 忽略任何其他选项。 例如：

* 若要获取 `az` 命令的帮助，请输入：`az -h`
* 若要获取 CLI 中所有 Batch 命令的列表，请使用：`az batch -h`
* 若要获取有关创建 Batch 帐户的帮助，请输入： `az batch account create -h`

如有疑问，请使用 `-h` 命令行选项获取有关任何 Azure CLI 命令的帮助。

> [!NOTE]
> 早期版本的 Azure CLI 使用 `azure` 作为 CLI 命令的前缀。 在 2.0 版中，所有命令现在都带 `az` 前缀。 请务必更新脚本，对 2.0 版使用新语法。
>
>  

另请参阅 Azure CLI 参考文档，详细了解[适用于 Batch 的 Azure CLI 命令](https://docs.microsoft.com/cli/azure/batch)。 

## <a name="log-in-and-authenticate"></a>登录并进行身份验证

若要将 Azure CLI 与 Batch 配合使用，需登录并进行身份验证。 请执行两个简单的步骤：

1. **登录到 Azure。** 登录到 Azure 即可访问 Azure Resource Manager 命令，包括 [Batch Management 服务](batch-management-dotnet.md)命令。  
2. **登录到 Batch 帐户。** 登录到 Batch 帐户即可访问 Batch 服务命令。   

### <a name="log-in-to-azure"></a>登录 Azure

可以通过多种不同的方式登录到 Azure，详见[使用 Azure CLI 2.0 登录](https://docs.microsoft.com/cli/azure/authenticate-azure-cli)：

1. [以交互方式登录](https://docs.microsoft.com/cli/azure/authenticate-azure-cli#interactive-log-in)。 从命令行自行运行Azure CLI 命令时，请以交互方式登录。
2. [使用服务主体登录](https://docs.microsoft.com/cli/azure/authenticate-azure-cli#logging-in-with-a-service-principal)。 从脚本或应用程序运行 Azure CLI 命令时，请使用服务主体登录。

本文的目的是介绍如何以交互方式登录到 Azure。 在命令行中键入 [az login](https://docs.microsoft.com/cli/azure/#login)：

```azurecli
# Log in to Azure and authenticate interactively.
az login
```

`az login` 命令返回一个用于身份验证的令牌，如下所示。 请按提供的说明打开网页，并将令牌提交到 Azure：

![登录 Azure](./media/batch-cli-get-started/az-login.png)

[示例 shell 脚本](#sample-shell-scripts)部分列出的示例也演示了如何以交互方式登录到 Azure，从而启动 Azure CLI 会话。 登录后，即可调用命令来处理 Batch Management 资源，包括 Batch 帐户、密钥、应用程序包和配额。  

### <a name="log-in-to-your-batch-account"></a>登录到 Batch 帐户

若要使用 Azure CLI 来管理 Batch 资源（例如池、作业和任务），需登录到 Batch 帐户并进行身份验证。 若要登录到 Batch 服务，请使用 [az batch account login](https://docs.microsoft.com/cli/azure/batch/account#login) 命令。 

可以使用两个选项对 Batch 帐户进行身份验证：

- **使用 Azure Active Directory (Azure AD) 身份验证。** 

    将 Azure CLI 与 Batch 配合使用时，通过 Azure AD 进行身份验证是默认设置，建议用于大多数方案。 
    
    如上一部分所述，以交互方式登录到 Azure 时，系统会缓存凭据，因此 Azure CLI 可以使用这些相同的凭据将你登录到 Batch 帐户。 如果通过服务主体登录到 Azure，也会使用这些凭据登录到 Batch 帐户。

    Azure AD 的优势是提供基于角色的访问控制 (RBAC)。 使用 RBAC 时，用户的访问权限取决于分配给他们的角色，而不是是否拥有帐户密钥。 你可以管理 RBAC 角色而不是帐户密钥，让 Azure AD 负责访问权限和身份验证。  

    如果在创建 Azure Batch 帐户时将其池分配模式设置为“用户订阅”，则需使用 Azure AD 进行身份验证。 

    若要使用 Azure AD 登录到 Batch 帐户，请调用 [az batch account login](https://docs.microsoft.com/cli/azure/batch/account#login) 命令： 

    ```azurecli
    az batch account login -g myresource group -n mybatchaccount
    ```

- **使用“共享密钥”身份验证。**

    [“共享密钥”身份验证](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service#authentication-via-shared-key)通过帐户访问密钥对适用于 Batch 服务的 Azure CLI 命令进行身份验证。

    若要通过创建 Azure CLI 脚本来自动调用 Batch 命令，则可使用“共享密钥”身份验证或 Azure AD 服务主体。 在某些方案中，使用“共享密钥”身份验证可能比创建服务主体简单。  

    若要使用“共享密钥”身份验证登录，请在命令行中包括 `--shared-key-auth` 选项：

    ```azurecli
    az batch account login -g myresourcegroup -n mybatchaccount --shared-key-auth
    ```

[示例 shell 脚本](#sample-shell-scripts)部分列出的示例演示了如何在使用 Azure AD 和共享密钥的情况下，通过 Azure CLI 登录到 Batch 帐户。

## <a name="sample-shell-scripts"></a>示例 shell 脚本

下表中列出的示例脚本演示如何将 Azure CLI 命令与 Batch 服务和 Batch Management 服务配合使用，以便完成常规任务。 这些示例脚本涵盖可以在用于 Batch 的 Azure CLI 中使用的许多命令。 

| 脚本 | 说明 |
|---|---|
| [创建批处理帐户](./scripts/batch-cli-sample-create-account.md) | 创建 Batch 帐户并将其与存储帐户相关联。 |
| [添加应用程序](./scripts/batch-cli-sample-add-application.md) | 添加应用程序，并上传打包的二进制文件。|
| [管理批处理池](./scripts/batch-cli-sample-manage-pool.md) | 演示如何创建、管理池并调整其大小。 |
| [使用批处理运行作业和任务](./scripts/batch-cli-sample-run-job.md) | 演示如何运行作业和添加任务。 |

## <a name="json-files-for-resource-creation"></a>用于创建资源的 JSON 文件

创建 Batch 资源（如池和作业）时，可以指定包含新资源配置的 JSON 文件，而无需将资源的参数作为命令行选项传递。 例如：

```azurecli
az batch pool create my_batch_pool.json
```

尽管只使用命令行选项即可创建大多数 Batch 资源，但某些功能需要指定 JSON 格式的包含资源详细信息的文件。 例如，若要指定启动任务的资源文件，必须使用 JSON 文件。

若要查看创建资源所需的 JSON 语法，请参阅 [Batch REST API 参考][rest_api]文档。 REST API 参考中的每个“添加资源类型”主题都包含用于创建该资源的示例 JSON 脚本。 可以将这些示例 JSON 脚本用作模板，以便将 JSON 文件与 Azure CLI 配合使用。 例如，若要查看用于创建池的 JSON 语法，请参阅[向帐户添加池][rest_add_pool]。

如需用于指定 JSON 文件的示例脚本，请参阅[使用 Batch 运行作业和任务](./scripts/batch-cli-sample-run-job.md)。

> [!NOTE]
> 如果在创建资源时指定 JSON 文件，则会忽略在命令行上为该资源指定的任何其他参数。
> 
> 

## <a name="efficient-queries-for-batch-resources"></a>适用于 Batch 资源的高效查询

每个 Batch 资源类型都支持 `list` 命令，该命令可查询 Batch 帐户并列出该类型的资源。 例如，可以列出帐户中的池以及作业中的任务：

```azurecli
az batch pool list
az batch task list --job-id job001
```

通过 `list` 操作查询 Batch 服务时，可以指定一个 OData 子句，以便限制返回的数据量。 由于所有筛选发生在服务器端，因此只会返回请求的数据。 执行 list 操作时，使用这些子句可以节省带宽（因而节省时间）。

下表介绍 Batch 服务支持的 OData 子句：

| 子句 | 说明 |
|---|---|
| `--select-clause [select-clause]` | 返回每个实体的属性子集。 |
| `--filter-clause [filter-clause]` | 仅返回与指定的 OData 表达式匹配的实体。 |
| `--expand-clause [expand-clause]` | 通过单个基础 REST 调用获取实体信息。 expand 子句目前仅支持 `stats` 属性。 |

如需演示如何使用 OData 子句的示例脚本，请参阅[使用 Batch 运行作业和任务](./scripts/batch-cli-sample-run-job.md)。

若要详细了解如何使用 OData 子句执行高效的列表查询，请参阅[高效查询 Azure Batch 服务](batch-efficient-list-queries.md)。

## <a name="troubleshooting-tips"></a>故障排除提示

排查 Azure CLI 问题时，可以参考以下提示：

* 使用 `-h` 获取任何 CLI 命令的 **帮助文本**
* 使用 `-v` 和 `-vv` 显示**详细的**命令输出。 包括 `-vv` 标志时，Azure CLI 显示实际的 REST 请求和响应。 使用这些开关可以方便地显示完整的错误输出。
* 可以使用 `--json` 选项查看 **JSON 格式的命令输出**。 例如， `az batch pool show pool001 --json` 以 JSON 格式显示 pool001 的属性。 然后，可以复制并修改此输出，以便在 `--json-file` 中使用（请参阅本文前面的 [JSON 文件](#json-files) ）。
* [Batch 论坛][batch_forum]由 Batch 团队成员监管。 如果遇到问题或需要具体操作方面的帮助，可将问题发布到该论坛。

## <a name="next-steps"></a>后续步骤

* 有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure/overview)。
* 有关 Batch 资源的详细信息，请参阅[适用于开发人员的 Azure Batch 概述](batch-api-basics.md)。
* 请参阅 [Application deployment with Azure Batch application packages](batch-application-packages.md) （使用 Azure Batch 应用程序包部署应用程序），了解如何使用此功能来管理和部署 Batch 计算节点上执行的应用程序。

[batch_forum]: https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch
[github_readme]: https://github.com/Azure/azure-xplat-cli/blob/dev/README.md
[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx

