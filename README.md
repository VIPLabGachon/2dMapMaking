# 2dMapMaking </br>

LeGO-LOAM 패키지는 3D 맵을 생성하는 코드이다. </br>

mapmakingGUI.py는 GUI를 이용해 rosbag 파일을 선택하고 LeGO-LOAM을 실행한다. </br>

Select rosbag을 버튼을 클릭하여 3D 맵을 생성하기 위한 rosbag 파일을 선택한 후, make a map 버튼을 클릭하여 LeGO-LOAM을 실행하여 3D 맵을 생성한다. </br>

stop lego loam  버튼은 레고로암을 멈춰서 맵을 생성하는 버튼이다, 그러나 현재는 프로세스 handler 구현이 완료되지 않았다.</br>

 따라서 현재는 반드시 `ctrl + c`를 터미널 창에 입력하여 LeGO-LOAM을 종료해야 한다.</br>

LeGO-LOAM 종료 후 3D 맵이 `/tmp` 경로에 `finalCloud.pcd` 이름으로 pcd 포맷으로 생성된다. </br>

### 발생 가능한 오류 </br>

```bash
Unable to register with master node [http://172.16.235.182:11311]: master may not be running yet. Will keep trying. 
```

위와 같은 형태의 오류는 ros node가 작동하기 위한 `roscore` 가 실행되지 않는 경우 발생한다. </br>

이를 해결하기 위하여 새로운 터미널 창을 열어서 `roscore` 명령어를 입력한다. </br>

```jsx
roscore\
```
</br>

```bash
[mapOptmization-7] process has died [pid 6638, exit code 127, cmd /home/xxxxxx/catkin_pkg/devel/lib/lego_loam/mapOptmization __name:=mapOptmization ]
```
</br>
위와 같은 오류를 해결하기 위하여 `~/.bashrc` 파일을 열어 </br>

```jsx
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib
```

위의 문자열을 입력하여 환경변수를 수정한다.</br>

실행 전 자신이 사용할 rosbag 파일에 sensor_msgs/PointCloud2 포맷의 topic 명을mapmakingGUI.py 내에rosbag_play_cmd에 입력해야 한다. 기본 값은 `/ouster/points` 로 설정되어 있다.</br>

conv.py 코드는 LeGO-LOAM이 zxy축으로 틀어져있는 좌표값을 xyz로 변경해주는 코드이다. 추가적으로, 필요없는 intensity 값을 제거해서 코드를 가져다 쓰기 더 쉽게 만들어준다.</br>

---

p4.py 코드는 noise를 제거하는 코드이다. hdbscan, voxel filtering, sor을 누적해서 진행하고, 각 단계별 pcd를 저장한다. </br>

noisefilteringGUI.py에서는 p4에서 만든 각각의 pcd에서 z축을 제거해 2d 이미지로 projection하고 보여준다. 그 이미지 중 사용자가 원하는 이미지를 선택해서 저장할 수 있다.</br>

발생할 수 있는 오류</br>

noisefilteringGUI.py 를 실행할 때는 반드시 noisefilteringGUI.py 코드가 있는 터미널 창에서 실행해줘야 한다. 아니면 이런 오류가 발생한다.</br>

```jsx
python: can't open file '[coordinateConversion.py](http://conv.py/)': [Errno 2] No such file or directory
```

---

투두리스트</br>

레고로암 죽이기 만들기</br>

토픽이름 자동화</br>
