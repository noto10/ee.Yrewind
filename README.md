# ee.Yrewind

[ ` -url ` ](#-urlurl)
[ ` -start ` ](#-startyyyymmddhhmm--startyyyymmddhhmmss)
[ ` -duration ` ](#-durationminutes)
[ ` -resolution ` ](#-resolutionheightpixels)
[ ` -ffmpeg ` ](#-ffmpegcpathtoffmpeg)
[ ` -output ` ](#-outputcdir1dir2filenameextension)
[ ` -executeonexit ` ](#-executeonexitcpathtosomefileext)

Yrewind is a command line utility to save YouTube live stream in its original quality. The program has the following features:

* Delayed start recording
* Recording in real time
* Rewinding to the specified time point in the past
* Rewinding to the first available time point in the past
* Waiting for the scheduled live stream to start and then automatically recording from the first second
* Monitoring the specified channel for new live streams and then automatically recording from the first second

Yrewind also allows to set the required duration, resolution and format. For a list of changes in new version, see the [changelog](https://github.com/rytsikau/ee.Yrewind/blob/main/CHANGELOG.md). Short tutorial video [here](https://www.youtube.com/watch?v=7HIIKBUPH5U).

### [>> download version 21.051](https://github.com/rytsikau/ee.Yrewind/releases/download/20210520/ee.yrewind_21.051.zip)

<br>

## Screenshot

<img src='https://github.com/rytsikau/ee.yrewind/raw/main/screenshot.png'>

<br>

## Quick start

1. Unpack the downloaded archive
2. Open *run.bat* in a text editor and paste the URLs of required streams instead of existing samples
3. Save *run.bat* and run it
* Short tutorial video [here](https://www.youtube.com/watch?v=7HIIKBUPH5U)

<br>

## Usage

The only required command line argument is the `-url`:

##### [**` -url=[url] `**](#)

With this command, the program records a livestream in real time for 1 hour at 1080p resolution, or at a lower if 1080p is not available. URL can be specified in various formats:
>     yrewind -url='youtube.com/watch?v=9Auq9mYxFEE'
>     yrewind -url=https://www.youtu.be/9Auq9mYxFEE
>     yrewind -url=9Auq9mYxFEE
>     (etc.)

You can also specify a channel URL (along with parameter `-start=wait`) to automatically record if livestream URL and start time are unknown. Please note that when specifying a channel URL, active livestreams on the channel are ignored, the program will wait for a new one to start. The channel URL can also be specified in various formats:
>     yrewind -url='https://www.youtube.com/c/SkyNews' -start=wait
>     yrewind -url=www.youtube.com/user/SkyNews/ -start=wait
>     yrewind -url='youtube.com/channel/UCoMdktPbSTixAyNGwb-UYkQ' -start=wait
>     yrewind -url=UCoMdktPbSTixAyNGwb-UYkQ -start=wait

<br>

To rewind the livestream or delay the start of recording, use the `-start` parameter. It has several spellings:

##### [**` -start=[YYYYMMDD:hhmm], -start=[YYYYMMDD:hhmmss] `**](#)
##### [**` -start=[Y:hhmm], -start=[T:hhmm] `**](#)
##### [**` -start=-[minutes], -start=+[minutes] `**](#)
##### [**` -start=beginning, -start=wait `**](#)

The parameter specifies the point in time from which to save the stream. If this parameter is missing, the program records a livestream in real time, starting from the moment the program starts.

To download the time interval from 7:10AM to 8:10AM on July 15, 2020:
>     yrewind -url='https://www.youtube.com/watch?v=9Auq9mYxFEE' -start=20200715:0710

To download the time interval from yesterday 10:15PM to 11:15PM:
>     yrewind -url='https://www.youtube.com/watch?v=9Auq9mYxFEE' -start=Y:2215

To download the time interval from today 02:00AM to 03:00AM:
>     yrewind -url='https://www.youtube.com/watch?v=9Auq9mYxFEE' -start=T:0200

To download the time interval from 3 hours ago to 2 hours ago:
>     yrewind -url='https://www.youtube.com/watch?v=9Auq9mYxFEE' -start=-180

To record 1 hour of livestream with a 2 hour delay:
>     yrewind -url='https://www.youtube.com/watch?v=9Auq9mYxFEE' -start=+120

To download the time interval from the first currently available moment:
>     yrewind -url='https://www.youtube.com/watch?v=9Auq9mYxFEE' -start=beginning

To record the scheduled livestream from the first second when it starts:
>     yrewind -url='https://www.youtube.com/watch?v=9Auq9mYxFEE' -start=wait

<br>

The program also has several other parameters:

##### [**` -duration=[minutes] `**](#)

Specifies the required duration in minutes. The minimum value is 1, the maximum is limited to 300 (if you need to make a longer recording, of course, you can repeat your command line several times in the batch file). If the parameter is missing, the program uses the default 60. The example below saves 15 minutes of the livestream:
>     yrewind -url='https://www.youtube.com/watch?v=9Auq9mYxFEE' -duration=15

<br>

##### [**` -resolution=[heightPixels] `**](#)

Specifies the required resolution in pixels (height). If this parameter is missing, the program uses the default 1080. If the requested resolution is not available, the program uses the nearest lower. In the examples below, the livestream will be saved at 480p:
>     yrewind -url='https://www.youtube.com/watch?v=9Auq9mYxFEE' -resolution=500
>     yrewind -url='https://www.youtube.com/watch?v=9Auq9mYxFEE' -resolution=480

<br>

##### [**` -ffmpeg='c:\path\to\ffmpeg\' `**](#)

Specifies the path to FFmpeg library. If this parameter is missing, Yrewind uses FFmpeg located in its folder.
>     yrewind -url='https://www.youtube.com/watch?v=9Auq9mYxFEE' -ffmpeg='d:\FFmpeg\'

<br>

##### [**` -output='c:\dir1\dir2\filename.extension' `**](#)

Specifies custom folder, filename and extension (media container format) for the saved livestream. If this parameter is missing, the program uses the default values:
* `[batch file folder]\saved_streams\` - for folder
* `[id]_[date]-[time]_[duration]_[resolution]` - for filename
* `.mp4` - for extension

The `-output` parameter can be specified partially, then the missing parts are replaced with the default values. In this case, the part of the string to the right of the last slash is interpreted as a filename and/or extension. If a string does not contain slashes, it's fully interpreted as a filename and/or extension:
* `c:\dir1\dir2\` - custom folder, default filename, default extension
* `dir1\filename` - custom subfolder, custom filename, default extension
* `dir1\.extension` - custom subfolder, default filename, custom extension
* `filename` - default folder, custom filename, default extension
* `.extension` - default folder, default filename, custom extension

Folder and filename supports renaming masks: `*id*`, `*start*`, `*start[customDateTime]*` (recognizes letters yyyyMMddHHmmss), `*duration*`, `*resolution*`, `*channel_id*`, `*author*` and `*title*`.

The extension defines the format of the media container in which the livestream will be saved. Formats description:
* `.avi`, `.mp4` - use AVC and MP4a data (if AVC is unavailable, use VP9)
* `.asf`, `.mkv`, `.wmv` - use VP9 and MP4a data (if VP9 is unavailable, use AVC)
* `.3gp`, ` .flv`, ` .mov`, `.ts` - use AVC and MP4a data (does not support 1080+ resolutions - saves at 1080p even if requested higher resolution is available)
* `.aac`, `.m4a`, `.wma` - use MP4a data (saves audio only)

The example below saves the livestream as *\saved_streams\9Auq9mYxFEE_20210123-185830_060m_1080p.ts*:
>     yrewind -url='https://www.youtube.com/watch?v=9Auq9mYxFEE' -output=.ts

The next example saves the livestream as *c:\downloads\Sky News\2021-01-12_12-34.mp4*:
>     yrewind -url='https://www.youtube.com/watch?v=9Auq9mYxFEE' -output='c:\downloads\*author*\*start[yyyy-MM-dd_HH-mm]*'

<br>

##### [**` -executeonexit='c:\path\to\some\file.ext' `**](#)

Specifies the command to run after Yrewind exits. If it's an executable file, you can also specify the arguments it supports (don't forget the quotes). The non-executable file will be launched by the associated program. The parameter supports two rename masks - `*output*`, which contains the full path of the saved video, and `*getnext*`, which contains the command to start Yrewind again to get the next interval of stream. In the first example below, the saved video will be opened by the associated program, in the second - using the specified program:
>     yrewind -url='https://www.youtube.com/watch?v=9Auq9mYxFEE' -executeonexit=*output*
>     yrewind -url='https://www.youtube.com/watch?v=9Auq9mYxFEE' -executeonexit=''c:\Program Files\VLC mediaplayer\vlc.exe' *output*'

<br>

**More examples:**

To save 15 minutes of the stream from yesterday 10:45AM to 11:00AM, at 1080p:
>     yrewind -url='https://www.youtube.com/watch?v=9Auq9mYxFEE' -start=Y:1045 -duration=15

To save 1 hour of the stream from 04:55AM to 05:55AM on May 5, 2020, at 720p, to specified folder:
>     yrewind -url=9Auq9mYxFEE -start=20200505:0455 -resolution=720 -output='d:\Saved_streams\'

To save 90 minutes of the stream, starting from half an hour ago, at the highest available resolution:
>     yrewind -u=9Auq9mYxFEE -s=-30 -d=90 -r=9999

<br>

## Notes

* All arguments and keywords can be replaced with single-character aliases: `-url` with `-u`, `-start` with `-s`, `beginning` with `b`, etc. This does not apply to rename masks
* If you are using proxy, VPN or special firewall settings, keep in mind that not only Yrewind should have appropriate access, but also FFmpeg
* Loss of packets on the streamer side causes the estimated time to shift. The offset is usually seconds, but if its internet connection is unstable and/or the stream has been running for a long time, it can be minutes or even hours. For example, if the stream was interrupted for a total of 1 hour, then 24-hour frames will be downloaded as 23-hour. Thus, start point time accuracy can only be guaranteed for the current moment. The farther the livestream is rewound, the greater the probability of an error. Also, if there are interruptions in the livestream at the specified time interval (this often happens at the beginning of the stream), the duration of the saved file will be shorter by the total duration of those interruptions; a warning for this incompleted file will be displayed
* Occasionally, the message `unable to verify the saved file is correct` appears. The reasons may be as follows: if the duration of the saved file cannot be verified (there is a possibility that the file is damaged); if the duration of the saved file does not match the requested one (also in this case, the output file name contains the word *INCOMPLETE*); if the starting point of the requested time interval cannot be accurately determined (rare case due to server side error)
* The base point of reference for the `-start` parameter is the local time on the computer when the program was started (displayed on the first line)
* To determine the earliest available point, try download the knowingly unavailable time interval (for example, `-start=20000101:0000`). After a while, the program will show a warning indicating the earliest available point at the moment
* To prevent output file corruption due to network errors or software crashes, use FLV or TS media container (`-output=.flv`, `-output=.ts`). Another way to reduce the chance of error is to download the livestream as multiple files with a relatively short duration (for example, default 60 minutes)
* When using the `-executeonexit=*getnext*` command inside a batch file, keep in mind that this file is first executed to the end, and only then the `*getnext*` command is executed. Also use rename masks `*start*` and `*start[customDateTime]*` to avoid duplicate names of saved stream parts (or just use default auto-naming)

<br>

## Terms of use

* This software provides access to information on the Internet that is publicly available and does not require authorization or authentication
* This software is free for non-commercial use and is provided 'as is' without warranty of any kind, either express or implied
* The author will not be liable for data loss, damages or any other kind of loss while using or misusing this software
* The author will not be liable for the misuse of content obtained using this software, including copyrighted, age-restricted, or any other protected content

<br>

## Developer info

* C#
* .NET Framework 4.5
* Visual Studio Community 2019

<br>

## Requirements

* FFmpeg static build (included in the archive)
* Windows 7 and on / Windows Server 2008 and on

<br>

## Tested configuration

* FFmpeg 4.3 x86 (by Zeranoe)
* Windows 10 Pro x32 version 1909
* Windows 10 Pro x64 version 1909

<br>

## Tags

download dvr live livestream record rewind save stream youtube

<br>

## About

Save YouTube live stream: rewind to the beginning, record in real time, wait for a new one, etc.

---
[[program page]](https://rytsikau.github.io/ee.Yrewind) [[start page]](https://rytsikau.github.io) [[author e-mail]](mailto:y.rytsikau@gmail.com)
