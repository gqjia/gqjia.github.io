---
title: 【Voice Recognition】librosa.load() FileNotFoundError
date: 2019-05-21 17:49:17
---

当使用librosa读取音频文件时:
```Python
y, sr = librosa.load(os.path.join(path.DATA_PATH, wav))
```
甚至官方教程:
```Python
filename = librosa.util.example_audio_file()
y, sr = librosa.load(filename)
```
```txt
FileNotFoundError: [WinError 2] 系统找不到指定的文件。
```

---
在librosa-core-audio.py中load()函数

```Python
def load(path, sr=22050, mono=True, offset=0.0, duration=None,
         dtype=np.float32, res_type='kaiser_best'):
    y = []
    with audioread.audio_open(os.path.realpath(path)) as input_file:
        ...

```

当我们使用
```Python
audioread.audio_open(os.path.realpath(filename))
```

```Python
audioread.audio_open(os.path.realpath(os.path.join(path.DATA_PATH, wav)))
```
```txt
FileNotFoundError: [WinError 2] 系统找不到指定的文件。
```

---
os.path.realpath()
返回指定文件的标准路径，而非软链接所在的路径
Return the canonical path of the specified filename, eliminating any
symbolic links encountered in the path.

不同于os.path.abspath()
返回一个目录的绝对路径
Return an absolute path.


```Python
print(os.path.realpath(os.path.join(path.DATA_PATH, wav)))
```
```txt
D:\...\wav\63.wav
```

```Python
print(os.path.realpath(filename))
```
```txt
D:\...\Kevin_MacLeod_-_Vibe_Ace.ogg
```

---

在audioread-\__init__.py

```Python
def audio_open(path, backends=None):
    """Open an audio file using a library that is available on this
    system.

    The optional `backends` parameter can be a list of audio file
    classes to try opening the file with. If it is not provided,
    `audio_open` tries all available backends. If you call this function
    many times, you can avoid the cost of checking for available
    backends every time by calling `available_backends` once and passing
    the result to each `audio_open` call.

    If all backends fail to read the file, a NoBackendError exception is
    raised.
    """
    if backends is None:
        backends = available_backends()

    for BackendClass in backends:
        try:
            return BackendClass(path)
        except DecodeError:
            pass

    # All backends failed!
    raise NoBackendError()
```



最后解决方法：
以管理员身份运行



emmmm
