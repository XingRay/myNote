## 抽取视频数据

### 基本概念

1. StartCode
每一帧前面的特征码，区分帧与帧之间的间隔

2. SPS/PPS
解码的视频参数，比如宽高（分辨率）、帧率
一般多媒体文件有一个就可以
视频分辨率发生变化时需要更新SPS/PPS
直播流中可能会丢数据，在每个关键帧前面增加SPS/PPS,数据量很小，不会增加网络负担

3. codec->extradata 获取 SPS/PPS

### 步骤

1. log级别 输入参数判断
2. register_all
3. avformat_alloc_contetx
4. av_guess_format
5. avformat_new_stream
6. avio_open
7. fopen(outPath)
8. avformat_open_input
9. avformat_find_stream_info
10. av_dump_format
11. av_frame_alloc
12. av_init_packet
13. av_find_best_stream
14. avformat_write_header
15. av_read_frame
16. ** h264_mp4toannexb 设置startCode SPS PPS **
17. av_packet_unref 


h264_mp4toannexb
AvPacket* in

1. out = av_packet_alloc
2. 帧头4字节是大小，要取出，大端数据
3. 第一个字节后5为是ANL单元的类型
SPS =7
PPS = 8
关键帧 = 5
非关键帧=1

4. 获取SPS和PPS
5. 获取StartCode
6. fwrite/fflush

### startCode
00000001 SPS PPS 特征码 4字节
000001 非SPS PPS 特征码 3字节

### SPS PPS
扩展数据前4字节无用
后第一个字节低2位是SPS PPS长度数据所需的字节数

后一个字节的低5为是SPS 和PPS的个数，一般只有一个 很少有多个

循环读取SPS和PPS

