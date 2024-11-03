
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



### 로컬 환경 

프로젝트를 열 때 위의 옵션(Github Codespaces, Dev Containers) 중 하나를 사용하지 않는 경우, 다음을 수행해야 합니다.

1. 다음 도구가 설치되어 있는지 확인합니다.

    * [.NET 8](https://dotnet.microsoft.com/downloads/)
    * [Git](https://git-scm.com/downloads)
    * [Azure Developer CLI (azd)](https://aka.ms/install-azd)
    * [VS Code](https://code.visualstudio.com/Download) 또는 [Visual Studio](https://visualstudio.microsoft.com/downloads/)
        * VS Code를 사용하는 경우 [C# Dev Kit](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csdevkit)를 설치합니다. 

2. 프로젝트를 열 때 위의 옵션 중 하나를 사용하지 않는 경우 다음을 수행해야 합니다.

    ```shell
    azd init -t cosmosdb-nosql-copilot
    ```

3. Visual Studio를 사용하는 경우 src/cosmos-copilot.sln 솔루션 파일을 엽니다. VS Code를 사용하는 경우 src 폴더를 엽니다.


### 배포 

1. 터미널을 열고 이 솔루션을 복제할 위치로 이동합니다.

1. 다음 명령을 실행하여 솔루션을 컴퓨터에 로컬로 다운로드합니다.

    ```bash
    azd init -t AzureCosmosDB/cosmosdb-nosql-copilot
    ```

1. 터미널에서 이 솔루션의 /infra 디렉터리로 이동합니다.

1. AZD에 로그인합니다.
    
    ```bash
    azd auth login
    ```

1. Azure 서비스를 프로비전하고, 로컬 솔루션 컨테이너를 빌드하고, 응용 프로그램을 배포합니다.
    
    ```bash
    azd up
    ```

### 로컬 디버깅 설정

이 솔루션을 배포하면 .NET 응용 프로그램에서 사용하는 secrets.json 파일에 끝점 및 구성 값이 자동으로 삽입됩니다.

퀵 스타트의 값을 수정하려면 이 샘플의 /src 폴더에 있는 csproj 파일에서 의 값을 찾아 저장합니다. `UserSecretsId` 

```xml
<PropertyGroup>
  <UserSecretsId>your-guid-here</UserSecretsId>
</PropertyGroup>
```
secrets.json 파일을 찾아 텍스트 편집기로 엽니다.

- Windows: `C:\Users\<YourUserName>\AppData\Roaming\Microsoft\UserSecrets\<UserSecretsId>\secrets.json`
- macOS/Linux: `~/.microsoft/usersecrets/<UserSecretsId>/secrets.json`



## 리소스 정리

1. 터미널을 열고 이 솔루션의 /infra 디렉터리로 이동합니다.

1. azd down을 입력합니다.
    
    ```bash
    azd down
    ```

## 가이드

### 지역 가용성

이 템플릿은 모든 Azure 지역에서 사용하지 못할 수 있는 gpt-4o 및 text-embedding-3-large 모델을 사용합니다.[최신 지역 가용성](https://learn.microsoft.com/azure/ai-services/openai/concepts/models#standard-deployment-model-availability) 을 확인하고 그에 따라 배포 중에 지역을 선택합니다.
  * `eastus2', 'eastus', 'japaneast', 'uksouth', 'northeurope', 또는 'westus3'을 사용하는 것이 좋습니다. 

### 비용

[Azure 가격 계산기](https://azure.microsoft.com/pricing/calculator/)를 사용하여 이 프로젝트의 아키텍처 비용을 예측할 수 있습니다

예를 들어 미국 달러로 현재 샘플을 빌드하는 방법은 다음과 같습니다.

월 평균 비용:

Azure Cosmos DB 서버리스(1M RU/s당 $0.25 USD): $0.25
Azure App Service(B1 플랜): $12.41
Azure OpenAI(GPT-4o 1M 입력/출력 토큰): $20(샘플은 10K 토큰 사용)
Azure OpenAI(text-3-large): < $0.01(샘플은 5K 토큰을 사용함)

## 참고 문서

이 샘플에 설명된 서비스 및 기능에 대한 자세한 내용은 다음을 참조하세요.

- [Azure Cosmos DB for NoSQL Vector Search 발표](https://aka.ms/CosmosDBDiskANNBlog/)
- [Azure OpenAI Service 문서](https://learn.microsoft.com/azure/cognitive-services/openai/)
- [시맨틱 커널](https://learn.microsoft.com/semantic-kernel/overview)
- [Azure App Service 문서](https://learn.microsoft.com/azure/app-service/)
- [ASP.NET Core Blazor 문서](https://dotnet.microsoft.com/apps/aspnet/web-apps/blazor)
