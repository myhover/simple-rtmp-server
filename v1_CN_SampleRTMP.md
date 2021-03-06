# RTMP部署实例

RTMP部署的步骤。

<strong>假设服务器的IP是：192.168.1.170</strong>

<strong>第一步，获取SRS。</strong>详细参考[GIT获取代码](v1_CN_Git)

```bash
git clone https://github.com/ossrs/srs
cd srs/trunk
```

或者使用git更新已有代码：

```bash
git pull
```

<strong>第二步，编译SRS。</strong>详细参考[Build](v1_CN_Build)

```bash
./configure --disable-all --with-ssl && make
```

<strong>第三步，编写SRS配置文件。</strong>详细参考[RTMP分发](v1_CN_DeliveryRTMP)

将以下内容保存为文件，譬如`conf/rtmp.conf`，服务器启动时指定该配置文件(srs的conf文件夹有该文件)。

```bash
# conf/rtmp.conf
listen              1935;
max_connections     1000;
vhost __defaultVhost__ {
}
```

<strong>第四步，启动SRS。</strong>详细参考[RTMP分发](v1_CN_DeliveryRTMP)

```bash
./objs/srs -c conf/rtmp.conf
```

<strong>第五步，启动推流编码器。</strong>详细参考[RTMP分发](v1_CN_DeliveryRTMP)

使用FFMPEG命令推流：

```bash
    for((;;)); do \
        ./objs/ffmpeg/bin/ffmpeg -re -i ./doc/source.200kbps.768x320.flv \
        -vcodec copy -acodec copy \
        -f flv -y rtmp://192.168.1.170/live/livestream; \
        sleep 1; \
    done
```

或使用FMLE推流：

```bash
FMS URL: rtmp://192.168.1.170/live
Stream: livestream
```

<strong>第六步，观看RTMP流。</strong>详细参考[RTMP分发](v1_CN_DeliveryRTMP)

RTMP流地址为：`rtmp://192.168.1.170/live/livestream`

可以使用VLC观看。

或者使用在线SRS播放器播放：[srs-player][srs-player]

备注：请将所有实例的IP地址192.168.1.170都换成部署的服务器IP地址。

Winlin 2014.3

[nginx]: http://192.168.1.170:8080/nginx.html
[srs-player]: http://winlinvip.github.io/srs.release/trunk/research/players/srs_player.html?vhost=__defaultVhost__&autostart=true&server=192.168.1.170&app=live&stream=livestream&port=1935
[srs-player-19350]: http://winlinvip.github.io/srs.release/trunk/research/players/srs_player.html?vhost=__defaultVhost__&autostart=true&server=192.168.1.170&app=live&stream=livestream&port=19350
[srs-player-ff]: http://winlinvip.github.io/srs.release/trunk/research/players/srs_player.html?vhost=__defaultVhost__&autostart=true&server=192.168.1.170&app=live&stream=livestream_ff
[jwplayer]: http://winlinvip.github.io/srs.release/trunk/research/players/jwplayer6.html?vhost=__defaultVhost__&hls_autostart=true&server=192.168.1.170&app=live&stream=livestream&hls_port=8080
