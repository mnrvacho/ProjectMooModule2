
# Azure Cosmos DB for NoSQL, Azure OpenAI Service, Azure App Service 및 시맨틱 커널을 이용한 Copilot 앱 개발

이 샘플 애플리케이션은 Azure App Service, Azure OpenAI Service와 함께 새로운 벡터 데이터베이스 기능과 함께 NoSQL용 Azure Cosmos DB를 사용하여 다중 테넌트, 다중 사용자, Generative-AI RAG 패턴 애플리케이션을 빌드하는 방법을 보여 줍니다. 이 샘플은 네이티브 SDK와 시맨틱 커널 통합을 모두 사용하는 방법을 보여줍니다. 이 샘플에서는 이러한 유형의 응용 프로그램을 디자인하고 빌드하는 데 필요한 많은 개념에 대한 실용적인 지침을 제공합니다.


## 실습 목표

이 응용 프로그램은 다음 개념과 이를 구현하는 방법을 보여 줍니다.

- Azure Cosmos DB for NoSQL을 사용하여 확장성이 뛰어난 다중 테넌트 및 사용자, 생성형 AI 채팅 애플리케이션 개발
- Azure OpenAI 서비스를 사용하여 완성(completion)과 임베딩(embeddings) 생성
- Managing a context window (chat history) for natural conversational interactions with an LLM.
- Manage per-request token consumption and payload sizes for Azure OpenAI Service requests.
- Building a semantic cache using Azure Cosmos DB for NoSQL vector search for improved performance and cost.
- Using the Semantic Kernel SDK for completion and embeddings generation.
- Implementing RAG Pattern using vector search in Azure Cosmos DB for NoSQL on custom data to augment generated responses from an LLM. 



### 아키텍처 
![image](https://github.com/user-attachments/assets/0f20a36f-ab2e-4db7-8f76-bf5c22515f70)


### 사용자 환경
![image](https://github.com/user-attachments/assets/734ce15c-7d93-4ff2-82fb-ba409ec6a807)



## Getting Started

### Prerequisites

- Azure 구독
- Azure OpenAI 서비스에 대한 구독 액세스. [Azure OpenAI 서비스에 대한 액세스를 요청](https://aka.ms/oaiapply). 액세스 권한이 있는 경우 배포하기에 충분한 할당량을 확인하려면 아래를 참조하세요.
- [Azure Cosmos DB for NoSQL 벡터 검색 미리 보기](https://learn.microsoft.com/azure/cosmos-db/nosql/vector-search#enroll-in-the-vector-search-preview-feature) 에 등록합니다(자세한 내용은 아래 참조).
- .NET 8 이상. [Download](https://dotnet.microsoft.com/download/dotnet/8.0)
- [Azure 개발자 CLI](https://aka.ms/azd-install)
- Visual Studio, VS Code, GitHub Codespaces 또는 다른 편집기를 사용하여 이 샘플의 소스를 편집하거나 볼 수 있습니다.


    #### Vector search 미리보기 세부 정보
    이 랩에서는 미리 보기 기능 등록이 필요한 **NoSQL용 Azure Cosmos DB에 대한 벡터 검색** 이라는 미리 보기 기능을 활용합니다. 아래 단계에 따라 등록하십시오. 이 솔루션을 배포하기 전에 등록해야 합니다.

  1. Azure Cosmos DB for NoSQL 리소스 페이지로 이동합니다.
  2. "설정" 메뉴 항목에서 "기능" 창을 선택합니다.
  3. "Azure Cosmos DB for NoSQL에서 벡터 검색"을 선택합니다.
  4. 설명을 읽고 미리 보기에 등록할 것인지 확인합니다.
  5. "Enable(사용)"을 선택하여 Vector Search 미리보기에 등록합니다.

    #### Azure OpenAI 할당량 한도 확인

    이 샘플을 성공적으로 배포하려면 구독 내에서 이 샘플에서 사용하는 모델에 대한 충분한 Azure OpenAI 할당량이 있어야 합니다. 이 샘플에서는 분당 **10K 토큰이 있는 gpt-4o**와 분당 **5K 토큰이 있는 text-3-large**의 두 가지 모델이 있는 새 Azure OpenAI 계정을 배포합니다. 모델 할당량을 확인하고 변경하는 방법에 대한 자세한 내용은 [ Azure OpenAI 서비스 할당량 관리](https://learn.microsoft.com/azure/ai-services/openai/how-to/quota)를 참조하세요.

    #### Azure 구독 권한 요구 사항

    이 솔루션은 [사용자가 할당한 관리 ID](https://learn.microsoft.com/entra/identity/managed-identities-azure-resources/overview) 를 배포하고 이 ID에 Azure Cosmos DB RBAC 권한을 정의한 다음 적용합니다. 최소한 Azure 구독 또는 [구독 소유자](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles/privileged#owner) 액세스 권한에서 ID에 할당된 다음 Azure RBAC 역할이 필요하며, 이를 통해 다음 두 가지를 모두 제공합니다.

    - [Manged Identity Contributor](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles/identity#managed-identity-contributor)
    - [DocumentDB Account Contributor](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles/databases#documentdb-account-contributor)





[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/AzureCosmosDB/cosmosdb-nosql-copilot)
[![Open in Dev Containers](https://img.shields.io/static/v1?style=for-the-badge&label=Dev%20Containers&message=Open&color=blue&logo=visualstudiocode)](https://vscode.dev/redirect?url=vscode://ms-vscode-remote.remote-containers/cloneInVolume?url=https://github.com/AzureCosmosDB/cosmosdb-nosql-copilot)



### Local Environment

If you're not using one of the above options for opening the project, then you'll need to:

1. Make sure the following tools are installed:

    * [.NET 8](https://dotnet.microsoft.com/downloads/)
    * [Git](https://git-scm.com/downloads)
    * [Azure Developer CLI (azd)](https://aka.ms/install-azd)
    * [VS Code](https://code.visualstudio.com/Download) or [Visual Studio](https://visualstudio.microsoft.com/downloads/)
        * If using VS Code, install the [C# Dev Kit](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csdevkit)

2. Download the project code:

    ```shell
    azd init -t cosmosdb-nosql-copilot
    ```

3. If you're using Visual Studio, open the src/cosmos-copilot.sln solution file. If you're using VS Code, open the src folder.

7. Continue with the [deploying steps](#deployment).

### VS Code Dev Containers

A related option is VS Code Dev Containers, which will open the project in your local VS Code using the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers):

1. Start Docker Desktop (install it if not already installed)
2. Open the project:

    [![Open in Dev Containers](https://img.shields.io/static/v1?style=for-the-badge&label=Dev%20Containers&message=Open&color=blue&logo=visualstudiocode)](https://vscode.dev/redirect?url=vscode://ms-vscode-remote.remote-containers/cloneInVolume?url=https://github.com/AzureCosmosDB/cosmosdb-nosql-copilot)

3. In the VS Code window that opens, once the project files show up (this may take several minutes), open a terminal window.

4. Continue with the [deploying steps](#deployment)

### Deployment

1. Open a terminal and navigate to where you would like to clone this solution

1. Run the following command to download the solution locally to your machine:

    ```bash
    azd init -t AzureCosmosDB/cosmosdb-nosql-copilot
    ```

1. From the terminal, navigate to the /infra directory in this solution.

1. Log in to AZD.
    
    ```bash
    azd auth login
    ```

1. Provision the Azure services, build your local solution container, and deploy the application.
    
    ```bash
    azd up
    ```

### Setting up local debugging

When you deploy this solution it automatically injects endpoints and configuration values into the secrets.json file used by .NET applications.

To modify values for the Quickstarts, locate the value of `UserSecretsId` in the csproj file in the /src folder of this sample and save the value.

```xml
<PropertyGroup>
  <UserSecretsId>your-guid-here</UserSecretsId>
</PropertyGroup>
```
Locate the secrets.json file and open with a text editor.

- Windows: `C:\Users\<YourUserName>\AppData\Roaming\Microsoft\UserSecrets\<UserSecretsId>\secrets.json`
- macOS/Linux: `~/.microsoft/usersecrets/<UserSecretsId>/secrets.json`


### Quickstart

Follow the Quickstarts in this solution to go through the concepts for building RAG Pattern apps and the features in this sample and how to implement them yourself.

Please see [Quickstarts](quickstart.md)


## Clean up

1. Open a terminal and navigate to the /infra directory in this solution.

1. Type azd down
    
    ```bash
    azd down
    ```

## Guidance

### Region Availability

This template uses gpt-4o and text-embedding-3-large models which may not be available in all Azure regions. Check for [up-to-date region availability](https://learn.microsoft.com/azure/ai-services/openai/concepts/models#standard-deployment-model-availability) and select a region during deployment accordingly
  * We recommend using `eastus2', 'eastus', 'japaneast', 'uksouth', 'northeurope', or 'westus3'

### Costs

You can estimate the cost of this project's architecture with [Azure's pricing calculator](https://azure.microsoft.com/pricing/calculator/)

As an example in US dollars, here's how the sample is currently built:

Average Monthly Cost:
* Azure Cosmos DB Serverless ($0.25 USD per 1M RU/s): $0.25
* Azure App Service (B1 Plan): $12.41
* Azure OpenAI (GPT-4o 1M input/output tokens): $20 (Sample uses 10K tokens)
* Azure OpenAI (text-3-large): < $0.01 (Sample uses 5K tokens)

## Resources

To learn more about the services and features demonstrated in this sample, see the following:

- [Azure Cosmos DB for NoSQL Vector Search announcement](https://aka.ms/CosmosDBDiskANNBlog/)
- [Azure OpenAI Service documentation](https://learn.microsoft.com/azure/cognitive-services/openai/)
- [Semantic Kernel](https://learn.microsoft.com/semantic-kernel/overview)
- [Azure App Service documentation](https://learn.microsoft.com/azure/app-service/)
- [ASP.NET Core Blazor documentation](https://dotnet.microsoft.com/apps/aspnet/web-apps/blazor)
