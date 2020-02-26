## 1. 새 프로젝트 생성 후 main.cpp파일 로드
## 2. include 경로 설정
C:\Program Files (x86)\IntelSWTools\openvino_2020.1.033\deployment_tools\inference_engine\include<br/>
C:\Program Files (x86)\IntelSWTools\openvino_2020.1.033\inference_engine\samples\cpp\common<br/>
C:\Program Files (x86)\IntelSWTools\openvino_2020.1.033\opencv\include<br/>

> C4996 오류가 날 경우<br/>
프로젝트의 Property Pages -> Configuration Properties -> C/C++ -> Advanced의 Disable Specific Warnings 란에 4996 입력 후 적용

## 3. 링크 경로 설정
C:\Program Files (x86)\IntelSWTools\openvino_2020.1.033\deployment_tools\inference_engine\lib\intel64\Release<br/>
C:\Program Files (x86)\IntelSWTools\openvino_2020.1.033\opencv\lib<br/>

## 4. 입력 추가
inference_engine.lib<br/>
opencv_core420.lib<br/>
opencv_imgcodecs420.lib<br/>

## 5. dll파일 프로젝트 폴더로 복사
inference_engine.dll<br/>
opencv_core420.dll<br/>
opencv_imgcodecs420.dll<br/>
opencv_imgproc420.dll<br/>
tbb.dll<br/>
ngraph.dll<br/>

## 6. 모델 다운로드
```
cd C:\Program Files (x86)\IntelSWTools\openvino_2020.1.033\deployment_tools\open_model_zoo\tools\downloader
```
로 이동 후
```
python donwloader.py --name alexnet
```

## 7. Model Optimizer로 Caffe 모델을 IR형식으로 변경

### 7.1. prerequisites 설치
```
cd C:\Program Files (x86)\IntelSWTools\openvino_2020.1.033\deployment_tools\model_optimizer\install_prerequisites
```
로 이동 후
```
install_prerequisites_caffe.bat
```

### 7.2. Caffe 모델 변환
```
cd C:\Program Files (x86)\IntelSWTools\openvino_2020.1.033\deployment_tools\model_optimizer
```

```
python mo.py --input_model <INPUT_MODEL>.caffemodel
```
```
ex) python mo.py --input_model D:\_shpark200224\alexnet\alexnet.caffemodel
```

## 8. 프로젝트 빌드
Visual Studio 2015에서 빌드하여 실행파일(exe) 생성

## 9. 실행파일 테스트
./x64/release (또는 debug)에 있는 실행파일을 프로젝트 루트경로(sln이 있는 경로)로 복사
```
cd D:\_shpark200224\repo\OpenVINO-vs2015-hello_classification
```
프로젝트 경로로 이동 후
```
OpenVINO-vs2015-hello_classification <path_to_model> <path_to_image> <device_name>
```
명령어로 실행 테스트<br/>
<path_to_model> : Model Optimizer로 변환한 xml파일 경로<br/>
<path_to_image> : 추론할 이미지<br/>
<device_name> : GPU or CPU<br/>

실행 예<br/>
```
OpenVINO-vs2015-hello_classification ./IR/alexnet.xml cat.jpg GPU
```


> * clDNNPlugin.dll 로드 문제 : C:\Program Files (x86)\IntelSWTools\openvino_2020.1.033\deployment_tools\inference_engine\bin\intel64\Release에서 dll파일 복사하여 현재 프로젝트 폴더에 붙여넣기
> * Doesn't support conversion from not dense cv::Mat - jpg이미지 사용시 빌생 -> bmp이미지로 사용
> * Please, make sure that pre-processing library inference_engine_preproc.dll is in D:\_shpark200224\repo\OpenVINO-vs2015-hello_classification
C:\Program Files (x86)\IntelSWTools\openvino_2020.1.033\deployment_tools\inference_engine\bin\intel64\Release에 있는 inference_engine_preproc.dll을 프로젝트 폴더로 복사

## 10. 결과 확인
<img src="/doc/result.png" title="result" alt="result"></img><br/>
