🐄 [Project] 한우 성장 모니터링 시스템 (소카이넷)
"영상 좌표의 한계를 논리적 좌표 변환으로 극복하고, 클라우드 인프라를 직접 구축한 AI 관제 솔루션"

🛠️ Tech Stack & Infrastructure
AI Engine: YOLOv7, ByteTrack

Language & Library: Python 3.8, PyTorch, OpenCV, Shapely (기하학 연산)

Infrastructure: AWS EC2 (Ubuntu)

Environment: Docker(선택적), Linux CLI 환경 기반 모델 서빙

🚀 Key Achievements (나의 역할)
1. 객체 추적 신뢰성 강화를 위한 '미니맵 2차 검증' 설계
기존 Detection 모델의 한계를 알고리즘적 논리로 보완했습니다.

좌표 투영 (Perspective Transformation): 원근법에 의해 왜곡된 영상 속 XY 좌표를 가상의 2D 평면(미니맵) 좌표로 변환하여 개체 간 실제 물리적 거리를 계산했습니다.

유클리드 거리 기반 재식별: Shapely 라이브러리를 활용, 개체가 겹침(Occlusion) 후 재등장 시 이전 궤적의 벡터와 현재 위치의 거리를 대조하여 ID를 복원하는 2차 검증 로직을 구현했습니다.

오브젝트 풀링(Object Pooling): 빈번한 객체 생성/소멸로 인한 메모리 오버헤드를 방지하고, 풀 내에서 유효 데이터를 유지하여 시스템 안정성을 확보했습니다.

2. 클라우드 서빙 인프라 구축 및 트러블슈팅
라이브러리 의존성 문제를 해결하며 서버급 모델 구동 환경을 직접 세팅했습니다.

Dependency Management: 서버 환경(Headless)에서 발생하는 cv2 임포트 에러를 libgl1-mesa-glx 설치 및 opencv-python-headless 전환으로 해결했습니다.

GPU 가속 검증: torch.cuda.is_available() 기반의 런타임 체크를 통해 클라우드 인프라의 리소스 활용 최적화 상태를 상시 점검했습니다.

📝 Troubleshooting Log
"로컬 환경과 서버 환경의 격차 해결"

[Challenge] > 로컬에서는 정상 작동하던 OpenCV가 EC2 배포 시 GUI 관련 라이브러리 부재로 인해 런타임 에러(ImportError)를 발생시켰습니다.

[Solution]
단순히 재설치하는 대신, 서버 환경에는 모니터가 없어 디스플레이 관련 라이브러리가 필요 없다는 점에 착안했습니다. apt 패키지 매니저를 통해 메사 그래픽 라이브러리를 설치하거나, 아예 GUI 의존성이 제거된 -headless 버전을 사용하여 배포 사이즈를 최적화했습니다.

🏗️ Deployment Steps
1
Repository & Env Setup
Git clone & Library install
YOLOv7과 ByteTrack 저장소를 클론한 후, 서버 운영에 필수적인 pip3 install -r requirements.txt를 실행합니다.

2
Handling Dependencies
OpenCV & Shapely
서버 환경의 그래픽 라이브러리 이슈 해결을 위해 libgl1-mesa-glx를 수동 설치하고, 2차 검증 로직을 위한 shapely를 추가로 구성합니다.

3
Environment Validation
PyTorch & CUDA check
추론 시작 전 torch.cuda.is_available()을 통해 GPU 가속 여부를 확인하여 딥러닝 연산 효율을 보장합니다.


// EC2 env

	sudo apt update -y
	
	sudo apt install python3-pip

	git clone https://github.com/inbn6619/yolov7.git

	cd yolov7

	git clone https://github.com/inbn6619/ByteTrack.git

	cd ByteTrack

	sudo apt install python3-pip -y

	sudo pip3 install -r requirements.txt

	sudo python3 setup.py develop

	sudo pip3 install cython; pip3 install 'git+https://github.com/cocodataset/cocoapi.git#subdirectory=PythonAPI'

	sudo pip3 install cython_bbox

	sudo pip3 install seaborn

	sudo apt-get update
	sudo apt-get -y install libgl1-mesa-glx

// 추가 라이브러리

	sudo pip3 install shapely

// cv2 Error Check

	version check
		python3
			import cv2
			불러오기 안된다면 인식, 설치 안된거임
			print(cv2.__version__) // 버전확인

// cv2 Error Fix


	sudo apt-get update
	sudo apt-get -y install libgl1-mesa-glx
	or
	sudo pip3 install opencv-python-headless
	
	sudo pip3 install -r requirements.txt



// torch Error Check

	version check
		python3
			import torch
			print(torch.__version__)
	cuda check(gpu on or off)
		python3
			import torch
			torch.cuda.is_available()
				True: GPU ON
				False: GPU OFF
