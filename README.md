# discordcrasharchive
y know those videos that reboots/freezes discord? ye those


## how it works

k so there are two versions (as i call it)

the first one simply freezes discord when you try to change the current channel

the second one is a variant of the first one that reboots discord

### how the first one works

In the middle of it, the pixel format of the video changes suddently, it was not intended so video player goes what

we can see this using `ffplay -loglevel verbose "file.mp4"`:

```
[ffplay_abuffer @ 000001d9696e7380] tb:1/48000 samplefmt:fltp samplerate:48000 chlayout:0x3
[ffplay_abuffersink @ 000001d9696e7940] auto-inserting filter 'auto_resampler_0' between the filter 'ffplay_abuffer' and the filter 'ffplay_abuffersink'
[auto_resampler_0 @ 000001d9696e7cc0] ch:2 chl:stereo fmt:fltp r:48000Hz -> ch:2 chl:stereo fmt:s16 r:48000Hz

[h264 @ 000001d9696edd80] Reinit context to 640x368, pix_fmt: yuv420p  <<<<<<<<<<<<<<<<<<<<<< note this 'yuv420', its the initial pixel format

[ffplay_abuffer @ 000001d96e20c380] tb:1/48000 samplefmt:fltp samplerate:48000 chlayout:0x3
[ffplay_abuffersink @ 000001d9696e7640] auto-inserting filter 'auto_resampler_0' between the filter 'ffplay_abuffer' and the filter 'ffplay_abuffersink'
[auto_resampler_0 @ 000001d96e21ccc0] ch:2 chl:stereo fmt:fltp r:48000Hz -> ch:2 chl:stereo fmt:s16 r:48000Hz
[ffplay_buffer @ 000001d967cd7080] w:640 h:360 pixfmt:yuv420p tb:1/30000 fr:30/1 sar:0/1
Created 640x360 texture with SDL_PIXELFORMAT_IYUV.

[h264 @ 000001d96972dd00] Reinit context to 320x240, pix_fmt: yuv444p  <<<<<<<<<<<<<<<<<<<<< now here is when it freezes, note that it changed to 'yuv444p'

[ffplay_buffer @ 000001d96eaaef00] w:320 h:240 pixfmt:yuv444p tb:1/30000 fr:30/1 sar:0/1
[auto_scaler_0 @ 000001d96eaaec00] w:iw h:ih flags:'bicubic' interl:0
[ffplay_buffersink @ 000001d96eaae400] auto-inserting filter 'auto_scaler_0' between the filter 'ffplay_buffer' and the filter 'ffplay_buffersink'
[auto_scaler_0 @ 000001d96eaaec00] w:320 h:240 fmt:yuv444p sar:0/1 -> w:320 h:240 fmt:yuv420p sar:0/1 flags:0x4
    Last message repeated 2 times
[auto_scaler_0 @ 000001d96eaaec00] w:320 h:240 fmt:yuv444p sar:1/1 -> w:320 h:240 fmt:yuv420p sar:1/1 flags:0x4
Created 320x240 texture with SDL_PIXELFORMAT_IYUV.sq=    0B f=0/0
```

it can be simply recreated by concatenating two videos using `ffmpeg`, the original one and the second one with yuv420p pixel format

### how the second one works

the second one uses the same method as the first, but now it also changes the video resolution to 15000x15000 while the video is playing, which is even worse

we can also see this using `ffplay -loglevel verbose "file2.mp4"`

```
[ffplay_abuffer @ 000001b381c2b080] tb:1/48000 samplefmt:fltp samplerate:48000 chlayout:0x3
[ffplay_abuffersink @ 000001b381c2b640] auto-inserting filter 'auto_resampler_0' between the filter 'ffplay_abuffer' and the filter 'ffplay_abuffersink'
[auto_resampler_0 @ 000001b381c2b980] ch:2 chl:stereo fmt:fltp r:48000Hz -> ch:2 chl:stereo fmt:s16 r:48000Hz

[h264 @ 000001b381c2cbc0] Reinit context to 640x368, pix_fmt: yuv444p >>>>>>>>>>>>>>>>> note that this is the initial resolution and pixel video format

[ffplay_abuffer @ 000001b381c2aa40] tb:1/48000 samplefmt:fltp samplerate:48000 chlayout:0x3
[ffplay_abuffersink @ 000001b381c2b480] auto-inserting filter 'auto_resampler_0' between the filter 'ffplay_abuffer' and the filter 'ffplay_abuffersink'
[auto_resampler_0 @ 000001b381c16a80] ch:2 chl:stereo fmt:fltp r:48000Hz -> ch:2 chl:stereo fmt:s16 r:48000Hz
[ffplay_buffer @ 000001b381c87080] w:640 h:360 pixfmt:yuv444p tb:1/15360 fr:30/1 sar:0/1
[auto_scaler_0 @ 000001b38874ed40] w:iw h:ih flags:'bicubic' interl:0
[ffplay_buffersink @ 000001b381bfd700] auto-inserting filter 'auto_scaler_0' between the filter 'ffplay_buffer' and the filter 'ffplay_buffersink'
[auto_scaler_0 @ 000001b38874ed40] w:640 h:360 fmt:yuv444p sar:0/1 -> w:640 h:360 fmt:yuv420p sar:0/1 flags:0x4
Created 640x360 texture with SDL_PIXELFORMAT_IYUV.

[h264 @ 000001b381c2cbc0] Reinit context to 15008x15008, pix_fmt: yuv420p  <<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
[ffplay_buffer @ 000001b388aec380] w:15000 h:15000 pixfmt:yuv420p tb:1/15360 fr:30/1 sar:0/1  <<<<<<<<<<<<<<<< now here is when it crashes, note how it goes from 
Created 15000x15000 texture with SDL_PIXELFORMAT_IYUV.   <<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<< 640x368 yuv444p to f*cking 15000x15000 yuv420p

```

it can be simply recreated by concatenating two videos using `ffmpeg`, the original one and the second one with 15000x15000 resolution and yuv420p pixel format

ppl also use [gfycat](https://gfycat.com/) to upload the videos and send it to others because it has autoplay so yeah be careful out there
