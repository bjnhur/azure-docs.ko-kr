---
title: '빠른 시작: Node.js 웹앱 만들기'
description: 몇 분 안에 첫 번째 Node.js Hello World를 Azure App Service에 배포합니다. App Service에 배포하는 여러 가지 방법 중 하나인 Visual Studio Code를 사용하여 배포합니다.
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.topic: quickstart
ms.date: 09/30/2019
ms.custom: seodec18
experimental: false
experiment_id: a231f2b4-2625-4d
ms.openlocfilehash: 91494cc9c1e3a1fc159702bdbb7f68a4423b604c
ms.sourcegitcommit: 265f1d6f3f4703daa8d0fc8a85cbd8acf0a17d30
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74671356"
---
# <a name="create-a-nodejs-web-app-in-azure"></a>Azure에서 Node.js 웹앱 만들기 

Azure App Service는 확장성 높은 자체 패치 웹 호스팅 서비스를 제공합니다. 이 빠른 시작에서는 Node.js 앱을 Azure App Service에 배포하는 방법을 보여줍니다.

## <a name="prerequisites"></a>필수 조건

Azure 계정이 없는 경우 지금 200달러의 Azure 크레딧으로 체험 계정에 [지금 가입](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-app-service-extension&mktingSource=vscode-tutorial-app-service-extension)하여 서비스 조합을 사용해 볼 수 있습니다.

Node.js 패키지 관리자인 [Node.js 및 npm](https://nodejs.org/en/download)과 함께 [Visual Studio Code](https://code.visualstudio.com/)를 설치해야 합니다.

또한 Azure PaaS(Platform as a Service)에서 Linux Web Apps를 만들고, 관리하고, 배포하는 데 사용할 수 있는 [Azure App Service 확장](vscode:extension/ms-azuretools.vscode-azureappservice)도 설치해야 합니다.

### <a name="sign-in"></a>로그인

확장이 설치되면 Azure 계정에 로그인합니다. 작업 표시줄에서 Azure 로고를 선택하여 **AZURE APP SERVICE** 탐색기를 표시합니다. **Azure에 로그인...** 을 선택하고 지침을 따릅니다.

![Azure에 로그인](containers/media/quickstart-nodejs/sign-in.png)

### <a name="troubleshooting"></a>문제 해결

**"이름이 [구독 ID]인 구독을 찾을 수 없습니다"** 오류가 표시되면 프록시를 사용하고 Azure API에 연결할 수 없기 때문일 수 있습니다. `export`를 사용하여 터미널의 프록시 정보를 사용하여 `HTTP_PROXY` 및 `HTTPS_PROXY` 환경 변수를 구성합니다.

```sh
export HTTPS_PROXY=https://username:password@proxy:8080
export HTTP_PROXY=http://username:password@proxy:8080
```

환경 변수를 설정해도 문제가 해결되지 않으면 아래에 있는 **문제가 발생했습니다.** 단추를 선택하여 문의해 주세요.

### <a name="prerequisite-check"></a>필수 구성 요소 확인

계속하기 전에 모든 필수 구성 요소를 설치하고 구성했는지 확인합니다.

VS Code에서 상태 표시줄에는 Azure 이메일 주소가 표시되고, **AZURE APP SERVICE** 탐색기에는 구독이 표시됩니다.

> [!div class="nextstepaction"]
> [문제가 발생했습니다.](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=getting-started)

## <a name="create-your-nodejs-application"></a>Node.js 애플리케이션 만들기

다음으로, 클라우드에 배포할 수 있는 Node.js 애플리케이션을 만듭니다. 이 빠른 시작에서는 애플리케이션 생성기를 사용하여 터미널에서 애플리케이션을 빠르게 스캐폴드합니다.

> [!TIP]
> [Node.js 자습서](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial)를 이미 완료한 경우 [Azure에 배포](#deploy-to-azure)로 건너뛸 수 있습니다.

### <a name="scaffold-a-new-application-with-the-express-generator"></a>Express 생성기를 사용하여 새 애플리케이션 스캐폴드

[Express](https://www.expressjs.com)는 Node.js 애플리케이션을 빌드하고 실행하는 데 널리 사용되는 프레임워크입니다. [Express 생성기](https://expressjs.com/en/starter/generator.html) 도구를 사용하여 새 Express 애플리케이션을 스캐폴드할(만들) 수 있습니다. Express 생성기는 npm 모듈로 제공되며, npm 명령줄 도구`npx`를 사용하여 직접(설치 없이) 실행할 수 있습니다.

```bash
npx express-generator myExpressApp --view pug --git
```

`--view pug --git` 매개 변수는 [pug](https://pugjs.org/api/getting-started.html) 템플릿 엔진(이전의 `jade`)을 사용하고 `.gitignore` 파일을 만들도록 생성기에 지시합니다.

애플리케이션의 모든 종속성을 설치하려면 새 폴더로 이동하여 `npm install`을 실행합니다.

```bash
cd myExpressApp
npm install
```

### <a name="run-the-application"></a>애플리케이션 실행

다음으로, 애플리케이션이 실행되는지 확인합니다. 터미널에서 `npm start` 명령을 사용하여 애플리케이션을 시작하여 서버를 시작합니다.

```bash
npm start
```

이제 브라우저를 열고, 다음과 같이 표시되는 [http://localhost:3000](http://localhost:3000)으로 이동합니다.

![Express 애플리케이션 실행](containers/media/quickstart-nodejs/express.png)

> [!div class="nextstepaction"]
> [문제가 발생했습니다.](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=create-app)

## <a name="deploy-to-azure"></a>Deploy to Azure

이 섹션에서는 VS Code와 Azure App Service 확장을 사용하여 Node.js 앱을 배포합니다. 이 빠른 시작에서는 앱이 압축되어 Linux의 Azure 웹앱에 배포되는 가장 기본적인 배포 모델을 사용합니다.

### <a name="deploy-using-azure-app-service"></a>Azure App Service를 사용하여 배포

먼저 VS Code에서 애플리케이션 폴더를 엽니다.

```bash
code .
```

**AZURE APP SERVICE** 탐색기에서 파란색 위쪽 화살표 아이콘을 선택하여 앱을 Azure에 배포합니다.

![웹앱에 배포](containers/media/quickstart-nodejs/deploy.png)

> [!TIP]
> '웹앱에 배포'를 입력하고 **Azure App Service: 웹앱에 배포** 명령을 실행하여 **명령 팔레트**(Ctrl+Shift+P)에서 배포할 수도 있습니다.

1. 현재 열려 있는 `myExpressApp` 디렉터리를 선택합니다.

1. 배포하려는 운영 체제에 따라 만들기 옵션을 선택합니다.

    - Linux: **새 웹앱 만들기**를 선택합니다.
    - Windows: **새 웹앱 만들기**를 선택하고 **고급** 옵션을 선택합니다.

1. 웹앱에 대해 전역적으로 고유한 이름을 입력하고, Enter 키를 누릅니다. 앱 이름에 유효한 문자는 'a-z', '0-9' 및 '-'입니다.

1. Linux를 대상으로 하는 경우 메시지가 표시되면 Node.js 버전을 선택합니다. **LTS** 버전을 권장합니다.

1. **고급** 옵션을 사용하여 Windows를 대상으로 지정하는 경우 추가 프롬프트를 따릅니다.
    1. **새 리소스 그룹 만들기**를 선택한 다음, 리소스 그룹의 이름을 입력합니다.
    1. 운영 체제에 대해 **Windows**를 선택합니다.
    1. 기존 App Service 계획을 선택하거나 새로 만듭니다. 새 계획을 만들 때 가격 책정 계층을 선택할 수 있습니다.
    1. Application Insights에 대한 메시지가 표시되면 **지금 건너뛰기**를 선택합니다.
    1. 액세스하려는 사용자 근처의 지역 또는 근처의 리소스를 선택합니다.

1. 모든 메시지에 응답하면 알림 채널에는 앱에 대해 만들어지는 Azure 리소스가 표시됩니다.

1. 대상 서버에서 `npm install`을 실행하도록 구성을 업데이트하라는 메시지가 표시되면 **예**를 선택합니다. 그러면 앱이 배포됩니다.

    ![구성된 배포](containers/media/quickstart-nodejs/server-build.png)

1. 배포가 시작되면 이후 배포에서 자동으로 동일한 App Service 웹앱을 대상으로 지정하도록 작업 영역을 업데이트하라는 메시지가 표시됩니다. 변경 내용이 올바른 앱에 배포되도록 하려면 **예**를 선택합니다.

    ![구성된 배포](containers/media/quickstart-nodejs/save-configuration.png)

> [!TIP]
> 애플리케이션이 PORT 환경 변수(`process.env.PORT`)에서 제공하는 포트를 수신 대기하고 있는지 확인하세요.

### <a name="browse-the-app-in-azure"></a>Azure에서 앱 찾아보기

배포가 완료되면 프롬프트에서 **웹 사이트 찾아보기**를 선택하여 새로 배포된 웹앱을 확인합니다.

### <a name="troubleshooting"></a>문제 해결

**"이 디렉터리 또는 페이지를 볼 수 있는 권한을 가지고 있지 않습니다."** 오류가 표시되면 애플리케이션이 제대로 시작되지 못했을 수 있습니다. 다음 섹션으로 이동하여 로그 출력을 확인하고 오류를 찾아 해결합니다. 해결할 수 없는 경우 아래에 있는 **문제가 발생했습니다.** 단추를 선택하여 문의해 주세요. 성심껏 도와드리겠습니다!

> [!div class="nextstepaction"]
> [문제가 발생했습니다.](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=deploy-app)

### <a name="update-the-app"></a>앱 업데이트

동일한 프로세스를 사용하여 새 앱을 만들지 않고 기존 앱을 선택하여 변경 내용을 해당 앱에 배포할 수 있습니다.

## <a name="viewing-logs"></a>로그 보기

이 섹션에서는 실행 중인 App Service 앱에서 로그를 보거나 "추적(tail)"하는 방법에 대해 알아봅니다. 앱에서 `console.log`에 대한 모든 호출은 Visual Studio Code의 출력 창에 표시됩니다.

**AZURE APP SERVICE** 탐색기에서 앱을 찾아서 마우스 오른쪽 단추로 클릭하고, **스트리밍 로그 보기**를 선택합니다.

메시지가 표시되면 로깅을 사용하도록 설정하고 애플리케이션을 다시 시작합니다. 애플리케이션이 다시 시작되면 VS Code 출력 창이 열려 로그 스트림에 연결됩니다.

![스트리밍 로그 보기](containers/media/quickstart-nodejs/view-logs.png)

![로깅 사용 및 다시 시작](containers/media/quickstart-nodejs/enable-restart.png)

몇 초 후에 로그 스트리밍 서비스에 연결되었다는 메시지가 표시됩니다. 페이지를 몇 번 새로 고쳐 더 많은 작업을 볼 수 있습니다.

    ```bash
    2019-09-20 20:37:39.574 INFO  - Initiating warmup request to container msdocs-vscode-node_2_00ac292a for site msdocs-vscode-node
    2019-09-20 20:37:55.011 INFO  - Waiting for response to warmup request for container msdocs-vscode-node_2_00ac292a. Elapsed time = 15.4373071 sec
    2019-09-20 20:38:08.233 INFO  - Container msdocs-vscode-node_2_00ac292a for site msdocs-vscode-node initialized successfully and is ready to serve requests.
    2019-09-20T20:38:21  Startup Request, url: /Default.cshtml, method: GET, type: request, pid: 61,1,7, SCM_SKIP_SSL_VALIDATION: 0, SCM_BIN_PATH: /opt/Kudu/bin, ScmType: None
    ```

> [!div class="nextstepaction"]
> [문제가 발생했습니다.](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=tailing-logs)

## <a name="next-steps"></a>다음 단계

축하합니다! 이 빠른 시작을 성공적으로 완료했습니다!

다음으로, 다른 Azure 확장을 확인합니다.

* [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
* [Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
* [Docker 도구](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)
* [Azure CLI 도구](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
* [Azure Resource Manager 도구](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

또는 [Azure용 Node 팩](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) 확장 팩을 설치하여 모두 가져옵니다.
