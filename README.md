# BLE Nurse Project Wiki  
  
## [1] 라즈베리파이 BLE 에이전트 (Container)
라즈베리파이에서 BLE 비콘의 신호 세기를 감지해 MQTT 브로커로 전송합니다.  
  
데이터 포맷은 payload_example에서 확인할 수 있습니다.
> Github 주소 : https://github.com/ble-nurse-project/ble-nurse-agent
<hr>
  
## [2] 데이터 수집 모듈 (Container)
라즈베리파이에서 수집되는 데이터를 수집해, 같은 컨테이너 내에 존재하는 MongoDB에 저장합니다.  
주기적으로, 또는 요청에 의해, 저장된 데이터를 분석해 현재 위치를 MQTT 브로커로 전송합니다.  
  
데이터 포맷은 payload_example에서 확인할 수 있습니다.

<hr>

## [3] 웹 대시보드 서버 (Spring)
-> 시각화 용도입니다. 아직 개발 중에 있습니다.

<hr>

## [4] 라즈베리파이 제어 서버 (Spring : rpi-controller)
라즈베리파이를 제어할 때 쓰입니다. 
231서버의 MySQL을, 195서버의 MQTT 브로커를 사용하고 있습니다.

### MQTT

(1) 라즈베리파이 exec 명령  

**Topic**: ble/command  

**Payload** 원하는 명령어를 입력합니다.
> 아래와 같이 쉘 스크립트를 실행함으로써, 특정 작업을 수행할 수 있습니다.
```
mkdir /temp
git clone https://github.com/alicek106/iot-rpi-command.git /temp
bash /temp/clean_and_add_wifi_info.sh RmCRC_NAT ilove
bash /temp/add_wifi_info.sh sbnet a3760
rm -rf /temp
```  
  
  
(2) 라즈베리파이에 에이전트 컨테이너 생성  

**Topic** : ble/container/create  

**Payload** : 
```
docker run -d --net=host --name agent --restart=always alicek106/ble-nurse-project:0.3 python alicek106-20180304.py {MQTT_SERVER_IP} topic {DELAY}
```
> topic은 서버 내에서 활성화된 라즈베리파이의 이름으로 치환되어 사용됩니다.  
  
  
### Rest API

(1) 라즈베리파이의 연결 상태를 업데이트합니다.  

Endpoint : localhost:8080/api/raspberrypi/status POST  

(2) 라즈베리파이의 연결 상태를 확인합니다.  

Endpoint : localhost:8080/api/raspberrypi/status GET  
