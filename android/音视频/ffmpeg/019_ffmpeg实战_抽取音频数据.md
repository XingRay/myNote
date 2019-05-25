## 实战：抽取音频数据

### 相关API
```cppp

// 初始化数据包结构体
av_init_packet()

// 在多媒体文件中找打最好的流
av_find_best_stream()

// 从流中获取数据包
// av_read_frame其实是读取包，历史遗留问题，名字没改
av_read_frame()

// 减少包数据的引用计数，ffmpeg会自动回收对象
av_packet_unref()

```



