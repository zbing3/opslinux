---
title: python实现控制Mplayer
tags:
  - mplayer
  - python
date: 2014-03-26 16:39:11
---

今天有这么一个需求，让用python来控制MPlayer，实现一个简单的web，功能有：暂停，播放，快进，快退，显示播放list，进度，显示当前播放文件名等等，本着不重复造轮子的理念，在Google一搜，发现了[mplayer.py](https://code.google.com/p/python-mplayer/)这个神器，来马上试用一下。


## Mplayer安装

我都用的是Mac系统（怎么这么不低调呢，这毛病什么时候能改），安装了MplayerX一到命令行输入：mplayer命令发现没命令，我就慌了，职业病的拿起brew进行安装：

`brew install mplayer`

没想到还真提示有包，安装ing。

当然，Ubuntu也不能少：

`apt-get install mplayer`

应该不会发生什么太大问题


## mplayer.py

### 安装

`pip install mplayer.py`

还是非常的方便

### 测试

进入python命令行，我们来测试：


```
(env)ce@mac:~> python
Python 2.7.5 (default, Aug 25 2013, 00:04:04)
[GCC 4.2.1 Compatible Apple LLVM 5.0 (clang-500.0.68)] on darwin
Type "help", "copyright", "credits" or "license" for more information.

>>>
>>> from mplayer import Player, CmdPrefix
```
根据文档，先引入需要的函数

```
>>> Player.cmd_prefix = CmdPrefix.PAUSING_KEEP
```
设置默认前缀为所有player的实例


```
>>> player = Player()
```

建立实例


```
>>> player.loadfile('/path/to/file.mkv')
```

引入文件，引入后文件就被打开了

这时候我们可以dir player来看看他都有什么方法让我们使用

```
>>> dir(player)
['__class__', '__del__', '__delattr__', '__dict__', '__doc__', '__format__', '__getattribute__', '__hash__', '__init__', '__module__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', '_args', '_base_args', '_gen_method_func', '_gen_propdoc', '_generate_methods', '_generate_properties', '_proc', '_process_args', '_propget', '_propset', '_run_command', '_stderr', '_stdout', 'af_add', 'af_clr', 'af_cmdline', 'af_del', 'af_switch', 'alt_src_step', 'angle', 'args', 'aspect', 'ass_use_margins', 'audio_bitrate', 'audio_codec', 'audio_delay', 'audio_format', 'balance', 'border', 'brightness', 'capturing', 'change_rectangle', 'channels', 'chapter', 'chapters', 'cmd_prefix', 'contrast', 'deinterlace', 'demuxer', 'dvdnav', 'edl_loadfile', 'edl_mark', 'exec_path', 'exit', 'file_filter', 'filename', 'forced_subs_only', 'fps', 'frame_drop', 'frame_step', 'framedropping', 'fullscreen', 'gamma', 'gui', 'height', 'help', 'hide', 'hue', 'introspect', 'is_alive', 'key_down_event', 'length', 'loadfile', 'loadlist', 'loop', 'menu', 'metadata', 'mute', 'ontop', 'osd', 'osd_show_progression', 'osd_show_property_text', 'osd_show_text', 'osdlevel', 'overlay_add', 'overlay_remove', 'panscan', 'path', 'pause', 'paused', 'percent_pos', 'pt_step', 'pt_up_step', 'quit', 'rootwin', 'run', 'samplerate', 'saturation', 'screenshot', 'seek', 'seek_chapter', 'set_menu', 'set_mouse_pos', 'spawn', 'speed', 'speed_incr', 'speed_mult', 'speed_set', 'stderr', 'stdout', 'stop', 'stream_end', 'stream_length', 'stream_pos', 'stream_start', 'stream_time_pos', 'sub', 'sub_alignment', 'sub_delay', 'sub_demux', 'sub_file', 'sub_forced_only', 'sub_load', 'sub_log', 'sub_pos', 'sub_remove', 'sub_scale', 'sub_select', 'sub_source', 'sub_step', 'sub_visibility', 'sub_vob', 'switch_angle', 'switch_audio', 'switch_program', 'switch_ratio', 'switch_title', 'switch_video', 'switch_vsync', 'teletext_add_dec', 'teletext_format', 'teletext_go_link', 'teletext_half_page', 'teletext_mode', 'teletext_page', 'teletext_subpage', 'time_pos', 'titles', 'tv_brightness', 'tv_contrast', 'tv_hue', 'tv_last_channel', 'tv_saturation', 'tv_set_brightness', 'tv_set_channel', 'tv_set_contrast', 'tv_set_freq', 'tv_set_hue', 'tv_set_norm', 'tv_set_saturation', 'tv_start_scan', 'tv_step_chanlist', 'tv_step_channel', 'tv_step_freq', 'tv_step_norm', 'use_master', 'version', 'video_bitrate', 'video_codec', 'video_format', 'vo_border', 'vo_fullscreen', 'vo_ontop', 'vo_rootwin', 'vobsub_lang', 'volume', 'vsync', 'width']
```

very well！

看来我们的需求是可以搞定了！


### 需要的方法

下面罗列下需要使用的方法：

player.pause()   #暂停

player.filename  #显示文件名

player.time_pos += 5  #快进5s

player.time_pos -= 5  #快退5s

player.stream_length #查看视频长度

player.stream_pos   #查看视频现在的位置， 根据上面可以做出进度条

player.volume  #显示音量

player.volume(+30.0)  #升高音量

player.volume(-30.0)  #降低音量

player.quit()  #关闭视频

好ok，有了以上功能大家是不是就已经可以控制Mplayer拉？现在就只差web界面了。


