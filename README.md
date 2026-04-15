📝 README 및 포트폴리오용 기술 기술 (Troubleshooting 중심)
"영상 좌표의 한계를 논리적 좌표 변환으로 극복하다"

[Challenge]
축사 내 한우 밀집도가 높아지면 개체 간 겹침(Occlusion)으로 인해 기존 Detection 모델만으로는 개체별 활동량 산출이 불가능했습니다. 특히 원근법으로 인한 좌표 왜곡이 추적의 정합성을 해치는 주요 원인이었습니다.

[Solution]

오브젝트 풀링: 개체 데이터를 효율적으로 관리하기 위해 풀링 시스템을 도입, 빈번한 객체 생성/소멸에 따른 오버헤드를 방지했습니다.

미니맵 2차 검증: 영상의 XY 좌표를 가상의 2D 평면 좌표로 변환했습니다. 객체가 사라졌다 다시 나타날 때, 미니맵 상의 이전 이동 궤적과 현재 위치의 유클리드 거리를 계산하여 동일 개체 여부를 판별하는 2차 검증 로직을 직접 구현했습니다.

[Result]
외부 라이브러리에만 의존하지 않고 도메인 특성에 맞는 검증 로직을 추가함으로써, 데이터의 연속성을 확보하고 최종적으로 프로젝트 우수상이라는 성과를 거두었습니다.


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
