---
layout: default
---
# Codec Mappings

A `Codec Mapping` is a set of attributes to identify, name, and contextualize the format and characteristics of encoded data that can be contained within Matroska Clusters.

Each TrackEntry used within Matroska MUST reference a defined `Codec Mapping` using the `Codec ID` to identify and describe the format of the encoded data in its associated Clusters. This `Codec ID` is a unique registered identifier that represents the encoding stored within the Track. Certain encodings MAY also require some form of codec initialisation in order to provide its decoder with context and technical metadata.

The intention behind this list is not to list all existing audio and video codecs, but rather to list those codecs that are currently supported in Matroska and therefore need a well defined `Codec ID` so that all developers supporting Matroska will use the same `Codec ID`. If you feel we missed support for a very important codec, please tell us on our development mailing list (cellar at ietf.org).

## Defining Matroska Codec Support

Support for a codec is defined in Matroska with the following values.

### Codec ID

Each codec supported for storage in Matroska MUST have a unique `Codec ID`. Each `Codec ID` MUST be prefixed with the string from the following table according to the associated type of the codec. All characters of a `Codec ID Prefix` MUST be capital letters (A-Z) except for the last character of a `Codec ID Prefix` which MUST be an underscore ("_").

Codec Type | Codec ID Prefix
-----------|----------------
Video      | "V_"
Audio      | "A_"
Subtitle   | "S_"
Button     | "B_"

Each `Codec ID` MUST include a `Major Codec ID` immediately following the `Codec ID Prefix`. A `Major Codec ID` MAY be followed by an OPTIONAL `Codec ID Suffix` to communicate a refinement of the `Major Codec ID`. If a `Codec ID Suffix` is used, then the `Codec ID` MUST include a forward slash ("/") as a separator between the `Major Codec ID` and the `Codec ID Suffix`. The `Major Codec ID` MUST be composed of only capital letters (A-Z) and numbers (0-9). The `Codec ID Suffix` MUST be composed of only capital letters (A-Z), numbers (0-9), underscore ("_"), and forward slash ("/").

The following table provides examples of valid `Codec IDs` and their components:

Codec ID Prefix | Major Codec ID | Separator | Codec ID Suffix | Codec ID
----------------|:---------------|:----------|:----------------|:--------
A_              | AAC            | /         | MPEG2/LC/SBR    | A_AAC/MPEG2/LC/SBR
V_              | V_MPEG4        | /         | ISO/ASP         | V_MPEG4/ISO/ASP
V_              | MPEG1          |           |                 | V_MPEG1

### Codec Name

Each encoding supported for storage in Matroska MUST have a Codec Name. The Codec Name provides a readable label for the encoding.

### Description

An optional description for the encoding. This value is only intended for human consumption.

### Initialisation

Each encoding supported for storage in Matroska MUST have a defined Initialisation. The Initialisation MUST describe the storage of data necessary to initialize the decoder, which MUST be stored within the `CodecPrivate Element`. When the Initialisation is updated within a track then that updated Initialisation data MUST be written into the `CodecState Element` of the first `Cluster` to require it. If the encoding does not require any form of Initialisation then `none` MUST be used to define the Initialisation and the `CodecPrivate Element` SHOULD NOT be written and MUST be ignored. Data that is defined Initialisation to be stored in the `CodecPrivate Element` is known as `Private Data`.

### Citation

Documentation of the associated normative and informative references for the codec is RECOMMENDED.

### Deprecation Date

A timestamp, expressed in [@!RFC3339] that notes when support for the `Codec Mapping` within Matroska was deprecated. If a `Codec Mapping` is defined with a `Deprecation Date`, then it is RECOMMENDED that Matroska writers SHOULD NOT use the `Codec Mapping` after the `Deprecation Date`.

### Superseded By

A `Codec Mapping` MAY only be defined with a `Superseded By` value, if it has an expressed `Deprecation Date`. If used, the `Superseded By` value MUST store the `Codec ID` of another `Codec Mapping` that has superseded the `Codec Mapping`.

## Video Codec Mappings

### V_MS/VFW/FOURCC

Codec ID: `V_MS/VFW/FOURCC`

Codec Name: Microsoft (TM) Video Codec Manager (VCM)

Description: The private data contains the VCM structure BITMAPINFOHEADER including the extra private bytes, as [defined by Microsoft](https://msdn.microsoft.com/en-us/library/windows/desktop/dd318229(v=vs.85).aspx). The data are stored in little endian format (like on IA32 machines). Where is the Huffman table stored in HuffYUV, not AVISTREAMINFO ??? And the FourCC, not in AVISTREAMINFO.fccHandler ???

Initialisation: `Private Data` contains the VCM structure BITMAPINFOHEADER including the extra private bytes, as defined by Microsoft in https://msdn.microsoft.com/en-us/library/windows/desktop/dd183376(v=vs.85).aspx.

Citation: https://msdn.microsoft.com/en-us/library/windows/desktop/dd183376(v=vs.85).aspx

### V_UNCOMPRESSED

Codec ID: V_UNCOMPRESSED

Codec Name: Video, raw uncompressed video frames

Description: All details about the used colour specs and bit depth are to be put/read from the KaxCodecColourSpace elements.

Initialisation: none

### V_MPEG4/ISO/SP

Codec ID: V_MPEG4/ISO/SP

Codec Name: MPEG4 ISO simple profile (DivX4)

Description: Stream was created via improved codec API (UCI) or even transmuxed from AVI (no b-frames in Simple Profile), frame order is coding order.

Initialisation: none

### V_MPEG4/ISO/ASP

Codec ID: V_MPEG4/ISO/ASP

Codec Name: MPEG4 ISO advanced simple profile (DivX5, XviD, FFMPEG)

Description: Stream was created via improved codec API (UCI) or transmuxed from MP4, not simply transmuxed from AVI. Note there are differences how b-frames are handled in these native streams, when being compared to a VfW created stream, as here there are `no` dummy frames inserted, the frame order is exactly the same as the coding order, same as in MP4 streams.

Initialisation: none

### V_MPEG4/ISO/AP

Codec ID: V_MPEG4/ISO/AP

Codec Name: MPEG4 ISO advanced profile

Description: Stream was created via improved codec API (UCI) or transmuxed from MP4, not simply transmuxed from AVI. Note there are differences how b-frames are handled in these native streams, when being compared to a VfW created stream, as here there are `no` dummy frames inserted, the frame order is exactly the same as the coding order, same as in MP4 streams.

Initialisation: none

### V_MPEG4/MS/V3

Codec ID: V_MPEG4/MS/V3

Codec Name: Microsoft (TM) MPEG4 V3

Description: Microsoft (TM) MPEG4 V3 and derivates, means DivX3, Angelpotion, SMR, etc.; stream was created using VfW codec or transmuxed from AVI; note that V1/V2 are covered in VfW compatibility mode.

Initialisation: none

### V_MPEG1

Codec ID: V_MPEG1

Codec Name: MPEG 1

Description: The Matroska video stream will contain a demuxed Elementary Stream (ES), where block boundaries are still to be defined. Its RECOMMENDED to use MPEG2MKV.exe for creating those files, and to compare the results with selfmade implementations

Initialisation: none

### V_MPEG2

Codec ID: V_MPEG2

Codec Name: MPEG 2

Description: The Matroska video stream will contain a demuxed Elementary Stream (ES), where block boundaries are still to be defined. Its RECOMMENDED to use MPEG2MKV.exe for creating those files, and to compare the results with selfmade implementations

Initialisation: none

### V_REAL/RV10

Codec ID: V_REAL/RV10

Codec Name: RealVideo 1.0 aka RealVideo 5

Description: Individual slices from the Real container are combined into a single frame.

Initialisation: The `Private Data` contains a `real_video_props_t` structure in Big Endian byte order as found in [librmff](https://github.com/mbunkus/mkvtoolnix/blob/master/lib/librmff/librmff.h).

### V_REAL/RV20

Codec ID: V_REAL/RV20

Codec Name: RealVideo G2 and RealVideo G2+SVT

Description: Individual slices from the Real container are combined into a single frame.

Initialisation: The `Private Data` contains a `real_video_props_t` structure in Big Endian byte order as found in [librmff](https://github.com/mbunkus/mkvtoolnix/blob/master/lib/librmff/librmff.h).

### V_REAL/RV30

Codec ID: V_REAL/RV30

Codec Name: RealVideo 8

Description: Individual slices from the Real container are combined into a single frame.

Initialisation: The `Private Data` contains a `real_video_props_t` structure in Big Endian byte order as found in [librmff](https://github.com/mbunkus/mkvtoolnix/blob/master/lib/librmff/librmff.h).

### V_REAL/RV40

Codec ID: V_REAL/RV40

Codec Name: rv40 : RealVideo 9

Description: Individual slices from the Real container are combined into a single frame.

Initialisation: The `Private Data` contains a `real_video_props_t` structure in Big Endian byte order as found in [librmff](https://github.com/mbunkus/mkvtoolnix/blob/master/lib/librmff/librmff.h).

### V_QUICKTIME

Codec ID: V_QUICKTIME

Codec Name: Video taken from QuickTime(TM) files

Description: Several codecs as stored in QuickTime, e.g. Sorenson or Cinepak. 

Initialisation: The `Private Data` contains all additional data that is stored in the 'stsd' (sample description) atom in the QuickTime file **after** the mandatory video descriptor structure (starting with the size and FourCC fields). For an explanation of the QuickTime file format read [QuickTime File Format Specification](https://developer.apple.com/library/mac/documentation/QuickTime/QTFF/QTFFPreface/qtffPreface.html).

### V_THEORA

Codec ID: V_THEORA

Codec Name: Theora

Initialisation: The `Private Data` contains the first three Theora packets in order. The lengths of the packets precedes them. The actual layout is:

* Byte 1: number of distinct packets '`#p`' minus one inside the CodecPrivate block. This MUST be '2' for current (as of 2016-07-08) Theora headers.
* Bytes 2..n: lengths of the first '`#p`' packets, coded in [Xiph-style lacing]({{site.baseurl}}/index.html#lacing). The length of the last packet is the length of the CodecPrivate block minus the lengths coded in these bytes minus one.
* Bytes n+1..: The Theora identification header, followed by the commend header followed by the codec setup header. Those are described in the [Theora specs](http://www.theora.org/doc/Theora.pdf).

### V_PRORES

Codec ID: V_PRORES

Codec Name: Apple ProRes

Initialisation: The `Private Data` contains the fourcc as found in MP4 movies:

*   apch: ProRes 422 High Quality
*   apcn: ProRes 422 Standard Definition
*   apcs: ProRes 422 LT
*   apco: ProRes 422 Proxy
*   ap4h: ProRes 4444

[this page for more technical details on ProRes](http://wiki.multimedia.cx/index.php?title=Apple_ProRes#Frame_layout)

### V_VP8

Codec ID: V_VP8

Codec Name: VP8 Codec format

Description: VP8 is an open and royalty free video compression format developed by Google and created by On2 Technologies as a successor to VP7. [@!RFC6386]

Initialisation: none

### V_VP9

Codec ID: V_VP9

Codec Name: VP9 Codec format

Description: VP9 is an open and royalty free video compression format developed by Google as a successor to VP8. [Draft VP9 Bitstream and Decoding Process Specification](https://www.webmproject.org/vp9/)

Initialisation: none

### V_FFV1

Codec ID: V_FFV1

Codec Name: FF Video Codec 1

Description: FFV1 is a lossless intra-frame video encoding format designed to efficiently compress video data in a variety of pixel formats. Compared to uncompressed video, FFV1 offers storage compression, frame fixity, and self-description, which makes FFV1 useful as a preservation or intermediate video format. [Draft FFV1 Specification](https://datatracker.ietf.org/doc/draft-niedermayer-cellar-ffv1/)

Initialisation: For FFV1 versions 2 or less, `Private Data` SHOULD NOT be written. For FFV1 version 3 and greater, the `Private Data` MUST contain the FFV1 Configuration Record structure, as defined in https://tools.ietf.org/html/draft-niedermayer-cellar-ffv1-01#section-4.1, and no other data.

## Audio Codec Mappings

### A_MPEG/L3

Codec ID: A_MPEG/L3

Codec Name: MPEG Audio 1, 2, 2.5 Layer III

Description: The data contain everything needed for playback in the MPEG Audio header of each frame. Corresponding ACM wFormatTag : 0x0055

Initialisation: none

### A_MPEG/L2

Codec ID: A_MPEG/L2

Codec Name: MPEG Audio 1, 2 Layer II

Description: The data contain everything needed for playback in the MPEG Audio header of each frame. Corresponding ACM wFormatTag : 0x0050

Initialisation: none

### A_MPEG/L1

Codec ID: A_MPEG/L1

Codec Name: MPEG Audio 1, 2 Layer I

Description: The data contain everything needed for playback in the MPEG Audio header of each frame. Corresponding ACM wFormatTag : 0x0050

Initialisation: none

### A_PCM/INT/BIG

Codec ID: A_PCM/INT/BIG

Codec Name: PCM Integer Big Endian

Description: The bitdepth has to be read and set from KaxAudioBitDepth element. Corresponding ACM wFormatTag : ???

Initialisation: none

### A_PCM/INT/LIT

Codec ID: A_PCM/INT/LIT

Codec Name: PCM Integer Little Endian

Description: The bitdepth has to be read and set from KaxAudioBitDepth element. Corresponding ACM wFormatTag : 0x0001

Initialisation: none

### A_PCM/FLOAT/IEEE

Codec ID: A_PCM/FLOAT/IEEE

Codec Name: Floating Point, IEEE compatible

Description: The bitdepth has to be read and set from KaxAudioBitDepth element (32 bit in most cases). The float are stored in little endian order (most common float format). Corresponding ACM wFormatTag : 0x0003

Initialisation: none

### A_MPC

Codec ID: A_MPC

Codec Name: MPC (musepack) SV8

Description: The main developer for musepack has requested that we wait until the SV8 framing has been fully defined for musepack before defining how to store it in Matroska.

### A_AC3

Codec ID: A_AC3

Codec Name: (Dolby™) AC3

Description: BSID <= 8 !! The private data is void ??? Corresponding ACM wFormatTag : 0x2000 ; channel number have to be read from the corresponding audio element

### A_AC3/BSID9

Codec ID: A_AC3/BSID9

Codec Name: (Dolby™) AC3

Description: The ac3 frame header has, similar to the mpeg-audio header a version field. Normal ac3 is defiened as bitstream id 8 (5 Bits, numbers are 0-15). Everything below 8 is still compatible with all decoders that handle 8 correctly. Everything higher are additions that break decoder compatibility.
For the samplerates 24kHz (00); 22,05kHz (01) and 16kHz (10) the BSID is 9
For the samplerates 12kHz (00); 11,025kHz (01) and 8kHz (10) the BSID is 10

Initialisation: none

### A_AC3/BSID10

Codec ID: A_AC3/BSID10

Codec Name: (Dolby™) AC3

Description: The ac3 frame header has, similar to the mpeg-audio header a version field. Normal ac3 is defiened as bitstream id 8 (5 Bits, numbers are 0-15). Everything below 8 is still compatible with all decoders that handle 8 correctly. Everything higher are additions that break decoder compatibility.
For the samplerates 24kHz (00); 22,05kHz (01) and 16kHz (10) the BSID is 9
For the samplerates 12kHz (00); 11,025kHz (01) and 8kHz (10) the BSID is 10

Initialisation: none

### A_ALAC

Codec ID: A_ALAC

Codec Name: ALAC (Apple Lossless Audio Codec)

Initialisation: The `Private Data` contains ALAC's magic cookie (both the codec specific configuration as well as the optional channel layout information). Its format is described in [ALAC's official source code](http://alac.macosforge.org/trac/browser/trunk/ALACMagicCookieDescription.txt).

### A_DTS

Codec ID: A_DTS

Codec Name: Digital Theatre System

Description: Supports DTS, DTS-ES, DTS-96/26, DTS-HD High Resolution Audio and DTS-HD Master Audio. The private data is void. Corresponding ACM wFormatTag : 0x2001

Initialisation: none

### A_DTS/EXPRESS

Codec ID: A_DTS/EXPRESS

Codec Name: Digital Theatre System Express

Description: DTS Express (a.k.a. LBR) audio streams. The private data is void. Corresponding ACM wFormatTag : 0x2001

Initialisation: none

### A_DTS/LOSSLESS

Codec ID: A_DTS/LOSSLESS

Codec Name: Digital Theatre System Lossless

Description: DTS Lossless audio that does not have a core substream. The private data is void. Corresponding ACM wFormatTag : 0x2001

Initialisation: none

### A_VORBIS

Codec ID: A_VORBIS

Codec Name: Vorbis

Initialisation: The `Private Data` contains the first three Vorbis packet in order. The lengths of the packets precedes them. The actual layout is:
- Byte 1: number of distinct packets '`#p`' minus one inside the CodecPrivate block. This MUST be '2' for current (as of 2016-07-08) Vorbis headers.
- Bytes 2..n: lengths of the first '`#p`' packets, coded in [Xiph-style lacing]({{site.baseurl}}/index.html#lacing). The length of the last packet is the length of the CodecPrivate block minus the lengths coded in these bytes minus one.
- Bytes n+1..: The [Vorbis identification header](https://xiph.org/vorbis/doc/Vorbis_I_spec.html), followed by the [Vorbis comment header](https://xiph.org/vorbis/doc/v-comment.html) followed by the [codec setup header](https://xiph.org/vorbis/doc/Vorbis_I_spec.html).

### A_FLAC

Codec ID: A_FLAC

Codec Name: [FLAC (Free Lossless Audio Codec)](http://flac.sourceforge.net/)

Initialisation: The `Private Data` contains all the header/metadata packets before the first data packet. These include the first header packet containing only the word `fLaC` as well as all metadata packets.

### A_REAL/14_4

Codec ID: A_REAL/14_4

Codec Name: Real Audio 1

Initialisation: The `Private Data` contains either the "real_audio_v4_props_t" or the "real_audio_v5_props_t" structure (differentiated by their "version" field; Big Endian byte order) as found in [librmff](https://github.com/mbunkus/mkvtoolnix/blob/master/lib/librmff/librmff.h).

### A_REAL/28_8

Codec ID: A_REAL/28_8

Codec Name: Real Audio 2

Initialisation: The `Private Data` contains either the "real_audio_v4_props_t" or the "real_audio_v5_props_t" structure (differentiated by their "version" field; Big Endian byte order) as found in [librmff](https://github.com/mbunkus/mkvtoolnix/blob/master/lib/librmff/librmff.h).

### A_REAL/COOK

Codec ID: A_REAL/COOK

Codec Name: Real Audio Cook Codec (codename: Gecko)

Initialisation: The `Private Data` contains either the "real_audio_v4_props_t" or the "real_audio_v5_props_t" structure (differentiated by their "version" field; Big Endian byte order) as found in [librmff](https://github.com/mbunkus/mkvtoolnix/blob/master/lib/librmff/librmff.h).

### A_REAL/SIPR

Codec ID: A_REAL/SIPR

Codec Name: Sipro Voice Codec

Initialisation: The `Private Data` contains either the "real_audio_v4_props_t" or the "real_audio_v5_props_t" structure (differentiated by their "version" field; Big Endian byte order) as found in [librmff](https://github.com/mbunkus/mkvtoolnix/blob/master/lib/librmff/librmff.h).

### A_REAL/RALF

Codec ID: A_REAL/RALF

Codec Name: Real Audio Lossless Format

Initialisation: The `Private Data` contains either the "real_audio_v4_props_t" or the "real_audio_v5_props_t" structure (differentiated by their "version" field; Big Endian byte order) as found in [librmff](https://github.com/mbunkus/mkvtoolnix/blob/master/lib/librmff/librmff.h).

### A_REAL/ATRC

Codec ID: A_REAL/ATRC

Codec Name: Sony Atrac3 Codec

Initialisation: The `Private Data` contains either the "real_audio_v4_props_t" or the "real_audio_v5_props_t" structure (differentiated by their "version" field; Big Endian byte order) as found in [librmff](https://github.com/mbunkus/mkvtoolnix/blob/master/lib/librmff/librmff.h).

### A_MS/ACM

Codec ID: A_MS/ACM

Codec Name: Microsoft(TM) Audio Codec Manager (ACM)

Description: The data are stored in little endian format (like on IA32 machines).

Initialisation: The `Private Data` contains the ACM structure WAVEFORMATEX including the extra private bytes, as [defined by Microsoft](http://msdn.microsoft.com/library/default.asp?url=/library/en-us/multimed/mmstr_625u.asp).

### A_AAC/MPEG2/MAIN

Codec ID: A_AAC/MPEG2/MAIN

Codec Name: MPEG2 Main Profile

Description: Channel number and sample rate have to be read from the corresponding audio element. Audio stream is stripped from ADTS headers and normal Matroska frame based muxing scheme is applied. AAC audio always uses wFormatTag 0xFF.

Initialisation: none

### A_AAC/MPEG2/LC

Codec ID: A_AAC/MPEG2/LC

Codec Name: Low Complexity

Description: Channel number and sample rate have to be read from the corresponding audio element. Audio stream is stripped from ADTS headers and normal Matroska frame based muxing scheme is applied. AAC audio always uses wFormatTag 0xFF.

Initialisation: none

### A_AAC/MPEG2/LC/SBR

Codec ID: A_AAC/MPEG2/LC/SBR

Codec Name: Low Complexity with Spectral Band Replication

Description: Channel number and sample rate have to be read from the corresponding audio element. Audio stream is stripped from ADTS headers and normal Matroska frame based muxing scheme is applied. AAC audio always uses wFormatTag 0xFF.

Initialisation: none

### A_AAC/MPEG2/SSR

Codec ID: A_AAC/MPEG2/SSR

Codec Name: Scalable Sampling Rate

Description: Channel number and sample rate have to be read from the corresponding audio element. Audio stream is stripped from ADTS headers and normal Matroska frame based muxing scheme is applied. AAC audio always uses wFormatTag 0xFF.

Initialisation: none

### A_AAC/MPEG4/MAIN

Codec ID: A_AAC/MPEG4/MAIN

Codec Name: MPEG4 Main Profile

Description: Channel number and sample rate have to be read from the corresponding audio element. Audio stream is stripped from ADTS headers and normal Matroska frame based muxing scheme is applied. AAC audio always uses wFormatTag 0xFF.

Initialisation: none

### A_AAC/MPEG4/LC

Codec ID: A_AAC/MPEG4/LC

Codec Name: Low Complexity

Description: Channel number and sample rate have to be read from the corresponding audio element. Audio stream is stripped from ADTS headers and normal Matroska frame based muxing scheme is applied. AAC audio always uses wFormatTag 0xFF.

Initialisation: none

### A_AAC/MPEG4/LC/SBR

Codec ID: A_AAC/MPEG4/LC/SBR

Codec Name: Low Complexity with Spectral Band Replication

Description: Channel number and sample rate have to be read from the corresponding audio element. Audio stream is stripped from ADTS headers and normal Matroska frame based muxing scheme is applied. AAC audio always uses wFormatTag 0xFF.

Initialisation: none

### A_AAC/MPEG4/SSR

Codec ID: A_AAC/MPEG4/SSR

Codec Name: Scalable Sampling Rate

Description: Channel number and sample rate have to be read from the corresponding audio element. Audio stream is stripped from ADTS headers and normal Matroska frame based muxing scheme is applied. AAC audio always uses wFormatTag 0xFF.

Initialisation: none

### A_AAC/MPEG4/LTP

Codec ID: A_AAC/MPEG4/LTP

Codec Name: Long Term Prediction

Description: Channel number and sample rate have to be read from the corresponding audio element. Audio stream is stripped from ADTS headers and normal Matroska frame based muxing scheme is applied. AAC audio always uses wFormatTag 0xFF.

Initialisation: none

### A_QUICKTIME

Codec ID: A_QUICKTIME

Codec Name: Audio taken from QuickTime(TM) files

Description: Several codecs as stored in QuickTime, e.g. QDesign Music v1 or v2.

Initialisation: The `Private Data` contains all additional data that is stored in the 'stsd' (sample description) atom in the QuickTime file **after** the mandatory sound descriptor structure (starting with the size and FourCC fields). For an explanation of the QuickTime file format read [QuickTime File Format Specification](https://developer.apple.com/library/mac/documentation/QuickTime/QTFF/QTFFPreface/qtffPreface.html).

### A_QUICKTIME/QDMC

Codec ID: A_QUICKTIME/QDMC

Codec Name: QDesign Music

Description:

Initialisation: The `Private Data` contains all additional data that is stored in the 'stsd' (sample description) atom in the QuickTime file **after** the mandatory sound descriptor structure (starting with the size and FourCC fields). For an explanation of the QuickTime file format read [QuickTime File Format Specification](https://developer.apple.com/library/mac/documentation/QuickTime/QTFF/QTFFPreface/qtffPreface.html).

Superseded By: A_QUICKTIME

### A_QUICKTIME/QDM2

Codec ID: A_QUICKTIME/QDM2

Codec Name: QDesign Music v2

Description:

Initialisation: The `Private Data` contains all additional data that is stored in the 'stsd' (sample description) atom in the QuickTime file **after** the mandatory sound descriptor structure (starting with the size and FourCC fields). For an explanation of the QuickTime file format read [QuickTime File Format Specification](https://developer.apple.com/library/mac/documentation/QuickTime/QTFF/QTFFPreface/qtffPreface.html).

Superseded By: A_QUICKTIME

### A_TTA1

Codec ID: A_TTA1

Codec Name: [The True Audio](http://tausoft.org/) lossles audio compressor

Description: [TTA format description](http://tausoft.org/wiki/True_Audio_Codec_Format)
Each frame is kept intact, including the CRC32. The header and seektable are dropped. SamplingFrequency, Channels and BitDepth are used in the TrackEntry. wFormatTag = 0x77A1

Initialisation: none

### A_WAVPACK4

Codec ID: A_WAVPACK4

Codec Name: [WavPack](http://www.wavpack.com/) lossless audio compressor

Description: The Wavpack packets consist of a stripped header followed by the frame data. For multi-track (> 2 tracks) a frame consists of many packets. For hybrid files (lossy part + correction part), the correction part is stored in an additional block (level 1). For more details, check the [WavPack muxing description](wavpack.html).

Initialisation: none

## Subtitle Codec Mappings

### S_TEXT/UTF8

Codec ID: S_TEXT/UTF8

Codec Name: UTF-8 Plain Text

Description: Basic text subtitles. For more information, please look at the [Subtitle specifications]({{site.baseurl}}/subtitles.html).

### S_TEXT/SSA

Codec ID: S_TEXT/SSA

Codec Name: Subtitles Format

Description: The [Script Info] and [V4 Styles] sections are stored in the codecprivate. Each event is stored in its own Block. For more information, please read the [specs for SSA/ASS]({{site.baseurl}}/subtitles.html).

### S_TEXT/ASS

Codec ID: S_TEXT/ASS

Codec Name: Advanced Subtitles Format

Description: The [Script Info] and [V4 Styles] sections are stored in the codecprivate. Each event is stored in its own Block. For more information, please read the [specs for SSA/ASS]({{site.baseurl}}/subtitles.html).

### S_TEXT/USF

Codec ID: S_TEXT/USF

Codec Name: Universal Subtitle Format

Description: This is mostly defined, but not typed out yet. It will first be available on the [USF specs page]({{site.baseurl}}/subtitles.html).

### S_TEXT/WEBVTT

Codec ID: S_TEXT/WEBVTT

Codec Name: Web Video Text Tracks Format (WebVTT)

Description: Advanced text subtitles. For more information about the storage please look at the [WebVTT in Matroska specifications]({{site.baseurl}}/subtitles.html).

### S_IMAGE/BMP

Codec ID: S_IMAGE/BMP

Codec Name: Bitmap

Description: Basic image based subtitle format; The subtitles are stored as images, like in the DVD. The timestamp in the block header of Matroska indicates the start display time, the duration is set with the Duration element. The full data for the subtitle bitmap is stored in the Block's data section.

### S_DVBSUB

Codec ID: S_DVBSUB

Codec Name: Digital Video Broadcasting (DVB) subtitles

Description: This is the graphical subtitle format used in the Digital Video Broadcasting standard. For more information about the storage please look at the [Digital Video Broadcasting (DVB) subtitles in Matroska specifications]({{site.baseurl}}/subtitles.html).

### S_VOBSUB

Codec ID: S_VOBSUB

Codec Name: VobSub subtitles

Description: The same subtitle format used on DVDs. Supoprted is only format version 7 and newer. VobSubs consist of two files, the .idx containing information, and the .sub, containing the actual data. The .idx file is stripped of all empty lines, of all comments and of lines beginning with `alt:` or `langidx:`. The line beginning with `id:` SHOULD be transformed into the appropriate Matroska track language element and is discarded. All remaining lines but the ones containing timestamps and file positions are put into the `CodecPrivate` element.

For each line containing the timestamp and file position data is read from the appropriate position in the .sub file. This data consists of a MPEG program stream which in turn contains SPU packets. The MPEG program stream data is discarded, and each SPU packet is put into one Matroska frame.

### S_HDMV/PGS

Codec ID: S_HDMV/PGS

Codec Name: HDMV presentation graphics subtitles (PGS)

Description: This is the graphical subtitle format used on Blu-rays. For more information about the storage please look at the [HDMV presentation graphics subtitles in Matroska specifications]({{site.baseurl}}/subtitles.html).

### S_HDMV/TEXTST

Codec ID: S_HDMV/TEXTST

Codec Name: HDMV text subtitles

Description: This is the textual subtitle format used on Blu-rays. For more information about the storage please look at the [HDMV text subtitles in Matroska specifications]({{site.baseurl}}/subtitles.html).

### S_KATE

Codec ID: S_KATE

Codec Name: Karaoke And Text Encapsulation

Description: A subtitle format developped for ogg. The mapping for Matroska is described on the [Xiph wiki](http://wiki.xiph.org/index.php/OggKate#Matroska_mapping). As for Theora and Vorbis, Kate headers are stored in the private data as xiph-laced packets.

## Button Codec Mappings

### B_VOBBTN

Codec ID: B_VOBBTN

Codec Name: VobBtn Buttons

Description: Based on [MPEG/VOB PCI packets](http://dvd.sourceforge.net/dvdinfo/pci_pkt.html). The file contains a header consisting of the string "butonDVD" followed by the width and height in pixels (16 bits integer each) and 4 reserved bytes. The rest is full [PCI packets](http://dvd.sourceforge.net/dvdinfo/pci_pkt.html).
