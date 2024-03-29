# FFmpeg 사용 명령어

---

### Passthrough CMD

```java
ffmpeg -i input.mp4 -benchmark -c copy output.mp4
```

- 인코딩을 수행하지 않고 원본 파일을 그대로 output.mp4 로 출력함

<br />

### **FFmpeg h264 Encoding CMD**

```java
ffmpeg -i input.mp4 -c:v libx264 -b:v 1500k -preset slow -crf 22 -c:a copy -benchmark output.mp4
```

- `-i input.mp4` : 인코딩할 원본 비디오 파일의 이름을 지정합니다.
- `-c:v libx264` : 비디오 코덱을 libx264 로 지정합니다.
- `-b:v 1500k` : 비디오의 비트레이트를 1500kbps 로 지정합니다.
- `-preset slow` : 인코딩 속도와 출력파일의 품질 사이의 균형을 설정합니다.
    - 느린 속도는 높은 품질을, 빠른 속도는 낮은 품질을 제공합니다.
- `-crf 22` : 출력 파일의 비디오 품질을 설정합니다.
    - 낮은 값은 높은 품질을 의미합니다.
    - 일반적으로 18-28 범위의 값을 사용합니다.
- `-c:a copy` : 오디오를 인코딩하지 않고 원본 파일에서 복사합니다.
    - 이 옵션을 사용하면 소스 파일에서 복사된 오디오가 출력 파일에 그대로 포함됩니다.
- `-benchmark` : 인코딩 작업이 완료된 후 성능 통계를 출력합니다.
- `output.mp4` : 생성된 출력 파일의 이름을 지정합니다.

<br />

### **FFmpeg h265 Encoding CMD**

```java
ffmpeg -i input.mp4 -gpu 0 -c:v hevc_nvenc -b:v 1500k -preset slow -crf 22 -c:a copy -benchmark out.mp4
```

- `-i input.mp4` : 인코딩할 원본 비디오 파일의 이름을 지정합니다.
- `-gpu 0` : FFmpeg 에서 GPU 를 사용하도록 하는 옵션
    - FFmpeg 은 첫번째 GPU 를 사용함
- `-c:v hevc_nvenc` : HEVC(H.265) 비디오 코덱을 사용하고, NVIDIA GPU 하드웨어 가속을 사용하여 인코딩합니다.
- `-b:v 1500k` : 비디오의 비트레이트를 1500kbps 로 지정합니다.
- `-preset slow` : 인코딩 속도와 출력파일의 품질 사이의 균형을 설정합니다.
    - 느린 속도는 높은 품질을, 빠른 속도는 낮은 품질을 제공합니다.
- `-crf 22` : 출력 파일의 비디오 품질을 설정합니다.
    - 낮은 값은 높은 품질을 의미합니다.
    - 일반적으로 18-28 범위의 값을 사용합니다.
- `-c:a copy` : 오디오를 인코딩하지 않고 원본 파일에서 복사합니다.
    - 이 옵션을 사용하면 소스 파일에서 복사된 오디오가 출력 파일에 그대로 포함됩니다.
- `-benchmark` : 인코딩 작업이 완료된 후 성능 통계를 출력합니다.
- `output.mp4` : 생성된 출력 파일의 이름을 지정합니다.

<br />

### FFmpeg AV1 Encoding CMD

```java
ffmpeg -i input.mp4 -c:v libaom-av1 -crf 30 -b:v 0 -c:a copy -pix_fmt yuv420p -f webm -speed 6 -threads 8 -tile-columns 4 -auto-alt-ref 1 -lag-in-frames 25 -g 9999 output.webm
```

- `-i input.mp4` : 입력 파일로 `input.mp4` 를 지정합니다.
- `-c:v libaom-av1` : 비디오 코덱으로 libaom-av1 을 사용합니다.
- `-crf 30` : 비디오의 압축률을 나타내는 값으로 30을 사용합니다. 이 값이 낮을수록 더 높은 품질의 비디오를 얻을 수 있지만 더 큰 파일 크기를 가집니다.
- `-b:v 0` : 비트레이트를 0으로 설정합니다. 이는 제한 없이 비트레이트를 사용하게 하여 비디오 품질을 최대한으로 유지하도록 합니다.
- `-c:a copy` : 오디오 코덱으로 원본 오디오를 그대로 복사합니다.
- `-pix_fmt yuv420p` : 픽셀 포맷으로 yuv420p 를 사용합니다.
- `-f webm` : 출력 파일 포맷을 webm 으로 설정합니다.
- `-speed 6` : 인코딩 속도를 조절하기 위해 6의 값을 사용합니다. 값이 높을수록 빠르게 인코딩되지만 품질이 저하될 가능성이 있습니다.
- `-threads 8` : 사용할 쓰레드의 수를 8개로 설정합니다.
- `-tile-comlumns 4` : tile 의 열 수를 4로 설정합니다.
- `-auto-alt-ref 1` : 자동 대체 참조 프레임 수를 1로 설정합니다.
- `-lag-in-frames 25` : 인코딩 지연 프레임 수를 25로 설정합니다.
- `-g 9999` : IDR 프레임의 주기를 9999로 설정합니다.

<br />


### FFmpeg AV1 Encoding CMD2

```java
ffmpeg -i input.mp4 -c:v libaom-av1 -threads 4 -tile-columns 4 -tile-rows 2 -speed 4 -deadline good -cpu-used 4 output.av1
```

- `-threads` : 이 옵션을 사용하면 인코딩에 사용할 스레드 수를 지정할 수 있습니다.
- `-tile-columns` : 이 옵션을 사용하면 AV1 인코딩 프로세스를 타일 단위로 분할할 수 있습니다. 이것은 다중 코어 CPU 에서 특히 효과적입니다.
  - `-tile-columns 4 -tile-rows 2` : 인코딩 프로세스가 4개의 열과 2개의 행으로 분할됩니다.
- `-speed` : 이 옵션을 사용하면 인코딩 속도와 품질 간의 균형을 설정합니다.
  - `-speed` 값이 낮을수록 인코딩 속도는 더 빠르지만 품질은 떨어집니다.
- `-deadline` :   이 옵션은 인코딩 품질에 대한 제한 기간을 설정합니다.
  - `-deadline good` 옵션을 사용하면 높은 인코딩 품질을 제공합니다.
- `-cpu-used` : 이 옵션은 CPU 사용량을 제어합니다. -cpu-used 값이 높을수록 인코딩 속도는 빨라지지만 CPU 사용량도 많아집니다.


### **FFmpeg h.264 Encoding CMD**

```java
ffmpeg -y -i input.mp4 -c:v libx264 -b:v 1500k -minrate 1500k -maxrate 1500k -bufsize 1500k -vf scale=1920*820 -r 60 -preset slow -c:a copy -benchmark history/output.mp4
```

- `-i input.mp4` : 인코딩할 원본 비디오 파일의 이름을 지정합니다.
- `-c:v libx264` : 비디오 코덱을 libx264 로 지정합니다.
- `-b:v 1500k` : 비디오의 비트레이트를 1500kbps 로 지정합니다.
- `-preset slow` : 인코딩 속도와 출력파일의 품질 사이의 균형을 설정합니다.
  - 느린 속도는 높은 품질을, 빠른 속도는 낮은 품질을 제공합니다.
- `-crf 22` : 출력 파일의 비디오 품질을 설정합니다.
  - 낮은 값은 높은 품질을 의미합니다.
  - 일반적으로 18-28 범위의 값을 사용합니다.
- `-c:a copy` : 오디오를 인코딩하지 않고 원본 파일에서 복사합니다.
  - 이 옵션을 사용하면 소스 파일에서 복사된 오디오가 출력 파일에 그대로 포함됩니다.
- `-benchmark` : 인코딩 작업이 완료된 후 성능 통계를 출력합니다.
- `output.mp4` : 생성된 출력 파일의 이름을 지정합니다.

### **FFmpeg h265 Encoding CMD**

```java
ffmpeg -y -i input.mp4 -gpu 0 -c:v hevc_nvenc -b:v 1000k -minrate 1000k -maxrate 1000k -bufsize 1000k -vf scale=1354*576 -r 30 -preset slow -c:a copy -benchmark history/output.mp4
```

- `-i input.mp4` : 인코딩할 원본 비디오 파일의 이름을 지정합니다.
- `-gpu 0` : FFmpeg 에서 GPU 를 사용하도록 하는 옵션
  - FFmpeg 은 첫번째 GPU 를 사용함
- `-c:v hevc_nvenc` : HEVC(H.265) 비디오 코덱을 사용하고, NVIDIA GPU 하드웨어 가속을 사용하여 인코딩합니다.
- `-b:v 1000k` : 비디오의 비트레이트를 1500kbps 로 지정합니다.
- `-preset slow` : 인코딩 속도와 출력파일의 품질 사이의 균형을 설정합니다.
  - 느린 속도는 높은 품질을, 빠른 속도는 낮은 품질을 제공합니다.
- `-crf 22` : 출력 파일의 비디오 품질을 설정합니다.
  - 낮은 값은 높은 품질을 의미합니다.
  - 일반적으로 18-28 범위의 값을 사용합니다.
- `-c:a copy` : 오디오를 인코딩하지 않고 원본 파일에서 복사합니다.
  - 이 옵션을 사용하면 소스 파일에서 복사된 오디오가 출력 파일에 그대로 포함됩니다.
- `-benchmark` : 인코딩 작업이 완료된 후 성능 통계를 출력합니다.
- `output.mp4` : 생성된 출력 파일의 이름을 지정합니다.

### 성능 테스트를 위한 ShellScript

---

성능테스트를 위해 ShellScript 를 만들었습니다.

테스트 Set 을 모두 일일이 실행하고, 기록하고 종료할 수 없기 때문에 실행 결과를 result.txt 파일에 기입하게 하였습니다.

출력파일은 history 디렉터리에 넣었습니다.

```java
#!/bin/bash

result_file="result.txt"

ffmpeg_cmds=(
	"ffmpeg -y -i input1.mp4 -c:v libx264 -b:v 1500k -minrate 1500k -maxrate 1500k -bufsize 1500k -vf scale=1920*820 -r 60 -preset slow -c:a copy -benchmark history/ouput1.mp4"
  "ffmpeg -y -i input2.mp4 -gpu 0 -c:v hevc_nvenc -b:v 800k -minrate 800k -maxrate 800k -bufsize 800k -vf scale=960*410 -r 30 -preset slow -c:a copy -benchmark history/output2.mp4"
)

for (( i=0; i<${#ffmpeg_cmds[@]}; i++ ))
do
    echo "FFmpeg command: ${ffmpeg_cmds[$i]}"
    echo "Encoding ${i+1}/${#ffmpeg_cmds[@]} start..."
    echo "--------------------------------------------------" >> $result_file
    echo "FFmpeg command: ${ffmpeg_cmds[$i]}" >> $result_file
    echo "--------------------------------------------------" >> $result_file
    echo "" >> $result_file
    ${ffmpeg_cmds[$i]} 2>&1 | tee -a $result_file
    echo "" >> $result_file
    echo "Encoding ${i+1}/${#ffmpeg_cmds[@]} complete."
done

```