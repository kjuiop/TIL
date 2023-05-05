### 2-pass encoding

---

2-pass encoding 은 비디오 인코딩 방법 중 하나로, 비디오 파일을 더 효율적으로 압축하기 위해 사용됩니다.

이 방법은 두단계의 패스를 통해 비디오를 인코딩합니다.

- pass 1
    - pass 1 은 비디오 파일을 분석하고, 인코딩에 필요한 정보를 수집하는 단계입니다.
    - 이 단계에서는 비디오의 해상도, 비율, 프레임 속도, 비트레이트 등의 정보를 수집하고 첫 번째 패스의 결과물을 저장합니다.
- pass 2
    - pass 2 는 첫 번째 패스에서 수집된 정보를 바탕으로 실제 비디오를 인코딩하는 단계입니다.
    - 이 단계에서는 인코더가 비디오를 처리하고, 높은 압축률을 유지하면서도 최적의 품질을 유지하도록 작업합니다.
    - pass 2 에서는 인코더가 pass 1 에서 생성한 정보를 바탕으로 비디오를 처리하므로, 보다 효율적인 인코딩이 가능합니다.

- 2-pass encoding 장점
    - 단일 패스 인코딩보다 더 오랜 시간이 걸리지만, 더 높은 압축률과 더 좋은 품질을 제공할 수 있습니다.
    - 대규모 비디오 파일을 인코딩하는 경우 효과적입니다.

- 2-pass 일 때 주의사항
    - 2-pass 는 단일 명령어가 아닌 2개의 명령어입니다.
    - 시간을 측정하는 time 이나 -benchmark 옵션을 한 번이 아닌 각 명령어마다 2개씩 적용시켜줘야 합니다.
    - 또한 passlog 옵션을 같게 지정하거나 지정하지 않은 경우 default passlog 파일이 생성됩니다.
        - 만일 2개 이상의 ffmpeg 명령어를 실행하는 경우, passlog 파일이 중복되어 하나의 파일을 2개의 프로세스가 접근하기 때문에 교착상태에 빠지게 됩니다.

<br />

```java
time ffmpeg -y -i input.mp4 -vcodec libx264 -profile:v high -level:v 4.1 -b:v 1200k -maxrate 1500k -minrate 800k -bufsize 2400k -g 90 -pass 1 -passlogfile passlog/output_$idx -preset medium -an -f mp4 /dev/null && time ffmpeg -y -i input.mp4 -vcodec libx264 -profile:v high -level:v 4.1 -b:v 1200k -maxrate 1500k -minrate 800k -bufsize 2400k -vf scale=1920:820 -g 90 -pass 2 -passlogfile passlog/output_$idx -c:a aac -b:a 96k -preset medium history/output_$idx.mp4
```

<br />

- `vcodec libx264` : 비디오 코덱으로 h.264를 사용합니다.
- `-profile:v high -level:v 4.1` : 인코딩할 비디오의 프로파일과 레벨을 설정합니다.
- **`b:v 1200k`**: 비디오의 비트레이트를 1200kbps로 설정합니다.
- **`maxrate 1500k -minrate 800k -bufsize 2400k`**: 최대 비트레이트, 최소 비트레이트, 버퍼 사이즈를 설정합니다.
- `-g 90` : Keyframe Interval 을 3초로 설정
- **`pass 1`**: 첫 번째 패스를 실행합니다.
- **`an`**: 오디오를 제거합니다.
- **`f mp4`**: 출력 포맷을 mp4로 설정합니다.
- **`/dev/null`**: 첫 번째 패스에서는 출력을 무시하므로, /dev/null로 출력을 보냅니다.
- **`&&`**: 첫 번째 패스가 성공적으로 완료되면 두 번째 패스를 실행합니다.
- **`vf scale=1920:820`**: 출력할 비디오의 해상도를 1920 x 820 으로 설정합니다.
- **`c:a aac -b:a 96k`**: 오디오를 aac 코덱으로 인코딩하고, 비트레이트를 96kbps로 설정합니다.
- **`preset medium`**: 인코딩 속도와 압축률을 설정합니다.

### 성능 테스트를 위한 Shell Script

---

```java
#!/bin/bash

idx=$1
result_file="history/result/result-$idx.txt"

ffmpeg_cmds=(
	"time ffmpeg -y -i input.mp4 -vcodec libx264 -profile:v high -level:v 4.1 -b:v 1200k -maxrate 1500k -minrate 800k -bufsize 2400k -g 90 -pass 1 -passlogfile passlog/output_$idx -preset medium -an -f mp4 /dev/null && time ffmpeg -y -i input.mp4 -vcodec libx264 -profile:v high -level:v 4.1 -b:v 1200k -maxrate 1500k -minrate 800k -bufsize 2400k -vf scale=1920:820 -g 90 -pass 2 -passlogfile passlog/output_$idx -c:a aac -b:a 96k -preset medium history/output_$idx.mp4
"
)

for (( i=0; i<${#ffmpeg_cmds[@]}; i++ ))
do
    echo "FFmpeg command: ${ffmpeg_cmds[$i]}"
    echo "Encoding $((i+1))/${#ffmpeg_cmds[@]} start..."
    echo "--------------------------------------------------" >> "$result_file"
    echo "FFmpeg command: ${ffmpeg_cmds[$i]}" >> "$result_file"
    echo "--------------------------------------------------" >> "$result_file"
    echo "" >> "$result_file"
    eval "${ffmpeg_cmds[$i]}" 2>&1 | tee -a "$result_file"
    echo "" >> "$result_file"
    echo "Encoding $((i+1))/${#ffmpeg_cmds[@]} complete."
done
```