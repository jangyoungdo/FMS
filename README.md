# FMS
![image](https://github.com/user-attachments/assets/6263f144-da66-4257-b925-16312bb5c298)

## 네트워크 세팅
로봇과 관제 PC가 서로 다른 공간에 있음을 가정하기에 가상 네트워크 활용하기로 함

## ZeroTier를 활용
 - 네트워크 생성
 - 실험용 Robot 역할 노트북과 관제 노트북 연결
 - 각 장치에 할당된 IP 확인

# MQTT
## 개요
- 경량 메시지 프로토콜로 IoT 기기 간 데이터 전송에 최적화
- 제한된 네트워크 대역폭과 전력 소비에 효율적

## 주요 특징
### 발행-구독 모델 (Pub/Sub)
- 발행자(Publisher)와 구독자(Subscriber)가 브로커를 통해 데이터 전송
- IoT 환경에서 다수 장치의 효율적인 통신 지원

### 브로커 (Broker)
- 발행자와 구독자 간 중개 역할
- 대표 브로커: Mosquitto, EMQX, HiveMQ

### QoS (Quality of Service)
- QoS 0: 한 번만 전송, 전달 보장 없음
- QoS 1: 적어도 한 번 전달, 중복 가능
- QoS 2: 정확히 한 번 전달, 중복 방지

### 저전력, 저용량 프로토콜
- TCP 기반, 패킷 크기 작아 제한된 대역폭에 적합

### 유지 연결 (Keep Alive)
- 연결 상태 확인을 위해 주기적 PINGREQ 전송, 연결 손실 시 자동 재연결


# MQTT 세팅
### Ubuntu 환경에 세팅
#### 실험용 노트북 ubuntu 20.04에서는 구동
#### 로봇 24.04에서는 구동되는지 궁금
```
 sudo apt update
 sudo apt install mosquitto mosquitto-clients
```
### 실행
```
sudo systemctl start mosquitto
sudo systemctl enable mosquitto  # 부팅 시 자동 실행
```
### MQTT Client 패키지 생성

```
cd ~/Proj/dev-0716/src
ros2 pkg create mqtt_client --build-type ament_python --dependencies rclpy std_msgs
```

### 패키지 실행
```
cd ~/Proj/dev-0716
colcon build --packages-select mqtt_client
source install/setup.bash
ros2 launch mqtt_client mqtt_client_launch.py

```

# 파일 구조
```
mqtt_client/
├── config/
│   └── mqtt_params.yaml       # MQTT 관련 설정 파일 (IP, 포트 등)
├── launch/
│   └── mqtt_client_launch.py   # ROS2 Launch 파일
├── mqtt_client/
│   ├── __init__.py             # 패키지 초기화 파일
│   └── mqtt_client.py          # MQTT 클라이언트 노드 코드
├── resource/
│   └── mqtt_client             # 패키지 리소스 파일 (패키지 인식용 빈 파일)
├── test/
├── package.xml                 # ROS2 패키지 메타데이터 파일
├── setup.cfg                   # 설정 파일
└── setup.py                    # 패키지 설치 설정 파일

```
