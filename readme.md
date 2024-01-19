MPEG DASH stream for the Fundamental Topics in Multimedia Systems WS2023/24 course

The stream can be tested on various DASH  players (e.g. https://bitmovin.com/demos/stream-test or https://reference.dashif.org/dash.js/latest/samples/dash-if-reference-player/index.html) by using the the following link: https://wilhelmstu.github.io/stream.mpd 
The link currently works on Chrome, there are issues with it on Firefox/Edge likely due to the H.265 Codec used.

The .mpd was created and packaged using the BENTO4 tools (https://www.bento4.com/)
The video files were encoded with ffmpeg to the four given qualities and bitrates:
ffmpeg -f rawvideo -pix_fmt yuv420p -s:v 3840x2160 -r 120 -i ReadySteadyGo_3840x2160.yuv -c:v libx265 -threads 16 -preset slow -crf 18 -b:v 300k -s:v 640x360 -r:v 30 -bf 1 -g 120 640x360.mp4
ffmpeg -f rawvideo -pix_fmt yuv420p -s:v 3840x2160 -r 120 -i ReadySteadyGo_3840x2160.yuv -c:v libx265 -threads 16 -preset slow -crf 18 -b:v 900k -s:v 960x540 -r:v 60 -bf 1 -g 120 960x540.mp4
ffmpeg -f rawvideo -pix_fmt yuv420p -s:v 3840x2160 -r 120 -i ReadySteadyGo_3840x2160.yuv -c:v libx265 -threads 16 -preset slow -crf 18 -b:v 2400k -s:v 1280x720 -r:v 60 -bf 1 -g 120 1280x720.mp4
ffmpeg -f rawvideo -pix_fmt yuv420p -s:v 3840x2160 -r 120 -i ReadySteadyGo_3840x2160.yuv -c:v libx265 -threads 16 -preset slow -crf 18 -b:v 4500k -s:v 1920x1080 -r:v 60 -bf 1 -g 120 1920x1080.mp4

The original source video can be found at: https://ultravideo.fi/dataset.html

---
The second .mpd and test stream inside video/v1 does not work properly with online DASH players, but works perfectly on my local machine.
It was created with the following ffmpeg command:

ffmpeg -f rawvideo -pix_fmt yuv420p -s:v 3840x2160 -r 120 -i ReadySteadyGo_3840x2160.yuv -map 0 -map 0 -map 0 -map 0 ^
-c:v libx265 -threads 16 -preset slow -crf 18 ^
-b:v:0 300k -s:v:0 640x360 -r:v:0 30 ^
-b:v:1 900k -s:v:1 960x540 -r:v:1 60 ^
-b:v:2 2400k -s:v:2 1280x720 -r:v:2 60 ^
-b:v:3 4500k -s:v:3 1920x1080 -r:v:3 60 ^
-bf 1 -keyint_min 120 -g 120 ^
-use_timeline 0 -use_template 1 ^
-seg_duration 4 -adaptation_sets "id=0,streams=v" ^
-f dash out/stream.mpd