<!-- Note! This content includes shared parts. Therefore, when you update this, you should beware of synchronization. -->

# Supported audio formats

If audio content is provided by the custom extension, it must be in an audio compression format supported by Clova.

<!-- Start of the shared content: SupportedAudioFormat -->

Audio compression formats supported by Clova are as follows:

| Audio compression format                     | License cost |
|----------------------------------|-----------|
| MPEG-1 or MPEG-2 Audio Layer III | Free       |

Audio container formats supported by Clova are as follows:

| Container Format   | MIME Type                      | Remark                           |
|-------------|-------------------------------|-------------------------------|
| mp3         | audio/mpeg                    | <!-- -->                      |
| m3u8        | application/vnd.apple.mpegurl | Uses HTTP Live Streaming.       |


<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>More audio compression formats may be additionally supported by Clova in the future.</p>
</div>

<!-- End of the shared content -->

<div class="warning">
  <p><strong>Warning!</strong></p>
  <p>If audio content is provided using an audio compression format not supported by Clova, the client may not be able to play it properly.</p>
</div>

It is recommended that you follow the sound attributes and loudness for each audio content type as shown in the table below.

| Audio content type        | Sampling frequency, bit depth, and channel | Loudness  | Remark                                     |
|-----------------------|-------------------------|--------------- |----------------------------------------|
| Music                   | 44100 Hz, 16-bit, stereo  | -10(±1) LUFS  | -17(±1) LUFS for beat box music. |
| Sound effect       | 44100 Hz, 16-bit, stereo  | -18(±1) LUFS  |                                         |
| Audio book               | 44100 Hz, 16-bit, stereo  | -12(±1) LUFS  |                                         |
| Music or sound in the ambient genre  | 44100 Hz, 16-bit, stereo  | -25(±1) LUFS  | Examples of this type of audio content are wave sounds or rain sounds. The loudness must be adjusted according to the characteristics of each content. |
