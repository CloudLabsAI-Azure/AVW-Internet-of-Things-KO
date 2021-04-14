# 연습 4: Azure IoT Edge 시작

## 시나리오

배포되는 디바이스 수가 매우 많으면 대량의 데이터가 수집되어 클라우드로 전송됩니다. 그러면 Edge로 인텔리전스를 전송하기가 어려워질 수도 있습니다.

Fabrikam, Inc.에서는 IoT Edge 게이트웨이 디바이스를 사용하여 즉시 처리 가능하도록 Edge로 인텔리전스를 전송하려고 합니다. 데이터 중 일부분은 클라우드로 계속 전송할 예정입니다. IoT Edge로 데이터 인텔리전스를 전송하면 로컬 네트워크 속도가 느려도 데이터를 처리하고 빠르게 대응 조치를 취할 수 있습니다.

이를 위해 Azure IoT Edge 솔루션의 프로토타입을 작성해야 합니다. 먼저 평균 온도를 계산하는 데 사용할 Stream Analytics 모듈을 디바이스에 배포합니다. 그리고 프로세스 제어 값이 초과되면 경고 알림을 생성합니다.

## 개요

이 연습에서는 Edge 디바이스에 Stream Analytics 모듈을 배포하고, 프로세스 제어 값이 초과되면 경고 알림을 생성합니다. 미리 작성되어 이 랩에서 사용할 IoT Edge 디바이스로 구성된 Linux 기반 Azure VM이 제공됩니다.

이 연습에서는 다음 작업을 완료합니다.

* IoT Edge VM에 연결
* Edge 디바이스에 Edge 모듈 추가
* Edge 디바이스에 Azure Stream Analytics Edge 모듈 배포

Azure IoT Edge에는 클라우드에서 실행되는 클라우드 서비스와 디바이스에서 실행되는 런타임이 함께 포함되어 있습니다. 런타임은 디바이스에서 워크플로를 시작하고 관리합니다. 이 워크플로는 전체 시나리오 작성을 위해 특정 순서로 연결해야 하는 컨테이너 집합으로 구성되어 있습니다. IoT Edge는 IoT Hub를 통해 관리됩니다. Azure IoT Edge에서는 클라우드 서비스를 사용하여 개발하는 워크로드를 Edge 디바이스에서 실행할 수 있습니다. 이 워크로드는 docker 호환 컨테이너를 사용하여 배포되는 모듈입니다. 이러한 모듈은 인공 지능 애플리케이션이나 Azure 및 타사 서비스일 수도 있고 비즈니스 논리일 수도 있습니다. IoT Edge에 대한 자세한 내용은 다음 링크를 참조하세요. ```https://docs.microsoft.com/en-us/azure/iot-edge/about-iot-edge```

## 솔루션 아키텍처
 
  ![랩 04 아키텍처](media/lab4_architecture.png)

### 작업 1: IoT Edge VM에 연결

이 작업에서는 IoT Edge VM에 연결한 다음 디바이스에서 Azure IoT Edge가 실행되는지 확인합니다.

1. Azure Portal 메뉴에서 **리소스 그룹**을 클릭합니다.
 
1. **iot-{deployment-id}** 리소스 그룹에서 IoT Edge 가상 머신 **linuxagentvm-{deployment-id}**을(를) 클릭합니다.

1. **개요** 창 위쪽에서 **연결**을 클릭하고 **SSH**를 클릭합니다.

1. **연결** 창의 **4. VM에 연결하려면 아래의 예제 명령 실행** 아래에서 예제 명령을 복사합니다.

    이 명령은 VM의 IP 주소와 관리자 사용자 이름이 포함된 가상 머신에 연결하는 데 사용할 수 있는 샘플 SSH 명령입니다. 명령의 형식은 `ssh demouser@52.170.205.79`와 같습니다.

    > **참고**: 복사한 샘플 명령에는 **-i <private key path>**가 포함되어 있습니다. 텍스트 편집기를 사용해 명령의 이 부분을 제거한 다음 업데이트된 명령을 클립보드에 복사하세요.
 
1. **```https://shell.azure.com```**으로 이동하여 Azure Cloud Shell을 열고 **Bash**를 선택합니다.

1. **고급 설정 표시**를 클릭하고 다음 세부 정보를 입력합니다.

   * 리소스 그룹: **기존 항목 사용** -> **iot-{deployment-id}** 선택
   * 스토리지 계정: **새로 만들기**를 선택하고 **cloudstore{deployment-id}** 입력
   * 파일 공유: **새로 만들기**를 선택하고 **blob** 입력
   
     >   **참고**: - **deployment-id** 세부 정보는 환경 세부 정보 페이지에서 확인할 수 있습니다.
        
   ![스토리지 계정을 만들기 위한 선택 경로를 보여 주는 Azure Portal 스크린샷.](media/storageaccount01.png)

1. Azure Cloud Shell 명령 프롬프트에서 텍스트 편집기에서 업데이트한 `ssh` 명령을 붙여넣고 **Enter** 키를 누릅니다.

1. **Are you sure you want to continue connecting?(계속 연결하시겠습니까?)**이라는 메시지가 표시되면 `yes(예)`를 입력하고 **Enter** 키를 누릅니다.

1. 암호를 입력하라는 메시지가 표시되면 **Password.1!!**를 입력하고 **Enter** 키를 누릅니다.

   ![Linuxagent 가상 머신에 연결하는 중.](media/vmlogin.png)

1. VM에 연결되면 터미널 명령 프롬프트가 변경되어 다음과 같이 Linux VM의 이름이 표시됩니다.

    ```cmd/sh
    demouser@linuxagentvm-{deployment-id}:~$
    ```

    따라서 연결되어 있는 VM을 확인할 수 있습니다.

    > **중요:** VM에 연결할 때는 아직 설치하지 않은 Edge VM용 업데이트를 설치하라는 메시지가 표시될 가능성이 높습니다.  이 랩에서는 해당 메시지를 무시합니다. 그러나 프로덕션 환경에서는 항상 Edge 디바이스를 최신 상태로 유지해야 합니다.

1. VM에 Azure IoT Edge 런타임이 설치되어 있는지 확인하려면 다음 명령을 실행합니다.

    ```cmd/sh
    iotedge version
    ```

    이 명령을 실행하면 가상 머신에 현재 설치되어 있는 Azure IoT Edge 런타임의 버전이 출력됩니다.
    IoT Edge 디바이스에는 IoT Edge 런타임이 설치되어 있습니다. IoT Edge 런타임은 디바이스를 IoT Edge 디바이스로 설정하는 프로그램 컬렉션입니다. 서로 연동되는 IoT Edge 런타임 구성 요소를 통해 IoT Edge 디바이스는 에지에서 코드를 수신하고 결과를 IoT Hub에 전달할 수 있습니다.

### 작업 2: Edge 디바이스에 Edge 모듈 추가

이 연습에서는 Simulated Temperature Sensor를 사용자 지정 IoT Edge 모듈로 추가한 다음 IoT Edge 디바이스에서 실행되도록 배포합니다.

IoT Edge 모듈은 컨테이너로 구현되는 실행 가능 패키지입니다.

IoT Edge 모듈을 통해 클라우드 워크로드를 IoT 디바이스에서 직접 실행되도록 배포할 수 있습니다. IoT Edge 모듈은 IoT Edge에서 관리하는 최소 계산 단위입니다. IoT Edge 모듈을 사용하면 클라우드가 아닌 디바이스에서 데이터를 분석할 수 있습니다. 워크로드 중 일부를 에지로 이동하면 디바이스가 더 짧은 시간 내에 클라우드로 메시지를 전송할 수 있으며, 이벤트에 더욱 빠르게 반응할 수 있습니다.

1. 필요한 경우 Azure 계정 자격 증명을 사용하여 Azure Potal에 로그인합니다.
 
1. 리소스 그룹 타일에서**iothub-{deployment-id}**을(를) 클릭하여 IoT Hub를 엽니다.

1. **IoT Hub** 블레이드 왼쪽의 **자동 디바이스 관리** 아래에서 **IoT Edge**를 클릭합니다.

1. IoT Edge 디바이스 목록에서 **turbine-06**을 클릭합니다.

1. **turbine-06** 블레이드의 **모듈** 탭에는 현재 디바이스용으로 구성되어 있는 모듈 목록이 표시됩니다.

    현재 IoT Edge 디바이스에는 IoT Edge 런타임의 일부분인 Edge 에이전트(`$edgeAgent`) 및 Edge 허브(`$edgeHub`) 모듈만 구성되어 있습니다.

1. **turbine-06** 블레이드 위쪽에서 **모듈 설정**을 클릭합니다.

1. **디바이스에서 모듈 설정: turbine-06** 블레이드에서 **IoT Edge 모듈** 섹션을 찾습니다.

1. **IoT Edge 모듈** 아래에서 **추가**를 클릭하고 **IoT Edge 모듈**을 클릭합니다.

1. **IoT Edge 모듈 추가** 창의 **IoT Edge 모듈 이름** 아래에 **turbinesensor**를 입력합니다.

    그러면 사용자 지정 모듈의 이름이 "turbinesensor"로 지정됩니다.

1. **이미지 URI** 아래에 **asaedgedockerhubtest/asa-edge-test-module:simulated-temperature-sensor**를 입력합니다.

    > **참고**: 이 이미지는 제품 그룹에서 이 테스트 시나리오 지원을 위해 제공하는 Docker Hub에 게시된 이미지입니다.

1. 선택한 탭을 변경하려면 **모듈 쌍 설정**을 클릭합니다.

1. 모듈 쌍에 대해 필요한 속성을 지정하려면 다음 JSON을 입력합니다.

    ```json
    {
        "EnableProtobufSerializer": false,
        "EventGeneratingSettings": {
            "IntervalMilliSec": 500,
            "PercentageChange": 2,
            "SpikeFactor": 2,
            "StartValue": 68,
            "SpikeFrequency": 20
        }
    }
    ```

    이 JSON은 모듈 쌍의 필요한 속성을 설정하여 Edge 모듈을 구성합니다.

1. 블레이드 아래쪽에서 **추가**를 클릭합니다.

1. **디바이스에서 모듈 설정: turbine-06** 블레이드 아래쪽에서 **다음: 경로 >**를 클릭합니다.

1. 기본 경로는 이미 구성되어 있습니다.

    * 이름: **route**
    * 값: `FROM /messages/* INTO $upstream`

    이 경로는 IoT Edge 디바이스에 포함되어 있는 모든 모듈의 메시지를 모두 IoT Hub로 전송합니다.

1. **검토 + 만들기**를 클릭합니다.

1. **배포** 아래에 표시된 배포 매니페스트를 잠시 검토합니다. 

    여기서 확인할 수 있는 것처럼 IoT Edge 디바이스용 배포 매니페스트는 JSON 형식이므로 내용을 쉽게 확인할 수 있습니다.

    `properties.desired` 섹션 아래의 `modules` 섹션에서 IoT Edge 디바이스로 배포할 IoT Edge 모듈이 선언됩니다. 모든 모듈의 이미지 URI와 컨테이너 레지스트리 자격 증명도 이 섹션에 포함되어 있습니다.

    ```json
    {
        "modulesContent": {
            "$edgeAgent": {
                "properties.desired": {
                    "modules": {
                        "turbinesensor": {
                            "settings": {
                                "image": "asaedgedockerhubtest/asa-edge-test-module:simulated-temperature-sensor",
                                "createOptions": ""
                            },
                            "type": "docker",
                            "version": "1.0",
                            "status": "running",
                            "restartPolicy": "always"
                        },
    ```

    JSON에서 아래쪽의 **$edgeHub** 섹션에는 Edge 허브의 필요한 속성이 포함되어 있습니다. 그리고 모듈 간에/모듈에서 IoT Hub로 이벤트를 라우팅하기 위한 라우팅 구성도 포함되어 있습니다.

    ```json
        "$edgeHub": {
            "properties.desired": {
                "routes": {
                  "route": "FROM /messages/* INTO $upstream"
                },
                "schemaVersion": "1.0",
                "storeAndForwardConfiguration": {
                    "timeToLiveSecs": 7200
                }
            }
        },
    ```

    JSON에서 더 아래쪽으로 내려가면 **turbinesensor** 모듈의 섹션이 있습니다. 여기서 `properties.desired` 섹션에는 Edge 모듈 구성의 필요한 속성이 포함되어 있습니다.

    ```json
                },
                "turbinesensor": {
                    "properties.desired": {
                        "EnableProtobufSerializer": false,
                        "EventGeneratingSettings": {
                            "IntervalMilliSec": 500,
                            "PercentageChange": 2,
                            "SpikeFactor": 2,
                            "StartValue": 68,
                            "SpikeFrequency": 20
                        }
                    }
                }
            }
        }
    ```

1. 블레이드 아래쪽에서 **만들기**를 클릭하여 디바이스용 모듈 설정을 완료합니다.

1. 이제 **turbine-06** 블레이드의 **모듈** 아래에 **turbinesensor**가 나열됩니다.

    > **참고**: 모듈을 처음으로 표시할 때는 **새로 고침**을 클릭해야 할 수도 있습니다.

    **turbinesensor**의 런타임 상태는 아직 보고되지 않았을 수도 있습니다.

1. 블레이드 위쪽에서 **새로 고침**을 클릭합니다.

1. 이제 **turbinesensor** 모듈의 **런타임 상태**가 **running(실행 중)**으로 설정됩니다.

    그래도 값이 보고되지 않으면 잠시 기다렸다가 블레이드를 다시 새로 고쳐 봅니다.
 
1. Cloud Shell 세션을 아직 열지 않았으면 엽니다.

    `vm-iot-edge-{deployment-id}` 가상 머신에 더 이상 연결되어 있지 않으면 앞에서와 같이 **SSH**를 사용하여 연결합니다.

1. IoT Edge 디바이스에서 현재 실행되고 있는 모듈의 목록을 표시하려면 Cloud Shell 명령 프롬프트에서 다음 명령을 입력합니다.

    ```cmd/sh
    iotedge list
    ```

1. 명령의 출력은 다음과 같습니다. 

    ```cmd/sh
    demouser@linuxagentvm-{deployment-id}:~$ iotedge list
    NAME             STATUS           DESCRIPTION      CONFIG
    edgeHub          running          Up a minute      mcr.microsoft.com/azureiotedge-hub:1.0
    edgeAgent        running          Up 26 minutes    mcr.microsoft.com/azureiotedge-agent:1.0
    turbinesensor    running          Up 34 seconds    asaedgedockerhubtest/asa-edge-test-module:simulated-temperature-sensor
    ```

    보시다시피 `turbinesensor`가 실행 중인 모듈 중 하나로 나열됩니다.

1. 모듈 로그를 확인하려면 다음 명령을 입력합니다.

    ```cmd/sh
    iotedge logs turbinesensor
    ```

    명령의 출력은 다음과 같습니다.

    ```cmd/sh
    demouser@linuxagentvm-{deployment-id}:~$ iotedge logs turbinesensor
    11/14/2019 18:05:02 - Send Json Event : {"machine":{"temperature":41.199999999999925,"pressure":1.0182182583425192},"ambient":{"temperature":21.460937846433808,"humidity":25},"timeCreated":"2019-11-14T18:05:02.8765526Z"}
    11/14/2019 18:05:03 - Send Json Event : {"machine":{"temperature":41.599999999999923,"pressure":1.0185790159334602},"ambient":{"temperature":20.51992724976499,"humidity":26},"timeCreated":"2019-11-14T18:05:03.3789786Z"}
    11/14/2019 18:05:03 - Send Json Event : {"machine":{"temperature":41.999999999999922,"pressure":1.0189397735244012},"ambient":{"temperature":20.715225311096397,"humidity":26},"timeCreated":"2019-11-14T18:05:03.8811372Z"}
    ```

    `iotedge logs` 명령을 사용하면 모든 Edge 모듈의 모듈 로그를 확인할 수 있습니다.

1. Simulated Temperature Sensor 모듈은 메시지 500개를 전송한 후에 중지됩니다. 다음 명령을 실행하면 모듈을 다시 시작할 수 있습니다.

    ```cmd/sh
    iotedge restart turbinesensor
    ```

    지금은 모듈을 다시 시작하지 않아도 됩니다. 나중에 모듈에서 원격 분석 전송이 중지되면 Cloud Shell로 다시 이동하여 SSH를 통해 Edge VM에 연결한 다음 이 명령을 실행하면 모듈이 다시 설정됩니다. 다시 설정된 모듈은 원격 분석 전송을 다시 시작합니다.

### 작업 3: Azure Stream Analytics를 IoT Edge 모듈로 배포

앞에서 **turbinesensor** 모듈을 배포했으며, IoT Edge 디바이스에서 이 모듈이 실행되고 있음을 확인했습니다. 이제 IoT Edge 디바이스 자체에서 메시지를 IoT Hub로 전송하기 전에 처리할 수 있는 Stream Analytics 모듈을 추가할 수 있습니다.

#### Stream Analytics 작업 검토

1. 리소스 그룹 타일에서 **iot-{deployment-id}**을(를) 클릭하고 Stream Analytics 작업 **iotedge-streamjob-{deployment-id}**을(를) 선택합니다.

1. 다음으로 블레이드 왼쪽의 **작업 토폴로지** 아래에서 **입력**을 선택하고 입력 작업 하나(**temperature**)가 이미 정의되어 있음을 확인합니다.

1. 다음으로 **작업 토폴로지** 아래에서 **출력**을 선택하고 출력 작업 하나(**alert**)가 이미 정의되어 있음을 확인합니다.

1. 왼쪽 탐색 메뉴의 **구성** 아래에서 스토리지 계정 설정을 클릭합니다. 그런 다음 iotstorage{deployment-id} 스토리지 계정이 추가되었는지 확인합니다.

#### Stream Analytics 작업 배포

1. Azure Portal에서 **iothub-{deployment-id}** IoT Hub 리소스로 이동합니다.

1. 왼쪽 탐색 메뉴의 **자동 디바이스 관리** 아래에서 **IoT Edge**를 클릭합니다.

1. **디바이스 ID** 아래에서 **turbine-06**을 클릭합니다.

1. **turbine-06** 창 위쪽에서 **모듈 설정**을 클릭합니다.

1. **디바이스에서 모듈 설정: turbine-06** 창에서 **IoT Edge 모듈** 섹션을 찾습니다.

1. **IoT Edge 모듈** 아래에서 **추가**를 클릭하고 **Azure Stream Analytics 모듈**을 클릭합니다.

1. **Edge 배포** 창의 **구독** 아래에서 이 과정용으로 사용 중인 구독이 선택되어 있는지 확인합니다.

1. **Edge 작업** 드롭다운에서 **iotedge-streamjob-{deployment-id}** Stream Analytics 작업이 선택되어 있는지 확인합니다.

    > **참고**: 작업은 이미 선택되어 있는데 **저장** 단추는 사용 불가능하도록 설정되어 있을 수도 있습니다. 이 경우 **Edge 작업** 드롭다운을 다시 열고 **iotedge-streamjob-{deployment-id}** 작업을 다시 선택하면 됩니다. 그러면 **저장** 단추가 사용 가능하도록 설정됩니다.

1. 창 아래쪽에서 저장을 클릭합니다.

    배포는 몇 분 정도 걸릴 수 있습니다.

1. **디바이스에서 모듈 설정: turbine-06** 블레이드 아래쪽에서 **검토 + 만들기**를 클릭합니다.

1. **검토 + 만들기** 탭에서 이제 **배포 Manifest** JSON이 방금 구성한 라우팅 정의와 Stream Analytics 모듈로 업데이트되어 있음을 확인합니다.

1. 블레이드 아래쪽에서 **만들기**를 클릭합니다.

1. Edge 패키지가 정상적으로 게시되면 **IoT Edge 모듈** 섹션 아래에 새 ASA 모듈이 나열됩니다.

1. **IoT Edge 모듈** 아래에서 **iotedge-streamjob-{deployment-id}**을(를) 클릭합니다.

    이 모듈이 방금 Edge 디바이스에 추가된 Stream Analytics 모듈입니다.

1. **IoT Edge 모듈 업데이트** 창에서 **이미지 URI**는 표준 Azure Stream Analytics 이미지를 가리키도록 설정되어 있습니다.

    ```text
    mcr.microsoft.com/azure-stream-analytics/azureiotedge:1.0.7
    ```

    이 이미지는 IoT Edge 디바이스에 배포되는 모든 ASA 작업에 사용되는 것과 같은 이미지입니다.

    > **참고**:  구성되어 있는 **이미지 URI** 끝부분의 버전 번호에는 Stream Analytics 모듈을 만든 시점의 현재 최신 버전이 반영됩니다.

1. 모든 값을 기본값으로 유지하고 **IoT Edge 사용자 지정 모듈** 창을 닫습니다.

1. **디바이스에서 모듈 설정: turbine-06** 블레이드에서 **다음: 경로 >**를 클릭합니다.

    기존 라우팅이 표시됩니다.

1. 정의되어 있는 기본 경로를 다음의 3개 경로로 바꿉니다.
   
   > **참고**: `iotstreamjob-edge-{deployment-id}` 자리 표시자는 실제 Azure Stream Analytics 작업 모듈의 이름으로 바꾸세요.
   
    * 경로 1
        * 이름: **telemetryToCloud**
        * 값: `FROM /messages/modules/turbinesensor/* INTO $upstream`
    * 경로 2
        * 이름: **alertsToReset**
        * 값: `FROM /messages/modules/iotedge-streamjob-{deployment-id}/* INTO BrokeredEndpoint("/modules/turbinesensor/inputs/control")`
    * 경로 3
        * 이름: **telemetryToAsa**
        * 값: `FROM /messages/modules/turbinesensor/* INTO BrokeredEndpoint("/modules/iotedge-streamjob-{deployment-id}/inputs/temperature")`

    > **참고**: **이전**을 클릭하여 모듈 및 모듈 이름 목록을 확인하고 **다음**을 클릭하여 이 단계로 돌아올 수도 있습니다.

    여기서 정의하는 경로는 다음과 같습니다.

    * **telemetryToCloud** 경로는 `turbinesensor` 모듈 출력의 모든 메시지를 Azure IoT Hub로 전송합니다.
    * **alertsToReset** 경로는 Stream Analytics 모듈 출력의 모든 경고 메시지를 **turbinesensor** 모듈의 입력으로 전송합니다.
    * **telemetryToAsa** 경로는 `turbinesensor` 모듈 출력의 모든 메시지를 Stream Analytics 모듈 입력으로 전송합니다.

1. **디바이스에서 모듈 설정: turbine-06** 블레이드 아래쪽에서 **검토 + 만들기**를 클릭합니다.

1. **검토 + 만들기** 탭에서 이제 **배포 Manifest** JSON이 방금 구성한 라우팅 정의와 Stream Analytics 모듈로 업데이트되어 있음을 확인합니다.

1. `turbinesensor` Simulated Temperature Sensor 모듈의 JSON 구성은 다음과 같습니다.

    ```json
    "turbinesensor": {
        "settings": {
            "image": "asaedgedockerhubtest/asa-edge-test-module:simulated-temperature-sensor",
            "createOptions": ""
        },
        "type": "docker",
        "version": "1.0",
        "status": "running",
        "restartPolicy": "always"
    },
    ```

1. 앞에서 구성한 경로의 JSON 구성, 그리고 JSON 배포 정의에서 이러한 경로가 구성되는 방식은 다음과 같습니다.

    ```json
    "$edgeHub": {
        "properties.desired": {
            "routes": {
                "telemetryToCloud": "FROM /messages/modules/turbinesensor/* INTO $upstream",
                "alertsToReset": "FROM /messages/modules/iotedge-streamjob-{deployment-id}/* INTO BrokeredEndpoint(\\\"/modules/turbinesensor/inputs/control\\\")",
                "telemetryToAsa": "FROM /messages/modules/turbinesensor/* INTO BrokeredEndpoint(\\\"/modules/iotedge-streamjob-{deployment-id}/inputs/temperature\\\")"
            },
            "schemaVersion": "1.0",
            "storeAndForwardConfiguration": {
                "timeToLiveSecs": 7200
            }
        }
    },
    ```

1. 블레이드 아래쪽에서 **만들기**를 클릭합니다.

#### 데이터 확인

1. **SSH**를 통해 **IoT Edge 디바이스**에 연결했던 **Cloud Shell** 세션으로 다시 이동합니다.  

    > **참고**: 연결이 닫혔거나 시간이 초과되었으면 다시 연결합니다. 앞에서와 같이 `SSH` 명령을 실행하고 로그인합니다.

1. 디바이스에 배포된 모듈의 목록을 표시하려면 명령 프롬프트에서 다음 명령을 입력합니다.

    ```cmd/sh
    iotedge list
    ```

    새 Stream Analytics 모듈이 IoT Edge 디바이스로 배포되려면 시간이 다소 걸릴 수 있습니다. 배포가 완료되면 이 명령의 목록 출력에 모듈이 표시됩니다.

    ```cmd/sh
    demouser@linuxagentvm-{deployment-id}:~$ iotedge list
    NAME                       STATUS           DESCRIPTION      CONFIG
    iotedge-streamjob-232539   running          Up a minute      mcr.microsoft.com/azure-stream-analytics/azureiotedge:1.0.5
    edgeAgent                  running          Up 6 hours       mcr.microsoft.com/azureiotedge-agent:1.0
    edgeHub                    running          Up 4 hours       mcr.microsoft.com/azureiotedge-hub:1.0
    turbinesensor              running          Up 4 hours       asaedgedockerhubtest/asa-edge-test-module:simulated-temperature-sensor
    ``` 

    > **참고**:  Stream Analytics 모듈이 목록에 표시되지 않으면 1~2분 정도 기다렸다가 다시 시도해 보세요. IoT Edge 디바이스에서 모듈 배포가 업데이트되려면 시간이 다소 걸릴 수 있습니다.

1. `turbinesensor` 모듈을 통해 Edge 디바이스에서 전송되는 원격 분석을 확인하려면 명령 프롬프트에 다음 명령을 입력합니다.

    ```cmd/sh
    iotedge logs turbinesensor
    ```

1. 출력을 잠시 관찰합니다.
 
    이 이벤트의 출력은 다음과 같습니다.

    ```cmd/sh
    11/14/2019 22:26:44 - Send Json Event : {"machine":{"temperature":231.599999999999959,"pressure":1.0095600761599359},"ambient":{"temperature":21.430643635304012,"humidity":24},"timeCreated":"2019-11-14T22:26:44.7904425Z"}
    11/14/2019 22:26:45 - Send Json Event : {"machine":{"temperature":531.999999999999957,"pressure":1.0099208337508767},"ambient":{"temperature":20.569532965342297,"humidity":25},"timeCreated":"2019-11-14T22:26:45.2901801Z"}
    Received message
    Received message Body: [{"command":"reset"}]
    Received message MetaData: {"MessageId":null,"To":null,"ExpiryTimeUtc":"0001-01-01T00:00:00","CorrelationId":null,"SequenceNumber":0,"LockToken":"e0e778b5-60ff-4e5d-93a4-ba5295b995941","EnqueuedTimeUtc":"0001-01-01T00:00:00","DeliveryCount":0,"UserId":null,"MessageSchema":null,"CreationTimeUtc":"0001-01-01T00:00:00","ContentType":"application/json","InputName":"control","ConnectionDeviceId":"turbine-06","ConnectionModuleId":"vm-iot-edge-CP1119","ContentEncoding":"utf-8","Properties":{},"BodyStream":{"CanRead":true,"CanSeek":false,"CanWrite":false,"CanTimeout":false}}
    Resetting temperature sensor..
    11/14/2019 22:26:45 - Send Json Event : {"machine":{"temperature":320.4,"pressure":0.99945886361358849},"ambient":{"temperature":20.940019742324957,"humidity":26},"timeCreated":"2019-11-14T22:26:45.7931201Z"}
    ```
 
1. **turbinesensor**에서 전송하는 온도 원격 분석을 살펴보면 `machine.temperature`의 평균이 `72`를 초과할 때 Stream Analytics 작업에서 **reset** 명령이 전송됨을 확인할 수 있습니다. 이 명령은 Stream Analytics 작업 쿼리에 구성된 작업입니다.

이 연습에서는 Azure IoT Edge 서비스를 활용하여 Edge 디바이스에서 메시지를 처리했습니다.
