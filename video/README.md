# video

## Download

```shell
youtube-dl https://www.youtube.com/watch?v=2MsN8gpT6jY
```

## Convert

```shell
ffmpeg -i video.webm -c:v copy video.mp4

mkdir hls
ffmpeg -y -i video.mp4 -hls_time 10 -hls_playlist_type vod -hls_segment_filename "hls/%6d.ts" video.m3u8

# macOS
sed -i '' 's/\(^.*.ts$\)/hls\/\1/' video.m3u8
# Linux
sed -i 's/\(^.*.ts$\)/hls\/\1/' video.m3u8
```

## Encryption

```shell
openssl rand 16 > enc.key
touch enc.keyinfo
echo "https://demo.yonghong.me/video/enc.key" > enc.keyinfo
echo "enc.key" >> enc.keyinfo
echo $(openssl rand -hex 16) >> enc.keyinfo

ffmpeg -y -i video.mp4 -hls_time 10 -hls_key_info_file enc.keyinfo -hls_playlist_type vod -hls_segment_filename "hls-encrypted/%6d.ts" video-encrypted.m3u8

# macOS
sed -i '' 's/\(^.*.ts$\)/hls-encrypted\/\1/' video-encrypted.m3u8
# Linux
sed -i 's/\(^.*.ts$\)/hls-encrypted\/\1/' video-encrypted.m3u8
```

## Play

```shell
# 播放未加密的
ffplay video.m3u8

# 从URL获取enc.key
ffplay -allowed_extensions ALL video-encrypted.m3u8

# 本地播放
cp enc.key hls
sed 's/https:\/\/demo.yonghong.me\/video\///' video-encrypted.m3u8 > video-encrypted-local.m3u8
ffplay -allowed_extensions ALL video-encrypted-local.m3u8
```