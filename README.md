# Autonomous-Lane-Changing
**자율주행 차선변경 모듈 개발**  


## Description
본 프로젝트는 고가의 센서(레이더, 라이다)를 제외한 저렴한 비용의 카메라만을 이용하여 자율 주행 기술의 일부를 구현해보자는 아이디어로 시작하였다.
총 두대의 카메라를 이용하여, 전/후방의 차량을 탐지하고 거리를 측정, 차선을 인식한다. 탐색한 객체와의 거리 정보와 차선 정보(실선/점선)을 종합적으로 판단하여 차선변경을 진행한다.

## Core Features
### 차선 인식 ### 
+ **Find contours**  
ROI로 지정한 영역에 대해 contour 탐색  
+ **Filtering**  
탐색한 contour의 영역이 일정값 이하인 것은 노이즈로 판단하여 제거  
+ **차선 탐색**  
탐색한 contour 중 기울기와 좌표를 이용하여 차선 검출  
+ **차선 업데이트**  
이전 프레임에서 찾은 차선과 새로 찾은 contour의 좌표 비교를 통해 가능한 차선 업데이트  
+ **실선/점선 구분**  
찾은 차선이 ROI내에서 차지하는 길이를 이용하여 실선/점선 구분  
  <img src="https://user-images.githubusercontent.com/71866756/144281129-fed4fad6-0caa-4760-b11e-ccc912d832d6.jpg" width="480" height="240"/> 

### 겍체 탐색 및 거리 측정 ###
+ **객체 탐색**  
전/후방 카메라를 이용하여 차량 탐색 
+ **거리 측정**  
카메라 정보(focal, tilt, height) + 탐색한 차량의 bbox를 이용하여 실거리 측정  
  <img src="https://user-images.githubusercontent.com/71866756/144281330-e04b6fdc-68ac-48e0-85ef-67a47216a069.JPG" width="480" height="240"/>

### 속도 가/감속 ###
+ **감속**  
현재 차선에 위치한 앞차량의 거리가 일정 거리 안으로 들어올 경우 거리에 따라 감속
  <img src="https://user-images.githubusercontent.com/71866756/144720494-bdb5a8ee-2b58-4262-b264-3ad3f5818d96.gif" width="360" height="180"/>
+ **가속**  
현재 차선에 위치한 앞차량의 거리가 멀어지는 경우 가속  
  <img src="https://user-images.githubusercontent.com/71866756/144720489-68bb7343-ce16-4c04-9a53-74cb7f7227aa.gif" width="280" height="220"/>  
### 차선 변경 알고리즘 ###
+ **차선 변경 가능 조건**  
차선이 점선 + 변경할 차선의 차량이 먼 경우  
  <img src="https://user-images.githubusercontent.com/71866756/144720449-bd1b8619-4621-47e0-a751-9208cbb4139b.gif" width="360" height="180"/>
+ **차선 변경 불가능 조건**  
 1.차선이 실선인 경우  
  <img src="https://user-images.githubusercontent.com/71866756/144720483-f12463f0-bdfc-40b8-a41d-43e48fb706a8.gif" width="360" height="180"/>  <br>  
   2.변경할 차선에 차량이 존재하는 경우  
  <img src="https://user-images.githubusercontent.com/71866756/144720473-facf2f09-610d-43c2-991b-97a19a2d1f35.gif" width="360" height="180"/>  <br>  
  3.변경할 차선 후방에서 차량이 접근하는 경우  
  <img src="https://user-images.githubusercontent.com/71866756/144720457-edd14abe-a866-4eda-8f8e-6fd909e177a6.gif" width="360" height="180"/>

## Demonstration video ##
## Env
+ Python 3.6
+ OpenCV
+ yolov4-tiny-288
## yolo weight file ##
https://drive.google.com/drive/folders/1NCf9wIF43LUdXHq3OmoTCzYD8SjHfZj7?usp=sharing
