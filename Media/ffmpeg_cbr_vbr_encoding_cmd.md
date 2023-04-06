```java
ffmpeg -h encoder=hevc_nvenc
```

**3. TR 요청 사항 to Media Platform 팀**

- 목적: H.265 Codec에 대한 CBR/1 Pass VBR/ 2 Pass VBR간 화질 비교 및 최종 서비스 Profile 확정
- TR 요청 사항

### h.265 1 Pass CBR

---

i. h.265 1 Pass CBR

Codec: nvenc h.265

Profile Level: Main @L4.1

Video Encoding Bitrate: 1.2 Mbps

Resolution: 1920 x 820

Audio(Codec/Bitrate): AAC/ 96kbps CBR

```java
time ffmpeg -y -i 1_iT4ZH2uO.mp4 -tag:v hvc1 -c:v hevc_nvenc -profile:v main -level 4.1 -b:v 1200k -maxrate:v 1200k -minrate:v 1200k -bufsize:v 2400k -vf scale=1920:820 -rc cbr_hq -preset hq -c:a aac -b:a 96k history/1_iT4ZH2uO_h265_nvenc_1pass_cbr_1200bps.mp4
```

- y : 덮어쓰기를 허용하는 옵션입니다.
- i 1_iT4ZH2uO.mp4 : 입력 파일 이름입니다.
- tag:v hvc1 : 비디오 스트림의 태그를 H.265/HEVC 코덱으로 설정합니다.
- c:v hevc_nvenc : 비디오 코덱을 NVIDIA NVENC 기반의 H.265/HEVC 코덱으로 설정합니다.
- profile:v main : 비디오 코덱 프로파일을 main으로 설정합니다.
- level 4.1 : 비디오 코덱 레벨을 4.1로 설정합니다.
- b:v 1200k : 비트레이트를 1200 kbps로 설정합니다.
- maxrate:v 1200k : 최대 비트레이트를 1200 kbps로 설정합니다.
- minrate:v 1200k : 최소 비트레이트를 1200 kbps로 설정합니다.
- bufsize:v 2400k : 비디오 버퍼 사이즈를 2400 kbps로 설정합니다.
- vf scale=1920:820 : 비디오 프레임 크기를 1920x820으로 조정합니다.
- rc cbr_hq : 비디오 인코딩 방식을 CBR (Constant Bitrate)로 설정하고, HQ (High Quality) 프리셋을 적용합니다.
- preset hq : 인코딩 프리셋을 HQ (High Quality)로 설정합니다.
- c:a aac : 오디오 코덱을 AAC로 설정합니다.
- b:a 96k : 오디오 비트레이트를 96 kbps로 설정합니다.


### h.265 1 Pass VBR

---

ii. h.265 1 Pass VBR

Codec: x265

Profile Level: Main @L4.1

Video Encoding Bitrate: 1.2 Mbps(Max: 1.5Mbps)

Resolution: 1920 x 820

Audio(Codec/Bitrate): AAC/ 96kbps CBR

```java
time ffmpeg -y -i 1_iT4ZH2uO.mp4 -c:v libx265 -tag:v hvc1 -b:v 1200k -x265-params vbv-maxrate=1500:vbv-bufsize=2400:profile=main:level=4.1 -preset medium -vf scale=1920:820 -c:a aac -b:a 96k history/1_iT4ZH2uO.mp4_h.265_1pass_vbr_1200k.mp4
```

- y : 덮어쓰기를 허용하는 옵션입니다.
- i 1_iT4ZH2uO.mp4 : 입력 파일 이름입니다.
- c:v libx265 : 비디오 코덱을 H.265/HEVC 코덱으로 설정합니다.
- tag:v hvc1 : 비디오 스트림의 태그를 H.265/HEVC 코덱으로 설정합니다.
- b:v 1200k : 비트레이트를 1200 kbps로 설정합니다.
- x265-params vbv-maxrate=1500:vbv-bufsize=2400:profile=main:level=4.1 : x265 코덱의 파라미터를 설정합니다. 최대 비트레이트를 1500 kbps, 버퍼 사이즈를 2400 kbps, 프로파일을 main, 레벨을 4.1로 설정합니다.
- preset medium : 인코딩 프리셋을 medium으로 설정합니다.
- vf scale=1920:820 : 비디오 프레임 크기를 1920x820으로 조정합니다.
- c:a aac : 오디오 코덱을 AAC로 설정합니다.
- b:a 96k : 오디오 비트레이트를 96 kbps로 설정합니다.

### h.265 1 Pass VBR

---

iii. h.265 1 Pass VBR

Codec: nvenc h.265

Profile Level: Main @L4.1

Video Encoding Bitrate: 1.2 Mbps(Max: 1.5Mbps)

Resolution: 1920 x 820

Audio(Codec/Bitrate): AAC/ 96kbps CBR

```java
ffmpeg -y -i 1_iT4ZH2uO.mp4 -tag:v hvc1 -c:v hevc_nvenc -profile:v main -level 4.1 -b:v 1200k -maxrate:v 1500k -minrate:v 1200k -bufsize:v 2400k -vf scale=1920:820 -rc vbr_hq -preset hq -c:a aac -b:a 96k history/1_iT4ZH2uO_h265_nvenc_1pass_vbr_1200bps.mp4
```

- y: 덮어쓰기를 허용하는 옵션
- i 1_iT4ZH2uO.mp4: 입력 파일의 이름
- tag:v hvc1: HEVC 비디오 스트림의 태그를 HVC1으로 설정합니다.
- c:v hevc_nvenc: NVIDIA NVENC 가속을 사용하여 HEVC 비디오 코덱을 사용합니다.
- profile:v main: HEVC 프로파일을 Main으로 설정합니다.
- level 4.1: HEVC 레벨을 4.1로 설정합니다.
- b:v 1200k: 비트레이트를 1200 kbps로 설정합니다.
- maxrate:v 1500k: 비디오 최대 비트레이트를 1500 kbps로 설정합니다.
- minrate:v 1200k: 비디오 최소 비트레이트를 1200 kbps로 설정합니다.
- bufsize:v 2400k: 버퍼 크기를 2400 kbps로 설정합니다.
- vf scale=1920:820: 비디오 프레임 크기를 1920x820으로 조정합니다.
- rc vbr_hq: HEVC 비트레이트 제어 방식을 가변 비트레이트 VBR로 설정하며, 품질을 우선으로 하는 고화질(rc_hq) 모드를 사용합니다.
- preset hq: NVIDIA NVENC가속 하드웨어 인코딩 프리셋을 high quality로 설정합니다.
- c:a aac: 오디오 코덱을 AAC로 설정합니다.
- b:a 96k: 오디오 비트레이트를 96 kbps로 설정합니다.

### 진행 Shell Script

---

- libx265 ffmpeg encoding shell script
- 위치 : dv02sg08 (182.252.140.170)
    - /home/kollus/jungin-kim/encoding-test7

```
#!/bin/bash

result_file="result.txt"

ffmpeg_cmds=(
"time ffmpeg -y -i 1_iT4ZH2uO.mp4 -c:v libx265 -tag:v hvc1 -b:v 1200k -x265-params vbv-maxrate=1500:vbv-bufsize=2400:profile=main:level=4.1 -preset medium -vf scale=1920:820 -c:a aac -b:a 96k history/1_iT4ZH2uO_h.265_1pass_vbr_1200k.mp4"
"time ffmpeg -y -i 2_RNlQH2uO.mp4 -c:v libx265 -tag:v hvc1 -b:v 1200k -x265-params vbv-maxrate=1500:vbv-bufsize=2400:profile=main:level=4.1 -preset medium -vf scale=1920:820 -c:a aac -b:a 96k history/2_RNlQH2uO_h.265_1pass_vbr_1200k.mp4"
"time ffmpeg -y -i 3_EaK2H2v3.mp4 -c:v libx265 -tag:v hvc1 -b:v 1200k -x265-params vbv-maxrate=1500:vbv-bufsize=2400:profile=main:level=4.1 -preset medium -vf scale=1920:820 -c:a aac -b:a 96k history/3_EaK2H2v3_h.265_1pass_vbr_1200k.mp4"
"time ffmpeg -y -i 4_AqIQH2v0.mp4 -c:v libx265 -tag:v hvc1 -b:v 1200k -x265-params vbv-maxrate=1500:vbv-bufsize=2400:profile=main:level=4.1 -preset medium -vf scale=1920:820 -c:a aac -b:a 96k history/4_AqIQH2v0_h.265_1pass_vbr_1200k.mp4"
"time ffmpeg -y -i 5_yZrlH2vn.mp4 -c:v libx265 -tag:v hvc1 -b:v 1200k -x265-params vbv-maxrate=1500:vbv-bufsize=2400:profile=main:level=4.1 -preset medium -vf scale=1920:820 -c:a aac -b:a 96k history/5_yZrlH2vn_h.265_1pass_vbr_1200k.mp4"
"time ffmpeg -y -i 6_7HJYH2tQ.mp4 -c:v libx265 -tag:v hvc1 -b:v 1200k -x265-params vbv-maxrate=1500:vbv-bufsize=2400:profile=main:level=4.1 -preset medium -vf scale=1920:820 -c:a aac -b:a 96k history/6_7HJYH2tQ_h.265_1pass_vbr_1200k.mp4"
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

- nvenc ffmpeg encoding shell script
- 위치 : stage-tr01 (182.252.181.39)
    - /home/kollus/jungin-kim/encoding-test7

```
#!/bin/bash

result_file="result.txt"

ffmpeg_cmds=(
"time ffmpeg -y -i 1_iT4ZH2uO.mp4 -tag:v hvc1 -c:v hevc_nvenc -profile:v main -level 4.1 -b:v 1200k -maxrate:v 1200k -minrate:v 1200k -bufsize:v 2400k -vf scale=1920:820 -rc cbr_hq -preset hq -c:a aac -b:a 96k history/1_iT4ZH2uO_h265_nvenc_1pass_cbr_1200bps.mp4"
"time ffmpeg -y -i 2_RNlQH2uO.mp4 -tag:v hvc1 -c:v hevc_nvenc -profile:v main -level 4.1 -b:v 1200k -maxrate:v 1200k -minrate:v 1200k -bufsize:v 2400k -vf scale=1920:820 -rc cbr_hq -preset hq -c:a aac -b:a 96k history/2_RNlQH2uO_h265_nvenc_1pass_cbr_1200bps.mp4"
"time ffmpeg -y -i 3_EaK2H2v3.mp4 -tag:v hvc1 -c:v hevc_nvenc -profile:v main -level 4.1 -b:v 1200k -maxrate:v 1200k -minrate:v 1200k -bufsize:v 2400k -vf scale=1920:820 -rc cbr_hq -preset hq -c:a aac -b:a 96k history/3_EaK2H2v3_h265_nvenc_1pass_cbr_1200bps.mp4"
"time ffmpeg -y -i 4_AqIQH2v0.mp4 -tag:v hvc1 -c:v hevc_nvenc -profile:v main -level 4.1 -b:v 1200k -maxrate:v 1200k -minrate:v 1200k -bufsize:v 2400k -vf scale=1920:820 -rc cbr_hq -preset hq -c:a aac -b:a 96k history/4_AqIQH2v0_h265_nvenc_1pass_cbr_1200bps.mp4"
"time ffmpeg -y -i 5_yZrlH2vn.mp4 -tag:v hvc1 -c:v hevc_nvenc -profile:v main -level 4.1 -b:v 1200k -maxrate:v 1200k -minrate:v 1200k -bufsize:v 2400k -vf scale=1920:820 -rc cbr_hq -preset hq -c:a aac -b:a 96k history/5_yZrlH2vn_h265_nvenc_1pass_cbr_1200bps.mp4"
"time ffmpeg -y -i 6_7HJYH2tQ.mp4 -tag:v hvc1 -c:v hevc_nvenc -profile:v main -level 4.1 -b:v 1200k -maxrate:v 1200k -minrate:v 1200k -bufsize:v 2400k -vf scale=1920:820 -rc cbr_hq -preset hq -c:a aac -b:a 96k history/6_7HJYH2tQ_h265_nvenc_1pass_cbr_1200bps.mp4"
"time ffmpeg -y -i 1_iT4ZH2uO.mp4 -tag:v hvc1 -c:v hevc_nvenc -profile:v main -level 4.1 -b:v 1200k -maxrate:v 1500k -minrate:v 1200k -bufsize:v 2400k -vf scale=1920:820 -rc vbr_hq -preset hq -c:a aac -b:a 96k history/1_iT4ZH2uO_h265_nvenc_1pass_vbr_1200bps.mp4"
"time ffmpeg -y -i 2_RNlQH2uO.mp4 -tag:v hvc1 -c:v hevc_nvenc -profile:v main -level 4.1 -b:v 1200k -maxrate:v 1500k -minrate:v 1200k -bufsize:v 2400k -vf scale=1920:820 -rc vbr_hq -preset hq -c:a aac -b:a 96k history/2_RNlQH2uO_h265_nvenc_1pass_vbr_1200bps.mp4"
"time ffmpeg -y -i 3_EaK2H2v3.mp4 -tag:v hvc1 -c:v hevc_nvenc -profile:v main -level 4.1 -b:v 1200k -maxrate:v 1500k -minrate:v 1200k -bufsize:v 2400k -vf scale=1920:820 -rc vbr_hq -preset hq -c:a aac -b:a 96k history/3_EaK2H2v3_h265_nvenc_1pass_vbr_1200bps.mp4"
"time ffmpeg -y -i 4_AqIQH2v0.mp4 -tag:v hvc1 -c:v hevc_nvenc -profile:v main -level 4.1 -b:v 1200k -maxrate:v 1500k -minrate:v 1200k -bufsize:v 2400k -vf scale=1920:820 -rc vbr_hq -preset hq -c:a aac -b:a 96k history/4_AqIQH2v0_h265_nvenc_1pass_vbr_1200bps.mp4"
"time ffmpeg -y -i 5_yZrlH2vn.mp4 -tag:v hvc1 -c:v hevc_nvenc -profile:v main -level 4.1 -b:v 1200k -maxrate:v 1500k -minrate:v 1200k -bufsize:v 2400k -vf scale=1920:820 -rc vbr_hq -preset hq -c:a aac -b:a 96k history/5_yZrlH2vn_h265_nvenc_1pass_vbr_1200bps.mp4"
"time ffmpeg -y -i 6_7HJYH2tQ.mp4 -tag:v hvc1 -c:v hevc_nvenc -profile:v main -level 4.1 -b:v 1200k -maxrate:v 1500k -minrate:v 1200k -bufsize:v 2400k -vf scale=1920:820 -rc vbr_hq -preset hq -c:a aac -b:a 96k history/6_7HJYH2tQ_h265_nvenc_1pass_vbr_1200bps.mp4"
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



