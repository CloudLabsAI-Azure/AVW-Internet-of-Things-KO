# ���� 4: Azure IoT Edge ����

## �ó�����

�����Ǵ� ����̽� ���� �ſ� ������ �뷮�� �����Ͱ� �����Ǿ� Ŭ����� ���۵˴ϴ�. �׷��� Edge�� ���ڸ������� �����ϱⰡ ������� ���� �ֽ��ϴ�.

Fabrikam, Inc.������ IoT Edge ����Ʈ���� ����̽��� ����Ͽ� ��� ó�� �����ϵ��� Edge�� ���ڸ������� �����Ϸ��� �մϴ�. ������ �� �Ϻκ��� Ŭ����� ��� ������ �����Դϴ�. IoT Edge�� ������ ���ڸ������� �����ϸ� ���� ��Ʈ��ũ �ӵ��� ������ �����͸� ó���ϰ� ������ ���� ��ġ�� ���� �� �ֽ��ϴ�.

�̸� ���� Azure IoT Edge �ַ���� ������Ÿ���� �ۼ��ؾ� �մϴ�. ���� ��� �µ��� ����ϴ� �� ����� Stream Analytics ����� ����̽��� �����մϴ�. �׸��� ���μ��� ���� ���� �ʰ��Ǹ� ��� �˸��� �����մϴ�.

## ����

�� ���������� Edge ����̽��� Stream Analytics ����� �����ϰ�, ���μ��� ���� ���� �ʰ��Ǹ� ��� �˸��� �����մϴ�. �̸� �ۼ��Ǿ� �� ������ ����� IoT Edge ����̽��� ������ Linux ��� Azure VM�� �����˴ϴ�.

�� ���������� ���� �۾��� �Ϸ��մϴ�.

* IoT Edge VM�� ����
* Edge ����̽��� Edge ��� �߰�
* Edge ����̽��� Azure Stream Analytics Edge ��� ����

Azure IoT Edge���� Ŭ���忡�� ����Ǵ� Ŭ���� ���񽺿� ����̽����� ����Ǵ� ��Ÿ���� �Բ� ���ԵǾ� �ֽ��ϴ�. ��Ÿ���� ����̽����� ��ũ�÷θ� �����ϰ� �����մϴ�. �� ��ũ�÷δ� ��ü �ó����� �ۼ��� ���� Ư�� ������ �����ؾ� �ϴ� �����̳� �������� �����Ǿ� �ֽ��ϴ�. IoT Edge�� IoT Hub�� ���� �����˴ϴ�. Azure IoT Edge������ Ŭ���� ���񽺸� ����Ͽ� �����ϴ� ��ũ�ε带 Edge ����̽����� ������ �� �ֽ��ϴ�. �� ��ũ�ε�� docker ȣȯ �����̳ʸ� ����Ͽ� �����Ǵ� ����Դϴ�. �̷��� ����� �ΰ� ���� ���ø����̼��̳� Azure �� Ÿ�� ������ ���� �ְ� ����Ͻ� ���� ���� �ֽ��ϴ�. IoT Edge�� ���� �ڼ��� ������ ���� ��ũ�� �����ϼ���. ```https://docs.microsoft.com/en-us/azure/iot-edge/about-iot-edge```

## �ַ�� ��Ű��ó
 
  ![�� 04 ��Ű��ó](media/lab4_architecture.png)

### �۾� 1: IoT Edge VM�� ����

�� �۾������� IoT Edge VM�� ������ ���� ����̽����� Azure IoT Edge�� ����Ǵ��� Ȯ���մϴ�.

1. Azure Portal �޴����� **���ҽ� �׷�**�� Ŭ���մϴ�.
 
1. **iot-{deployment-id}** ���ҽ� �׷쿡�� IoT Edge ���� �ӽ� **linuxagentvm-{deployment-id}**��(��) Ŭ���մϴ�.

1. **����** â ���ʿ��� **����**�� Ŭ���ϰ� **SSH**�� Ŭ���մϴ�.

1. **����** â�� **4. VM�� �����Ϸ��� �Ʒ��� ���� ��� ����** �Ʒ����� ���� ����� �����մϴ�.

    �� ����� VM�� IP �ּҿ� ������ ����� �̸��� ���Ե� ���� �ӽſ� �����ϴ� �� ����� �� �ִ� ���� SSH ����Դϴ�. ����� ������ `ssh demouser@52.170.205.79`�� �����ϴ�.

    > **����**: ������ ���� ��ɿ��� **-i <private key path>**�� ���ԵǾ� �ֽ��ϴ�. �ؽ�Ʈ �����⸦ ����� ����� �� �κ��� ������ ���� ������Ʈ�� ����� Ŭ�����忡 �����ϼ���.
 
1. **```https://shell.azure.com```**���� �̵��Ͽ� Azure Cloud Shell�� ���� **Bash**�� �����մϴ�.

1. **��� ���� ǥ��**�� Ŭ���ϰ� ���� ���� ������ �Է��մϴ�.

   * ���ҽ� �׷�: **���� �׸� ���** -> **iot-{deployment-id}** ����
   * ���丮�� ����: **���� �����**�� �����ϰ� **cloudstore{deployment-id}** �Է�
   * ���� ����: **���� �����**�� �����ϰ� **blob** �Է�
   
     >   **����**: - **deployment-id** ���� ������ ȯ�� ���� ���� ���������� Ȯ���� �� �ֽ��ϴ�.
        
   ![���丮�� ������ ����� ���� ���� ��θ� ���� �ִ� Azure Portal ��ũ����.](media/storageaccount01.png)

1. Azure Cloud Shell ��� ������Ʈ���� �ؽ�Ʈ �����⿡�� ������Ʈ�� `ssh` ����� �ٿ��ְ� **Enter** Ű�� �����ϴ�.

1. **Are you sure you want to continue connecting?(��� �����Ͻðڽ��ϱ�?)**�̶�� �޽����� ǥ�õǸ� `yes(��)`�� �Է��ϰ� **Enter** Ű�� �����ϴ�.

1. ��ȣ�� �Է��϶�� �޽����� ǥ�õǸ� **Password.1!!**�� �Է��ϰ� **Enter** Ű�� �����ϴ�.

   ![Linuxagent ���� �ӽſ� �����ϴ� ��.](media/vmlogin.png)

1. VM�� ����Ǹ� �͹̳� ��� ������Ʈ�� ����Ǿ� ������ ���� Linux VM�� �̸��� ǥ�õ˴ϴ�.

    ```cmd/sh
    demouser@linuxagentvm-{deployment-id}:~$
    ```

    ���� ����Ǿ� �ִ� VM�� Ȯ���� �� �ֽ��ϴ�.

    > **�߿�:** VM�� ������ ���� ���� ��ġ���� ���� Edge VM�� ������Ʈ�� ��ġ�϶�� �޽����� ǥ�õ� ���ɼ��� �����ϴ�.  �� �������� �ش� �޽����� �����մϴ�. �׷��� ���δ��� ȯ�濡���� �׻� Edge ����̽��� �ֽ� ���·� �����ؾ� �մϴ�.

1. VM�� Azure IoT Edge ��Ÿ���� ��ġ�Ǿ� �ִ��� Ȯ���Ϸ��� ���� ����� �����մϴ�.

    ```cmd/sh
    iotedge version
    ```

    �� ����� �����ϸ� ���� �ӽſ� ���� ��ġ�Ǿ� �ִ� Azure IoT Edge ��Ÿ���� ������ ��µ˴ϴ�.
    IoT Edge ����̽����� IoT Edge ��Ÿ���� ��ġ�Ǿ� �ֽ��ϴ�. IoT Edge ��Ÿ���� ����̽��� IoT Edge ����̽��� �����ϴ� ���α׷� �÷����Դϴ�. ���� �����Ǵ� IoT Edge ��Ÿ�� ���� ��Ҹ� ���� IoT Edge ����̽��� �������� �ڵ带 �����ϰ� ����� IoT Hub�� ������ �� �ֽ��ϴ�.

### �۾� 2: Edge ����̽��� Edge ��� �߰�

�� ���������� Simulated Temperature Sensor�� ����� ���� IoT Edge ���� �߰��� ���� IoT Edge ����̽����� ����ǵ��� �����մϴ�.

IoT Edge ����� �����̳ʷ� �����Ǵ� ���� ���� ��Ű���Դϴ�.

IoT Edge ����� ���� Ŭ���� ��ũ�ε带 IoT ����̽����� ���� ����ǵ��� ������ �� �ֽ��ϴ�. IoT Edge ����� IoT Edge���� �����ϴ� �ּ� ��� �����Դϴ�. IoT Edge ����� ����ϸ� Ŭ���尡 �ƴ� ����̽����� �����͸� �м��� �� �ֽ��ϴ�. ��ũ�ε� �� �Ϻθ� ������ �̵��ϸ� ����̽��� �� ª�� �ð� ���� Ŭ����� �޽����� ������ �� ������, �̺�Ʈ�� ���� ������ ������ �� �ֽ��ϴ�.

1. �ʿ��� ��� Azure ���� �ڰ� ������ ����Ͽ� Azure Potal�� �α����մϴ�.
 
1. ���ҽ� �׷� Ÿ�Ͽ���**iothub-{deployment-id}**��(��) Ŭ���Ͽ� IoT Hub�� ���ϴ�.

1. **IoT Hub** ���̵� ������ **�ڵ� ����̽� ����** �Ʒ����� **IoT Edge**�� Ŭ���մϴ�.

1. IoT Edge ����̽� ��Ͽ��� **turbine-06**�� Ŭ���մϴ�.

1. **turbine-06** ���̵��� **���** �ǿ��� ���� ����̽������� �����Ǿ� �ִ� ��� ����� ǥ�õ˴ϴ�.

    ���� IoT Edge ����̽����� IoT Edge ��Ÿ���� �Ϻκ��� Edge ������Ʈ(`$edgeAgent`) �� Edge ���(`$edgeHub`) ��⸸ �����Ǿ� �ֽ��ϴ�.

1. **turbine-06** ���̵� ���ʿ��� **��� ����**�� Ŭ���մϴ�.

1. **����̽����� ��� ����: turbine-06** ���̵忡�� **IoT Edge ���** ������ ã���ϴ�.

1. **IoT Edge ���** �Ʒ����� **�߰�**�� Ŭ���ϰ� **IoT Edge ���**�� Ŭ���մϴ�.

1. **IoT Edge ��� �߰�** â�� **IoT Edge ��� �̸�** �Ʒ��� **turbinesensor**�� �Է��մϴ�.

    �׷��� ����� ���� ����� �̸��� "turbinesensor"�� �����˴ϴ�.

1. **�̹��� URI** �Ʒ��� **asaedgedockerhubtest/asa-edge-test-module:simulated-temperature-sensor**�� �Է��մϴ�.

    > **����**: �� �̹����� ��ǰ �׷쿡�� �� �׽�Ʈ �ó����� ������ ���� �����ϴ� Docker Hub�� �Խõ� �̹����Դϴ�.

1. ������ ���� �����Ϸ��� **��� �� ����**�� Ŭ���մϴ�.

1. ��� �ֿ� ���� �ʿ��� �Ӽ��� �����Ϸ��� ���� JSON�� �Է��մϴ�.

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

    �� JSON�� ��� ���� �ʿ��� �Ӽ��� �����Ͽ� Edge ����� �����մϴ�.

1. ���̵� �Ʒ��ʿ��� **�߰�**�� Ŭ���մϴ�.

1. **����̽����� ��� ����: turbine-06** ���̵� �Ʒ��ʿ��� **����: ��� >**�� Ŭ���մϴ�.

1. �⺻ ��δ� �̹� �����Ǿ� �ֽ��ϴ�.

    * �̸�: **route**
    * ��: `FROM /messages/* INTO $upstream`

    �� ��δ� IoT Edge ����̽��� ���ԵǾ� �ִ� ��� ����� �޽����� ��� IoT Hub�� �����մϴ�.

1. **���� + �����**�� Ŭ���մϴ�.

1. **����** �Ʒ��� ǥ�õ� ���� �Ŵ��佺Ʈ�� ��� �����մϴ�. 

    ���⼭ Ȯ���� �� �ִ� ��ó�� IoT Edge ����̽��� ���� �Ŵ��佺Ʈ�� JSON �����̹Ƿ� ������ ���� Ȯ���� �� �ֽ��ϴ�.

    `properties.desired` ���� �Ʒ��� `modules` ���ǿ��� IoT Edge ����̽��� ������ IoT Edge ����� ����˴ϴ�. ��� ����� �̹��� URI�� �����̳� ������Ʈ�� �ڰ� ���� �� ���ǿ� ���ԵǾ� �ֽ��ϴ�.

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

    JSON���� �Ʒ����� **$edgeHub** ���ǿ��� Edge ����� �ʿ��� �Ӽ��� ���ԵǾ� �ֽ��ϴ�. �׸��� ��� ����/��⿡�� IoT Hub�� �̺�Ʈ�� ������ϱ� ���� ����� ������ ���ԵǾ� �ֽ��ϴ�.

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

    JSON���� �� �Ʒ������� �������� **turbinesensor** ����� ������ �ֽ��ϴ�. ���⼭ `properties.desired` ���ǿ��� Edge ��� ������ �ʿ��� �Ӽ��� ���ԵǾ� �ֽ��ϴ�.

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

1. ���̵� �Ʒ��ʿ��� **�����**�� Ŭ���Ͽ� ����̽��� ��� ������ �Ϸ��մϴ�.

1. ���� **turbine-06** ���̵��� **���** �Ʒ��� **turbinesensor**�� �����˴ϴ�.

    > **����**: ����� ó������ ǥ���� ���� **���� ��ħ**�� Ŭ���ؾ� �� ���� �ֽ��ϴ�.

    **turbinesensor**�� ��Ÿ�� ���´� ���� ������� �ʾ��� ���� �ֽ��ϴ�.

1. ���̵� ���ʿ��� **���� ��ħ**�� Ŭ���մϴ�.

1. ���� **turbinesensor** ����� **��Ÿ�� ����**�� **running(���� ��)**���� �����˴ϴ�.

    �׷��� ���� ������� ������ ��� ��ٷȴٰ� ���̵带 �ٽ� ���� ���� ���ϴ�.
 
1. Cloud Shell ������ ���� ���� �ʾ����� ���ϴ�.

    `vm-iot-edge-{deployment-id}` ���� �ӽſ� �� �̻� ����Ǿ� ���� ������ �տ����� ���� **SSH**�� ����Ͽ� �����մϴ�.

1. IoT Edge ����̽����� ���� ����ǰ� �ִ� ����� ����� ǥ���Ϸ��� Cloud Shell ��� ������Ʈ���� ���� ����� �Է��մϴ�.

    ```cmd/sh
    iotedge list
    ```

1. ����� ����� ������ �����ϴ�. 

    ```cmd/sh
    demouser@linuxagentvm-{deployment-id}:~$ iotedge list
    NAME             STATUS           DESCRIPTION      CONFIG
    edgeHub          running          Up a minute      mcr.microsoft.com/azureiotedge-hub:1.0
    edgeAgent        running          Up 26 minutes    mcr.microsoft.com/azureiotedge-agent:1.0
    turbinesensor    running          Up 34 seconds    asaedgedockerhubtest/asa-edge-test-module:simulated-temperature-sensor
    ```

    ���ôٽ��� `turbinesensor`�� ���� ���� ��� �� �ϳ��� �����˴ϴ�.

1. ��� �α׸� Ȯ���Ϸ��� ���� ����� �Է��մϴ�.

    ```cmd/sh
    iotedge logs turbinesensor
    ```

    ����� ����� ������ �����ϴ�.

    ```cmd/sh
    demouser@linuxagentvm-{deployment-id}:~$ iotedge logs turbinesensor
    11/14/2019 18:05:02 - Send Json Event : {"machine":{"temperature":41.199999999999925,"pressure":1.0182182583425192},"ambient":{"temperature":21.460937846433808,"humidity":25},"timeCreated":"2019-11-14T18:05:02.8765526Z"}
    11/14/2019 18:05:03 - Send Json Event : {"machine":{"temperature":41.599999999999923,"pressure":1.0185790159334602},"ambient":{"temperature":20.51992724976499,"humidity":26},"timeCreated":"2019-11-14T18:05:03.3789786Z"}
    11/14/2019 18:05:03 - Send Json Event : {"machine":{"temperature":41.999999999999922,"pressure":1.0189397735244012},"ambient":{"temperature":20.715225311096397,"humidity":26},"timeCreated":"2019-11-14T18:05:03.8811372Z"}
    ```

    `iotedge logs` ����� ����ϸ� ��� Edge ����� ��� �α׸� Ȯ���� �� �ֽ��ϴ�.

1. Simulated Temperature Sensor ����� �޽��� 500���� ������ �Ŀ� �����˴ϴ�. ���� ����� �����ϸ� ����� �ٽ� ������ �� �ֽ��ϴ�.

    ```cmd/sh
    iotedge restart turbinesensor
    ```

    ������ ����� �ٽ� �������� �ʾƵ� �˴ϴ�. ���߿� ��⿡�� ���� �м� ������ �����Ǹ� Cloud Shell�� �ٽ� �̵��Ͽ� SSH�� ���� Edge VM�� ������ ���� �� ����� �����ϸ� ����� �ٽ� �����˴ϴ�. �ٽ� ������ ����� ���� �м� ������ �ٽ� �����մϴ�.

### �۾� 3: Azure Stream Analytics�� IoT Edge ���� ����

�տ��� **turbinesensor** ����� ����������, IoT Edge ����̽����� �� ����� ����ǰ� ������ Ȯ���߽��ϴ�. ���� IoT Edge ����̽� ��ü���� �޽����� IoT Hub�� �����ϱ� ���� ó���� �� �ִ� Stream Analytics ����� �߰��� �� �ֽ��ϴ�.

#### Stream Analytics �۾� ����

1. ���ҽ� �׷� Ÿ�Ͽ��� **iot-{deployment-id}**��(��) Ŭ���ϰ� Stream Analytics �۾� **iotedge-streamjob-{deployment-id}**��(��) �����մϴ�.

1. �������� ���̵� ������ **�۾� ��������** �Ʒ����� **�Է�**�� �����ϰ� �Է� �۾� �ϳ�(**temperature**)�� �̹� ���ǵǾ� ������ Ȯ���մϴ�.

1. �������� **�۾� ��������** �Ʒ����� **���**�� �����ϰ� ��� �۾� �ϳ�(**alert**)�� �̹� ���ǵǾ� ������ Ȯ���մϴ�.

1. ���� Ž�� �޴��� **����** �Ʒ����� ���丮�� ���� ������ Ŭ���մϴ�. �׷� ���� iotstorage{deployment-id} ���丮�� ������ �߰��Ǿ����� Ȯ���մϴ�.

#### Stream Analytics �۾� ����

1. Azure Portal���� **iothub-{deployment-id}** IoT Hub ���ҽ��� �̵��մϴ�.

1. ���� Ž�� �޴��� **�ڵ� ����̽� ����** �Ʒ����� **IoT Edge**�� Ŭ���մϴ�.

1. **����̽� ID** �Ʒ����� **turbine-06**�� Ŭ���մϴ�.

1. **turbine-06** â ���ʿ��� **��� ����**�� Ŭ���մϴ�.

1. **����̽����� ��� ����: turbine-06** â���� **IoT Edge ���** ������ ã���ϴ�.

1. **IoT Edge ���** �Ʒ����� **�߰�**�� Ŭ���ϰ� **Azure Stream Analytics ���**�� Ŭ���մϴ�.

1. **Edge ����** â�� **����** �Ʒ����� �� ���������� ��� ���� ������ ���õǾ� �ִ��� Ȯ���մϴ�.

1. **Edge �۾�** ��Ӵٿ�� **iotedge-streamjob-{deployment-id}** Stream Analytics �۾��� ���õǾ� �ִ��� Ȯ���մϴ�.

    > **����**: �۾��� �̹� ���õǾ� �ִµ� **����** ���ߴ� ��� �Ұ����ϵ��� �����Ǿ� ���� ���� �ֽ��ϴ�. �� ��� **Edge �۾�** ��Ӵٿ��� �ٽ� ���� **iotedge-streamjob-{deployment-id}** �۾��� �ٽ� �����ϸ� �˴ϴ�. �׷��� **����** ���߰� ��� �����ϵ��� �����˴ϴ�.

1. â �Ʒ��ʿ��� ������ Ŭ���մϴ�.

    ������ �� �� ���� �ɸ� �� �ֽ��ϴ�.

1. **����̽����� ��� ����: turbine-06** ���̵� �Ʒ��ʿ��� **���� + �����**�� Ŭ���մϴ�.

1. **���� + �����** �ǿ��� ���� **���� Manifest** JSON�� ��� ������ ����� ���ǿ� Stream Analytics ���� ������Ʈ�Ǿ� ������ Ȯ���մϴ�.

1. ���̵� �Ʒ��ʿ��� **�����**�� Ŭ���մϴ�.

1. Edge ��Ű���� ���������� �ԽõǸ� **IoT Edge ���** ���� �Ʒ��� �� ASA ����� �����˴ϴ�.

1. **IoT Edge ���** �Ʒ����� **iotedge-streamjob-{deployment-id}**��(��) Ŭ���մϴ�.

    �� ����� ��� Edge ����̽��� �߰��� Stream Analytics ����Դϴ�.

1. **IoT Edge ��� ������Ʈ** â���� **�̹��� URI**�� ǥ�� Azure Stream Analytics �̹����� ����Ű���� �����Ǿ� �ֽ��ϴ�.

    ```text
    mcr.microsoft.com/azure-stream-analytics/azureiotedge:1.0.7
    ```

    �� �̹����� IoT Edge ����̽��� �����Ǵ� ��� ASA �۾��� ���Ǵ� �Ͱ� ���� �̹����Դϴ�.

    > **����**:  �����Ǿ� �ִ� **�̹��� URI** ���κ��� ���� ��ȣ���� Stream Analytics ����� ���� ������ ���� �ֽ� ������ �ݿ��˴ϴ�.

1. ��� ���� �⺻������ �����ϰ� **IoT Edge ����� ���� ���** â�� �ݽ��ϴ�.

1. **����̽����� ��� ����: turbine-06** ���̵忡�� **����: ��� >**�� Ŭ���մϴ�.

    ���� ������� ǥ�õ˴ϴ�.

1. ���ǵǾ� �ִ� �⺻ ��θ� ������ 3�� ��η� �ٲߴϴ�.
   
   > **����**: `iotstreamjob-edge-{deployment-id}` �ڸ� ǥ���ڴ� ���� Azure Stream Analytics �۾� ����� �̸����� �ٲټ���.
   
    * ��� 1
        * �̸�: **telemetryToCloud**
        * ��: `FROM /messages/modules/turbinesensor/* INTO $upstream`
    * ��� 2
        * �̸�: **alertsToReset**
        * ��: `FROM /messages/modules/iotedge-streamjob-{deployment-id}/* INTO BrokeredEndpoint("/modules/turbinesensor/inputs/control")`
    * ��� 3
        * �̸�: **telemetryToAsa**
        * ��: `FROM /messages/modules/turbinesensor/* INTO BrokeredEndpoint("/modules/iotedge-streamjob-{deployment-id}/inputs/temperature")`

    > **����**: **����**�� Ŭ���Ͽ� ��� �� ��� �̸� ����� Ȯ���ϰ� **����**�� Ŭ���Ͽ� �� �ܰ�� ���ƿ� ���� �ֽ��ϴ�.

    ���⼭ �����ϴ� ��δ� ������ �����ϴ�.

    * **telemetryToCloud** ��δ� `turbinesensor` ��� ����� ��� �޽����� Azure IoT Hub�� �����մϴ�.
    * **alertsToReset** ��δ� Stream Analytics ��� ����� ��� ��� �޽����� **turbinesensor** ����� �Է����� �����մϴ�.
    * **telemetryToAsa** ��δ� `turbinesensor` ��� ����� ��� �޽����� Stream Analytics ��� �Է����� �����մϴ�.

1. **����̽����� ��� ����: turbine-06** ���̵� �Ʒ��ʿ��� **���� + �����**�� Ŭ���մϴ�.

1. **���� + �����** �ǿ��� ���� **���� Manifest** JSON�� ��� ������ ����� ���ǿ� Stream Analytics ���� ������Ʈ�Ǿ� ������ Ȯ���մϴ�.

1. `turbinesensor` Simulated Temperature Sensor ����� JSON ������ ������ �����ϴ�.

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

1. �տ��� ������ ����� JSON ����, �׸��� JSON ���� ���ǿ��� �̷��� ��ΰ� �����Ǵ� ����� ������ �����ϴ�.

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

1. ���̵� �Ʒ��ʿ��� **�����**�� Ŭ���մϴ�.

#### ������ Ȯ��

1. **SSH**�� ���� **IoT Edge ����̽�**�� �����ߴ� **Cloud Shell** �������� �ٽ� �̵��մϴ�.  

    > **����**: ������ �����ų� �ð��� �ʰ��Ǿ����� �ٽ� �����մϴ�. �տ����� ���� `SSH` ����� �����ϰ� �α����մϴ�.

1. ����̽��� ������ ����� ����� ǥ���Ϸ��� ��� ������Ʈ���� ���� ����� �Է��մϴ�.

    ```cmd/sh
    iotedge list
    ```

    �� Stream Analytics ����� IoT Edge ����̽��� �����Ƿ��� �ð��� �ټ� �ɸ� �� �ֽ��ϴ�. ������ �Ϸ�Ǹ� �� ����� ��� ��¿� ����� ǥ�õ˴ϴ�.

    ```cmd/sh
    demouser@linuxagentvm-{deployment-id}:~$ iotedge list
    NAME                       STATUS           DESCRIPTION      CONFIG
    iotedge-streamjob-232539   running          Up a minute      mcr.microsoft.com/azure-stream-analytics/azureiotedge:1.0.5
    edgeAgent                  running          Up 6 hours       mcr.microsoft.com/azureiotedge-agent:1.0
    edgeHub                    running          Up 4 hours       mcr.microsoft.com/azureiotedge-hub:1.0
    turbinesensor              running          Up 4 hours       asaedgedockerhubtest/asa-edge-test-module:simulated-temperature-sensor
    ``` 

    > **����**:  Stream Analytics ����� ��Ͽ� ǥ�õ��� ������ 1~2�� ���� ��ٷȴٰ� �ٽ� �õ��� ������. IoT Edge ����̽����� ��� ������ ������Ʈ�Ƿ��� �ð��� �ټ� �ɸ� �� �ֽ��ϴ�.

1. `turbinesensor` ����� ���� Edge ����̽����� ���۵Ǵ� ���� �м��� Ȯ���Ϸ��� ��� ������Ʈ�� ���� ����� �Է��մϴ�.

    ```cmd/sh
    iotedge logs turbinesensor
    ```

1. ����� ��� �����մϴ�.
 
    �� �̺�Ʈ�� ����� ������ �����ϴ�.

    ```cmd/sh
    11/14/2019 22:26:44 - Send Json Event : {"machine":{"temperature":231.599999999999959,"pressure":1.0095600761599359},"ambient":{"temperature":21.430643635304012,"humidity":24},"timeCreated":"2019-11-14T22:26:44.7904425Z"}
    11/14/2019 22:26:45 - Send Json Event : {"machine":{"temperature":531.999999999999957,"pressure":1.0099208337508767},"ambient":{"temperature":20.569532965342297,"humidity":25},"timeCreated":"2019-11-14T22:26:45.2901801Z"}
    Received message
    Received message Body: [{"command":"reset"}]
    Received message MetaData: {"MessageId":null,"To":null,"ExpiryTimeUtc":"0001-01-01T00:00:00","CorrelationId":null,"SequenceNumber":0,"LockToken":"e0e778b5-60ff-4e5d-93a4-ba5295b995941","EnqueuedTimeUtc":"0001-01-01T00:00:00","DeliveryCount":0,"UserId":null,"MessageSchema":null,"CreationTimeUtc":"0001-01-01T00:00:00","ContentType":"application/json","InputName":"control","ConnectionDeviceId":"turbine-06","ConnectionModuleId":"vm-iot-edge-CP1119","ContentEncoding":"utf-8","Properties":{},"BodyStream":{"CanRead":true,"CanSeek":false,"CanWrite":false,"CanTimeout":false}}
    Resetting temperature sensor..
    11/14/2019 22:26:45 - Send Json Event : {"machine":{"temperature":320.4,"pressure":0.99945886361358849},"ambient":{"temperature":20.940019742324957,"humidity":26},"timeCreated":"2019-11-14T22:26:45.7931201Z"}
    ```
 
1. **turbinesensor**���� �����ϴ� �µ� ���� �м��� ���캸�� `machine.temperature`�� ����� `72`�� �ʰ��� �� Stream Analytics �۾����� **reset** ����� ���۵��� Ȯ���� �� �ֽ��ϴ�. �� ����� Stream Analytics �۾� ������ ������ �۾��Դϴ�.

�� ���������� Azure IoT Edge ���񽺸� Ȱ���Ͽ� Edge ����̽����� �޽����� ó���߽��ϴ�.
