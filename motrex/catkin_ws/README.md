# Autocal 실행 가이드

## 1. 환경 설정 및 빌드 (Build & Environment)

``` bash
cd ~/motrex/catkin_ws
catkin_make
source devel/setup.bash
chmod +x src/autocal/src/nodes/*.py src/autocal/src/*.py
pip3 install --upgrade pandas numexpr
```

------------------------------------------------------------------------

## 2. 런치파일 설정 모드 (Launch File Configuration)

### 로스백(Rosbag) 사용 시

``` xml
<arg name="use_offline_provider" default="true"/>
```

### 실센서(Real Sensor) 사용 시

``` xml
<arg name="use_offline_provider" default="false"/>
```

------------------------------------------------------------------------

## 3. 실행 모드별 명령어 (Execution Steps)

### (A) 통합 실행 (Consumer GUI)

``` bash
roslaunch autocal autocal.launch
```

### (B) 개발자용 실행 (Developer GUI)

``` bash
roslaunch autocal autocal_dev.launch
```

### (C) 실센서 전용 실행 (Radar + Camera 드라이버)

**1번 터미널**

``` bash
roslaunch autocal autocal_real_sensors.launch
```

**2번 터미널**

``` bash
roslaunch autocal autocal.launch
```

또는

``` bash
roslaunch autocal autocal_dev.launch
```

### (D) 토픽 개별 점검 (Quick Check)

``` bash
roscore
```

각 노드 개별 실행

``` bash
rosrun mv_camera camera_node.py
rosrun 4fn_basic_viewer radar_node _ip_addr:=192.168.137.15 _z_offset:=0
```

토픽 주기 확인

``` bash
rostopic hz /camera/image_raw
rostopic hz /point_cloud
```

------------------------------------------------------------------------

## 4. 로스백 및 시각화 도구 (Bag File & Visualization)

시뮬레이션 시간 사용

``` bash
rosparam set use_sim_time true
```

백파일 재생

``` bash
rosbag play --clock [파일명].bag
```

인덱스 오류 복구

``` bash
rosbag reindex [파일경로]
```

백파일 정보 확인

``` bash
rosbag info [파일경로]
```

------------------------------------------------------------------------

## 5. 배치 반복 실행 (Batch Runner)

``` bash
rosrun autocal autocal_batch_runner.py _iterations:=10
```
