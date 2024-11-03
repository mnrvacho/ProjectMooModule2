# ProjectMooModule2

## Hands-on Lab 개요

본 실습 과정은 한국 마이크로소프트와 파트너의 기술 협력 프로그램 중 하나인 "Project Moo"의 두 번째 실습 과정입니다. Azure OpenAI와 Data Platform의 활용 방안에 대해 알아 보고 해당 기능들을 구현해 봅니다. 

## 사전 요구 사항 (필요 도구)

* Azure 구독: Azure OpenAI, Azure AI Search, Azure App Service등의 Azure 리소스를 배포하게 됩니다. 권한이 있으신지 확인해주세요. (참고: Azure OpenAI에 대한 역할 기반 액세스 제어 - Azure AI services | Microsoft Learn)
* Visual Studio Code 설치: https://code.visualstudio.com/
* Docker Install: https://docs.docker.com/desktop/install/windows-install/
* Python 3.8 or later version 설치
* 아래 모듈을 Terminal에서 Install 해주세요.

    ```
    pip uninstall python-dotenv
    pip install python-dotenv
    ```

    ```
    pip install num2words
    pip install pandas
    pip install tiktoken
    ```

## 사용 Azure 리소스 및 환경

* Azure OpenAI
* Azure AI Search
* Bing Search
* Azure App Service
* Azure Storage Account

## 실습 순서

* [Global Batch API](https://github.com/jeongaelee/ProjectMooModule1/blob/main/Batch.md)
* [Azure OpenAI Assistants Function Calling, File Search 사용해보기](https://github.com/jeongaelee/ProjectMooModule1/blob/main/Assistants.md)
* [RAG를 사용한 Python 채팅 샘플 애플리케이션](https://github.com/jeongaelee/ProjectMooModule1/blob/main/RAG.md)
* [Azure OpenAI On Your Data - File Upload](https://github.com/jeongaelee/ProjectMooModule1/blob/main/OnYourData-FileUpload.md)
* [Azure OpenAI On Your Data - Embeddings and Search](https://github.com/jeongaelee/ProjectMooModule1/blob/main/OnYourData-EmbeddingsAndSearch.md)

## 참고 및 인용된 Repository

* [Azure OpenAI Service documentation] (https://learn.microsoft.com/en-us/azure/ai-services/openai/)
* [GPT Fundamentals, Use Cases and Sample Solutions] (https://github.com/Azure/azure-openai-samples)
* [GPT 기초, 사용 사례 및 샘플 솔루션 - 한국어 버전] (https://github.com/HyounsooKim/azure-openai-samples-kr)
