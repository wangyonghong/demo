# hls encryption test

## Tencent Demo

[HLS 普通加密](https://cloud.tencent.com/document/product/266/9638)

```shell
# 播放
ffplay http://251000406.vod2.myqcloud.com/42f90d89vodtransgzp251000406/a633b37814508071098245980156/f0.f210.m3u8

# 下载
youtube-dl http://251000406.vod2.myqcloud.com/42f90d89vodtransgzp251000406/a633b37814508071098245980156/f0.f210.m3u8
```

## Encryption Test

### Encryption

```shell
openssl rand  16 > enc.key
touch enc.keyinfo
echo "https://demo.yonghong.me/hls-encryption/enc.key" > enc.keyinfo
echo "enc.key" >> enc.keyinfo
echo $(openssl rand -hex 16) >> enc.keyinfo
```

### play

```shell
# 使用URL获取enc.key
ffplay -allowed_extensions ALL hls/index.m3u8

# 本地播放
cp enc.key hls
sed 's/https:\/\/demo.yonghong.me\/hls-encryption\///' hls/index.m3u8 > hls/index-local.m3u8
ffplay -allowed_extensions ALL hls/index-local.m3u8
```
