---
title: huggingface 下载模型和数据集
date: 2023-11-29 12-43-47
---


### step1: 安装 huggingface_hub 和 hf-transfer

```bash
pip install -U huggingface_hub
pip install -U hf-transfer
```
hf-transfer 及后续环境配置是可选的。


如果需要清华镜像源：
```bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple -U huggingface_hub
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple -U hf-transfer
```

### step2: 设置环境变量

Linux
```bash
export HF_ENDPOINT=https://hf-mirror.com 
export HF_HUB_ENABLE_HF_TRANSFER=1
```

Windows Powershell
```bash
$env:HF_ENDPOINT = "https://hf-mirror.com"
$env:HF_HUB_ENABLE_HF_TRANSFER = 1
```

python
```python
import os
os.environ['HF_ENDPOINT'] = 'https://hf-mirror.com'
```

### step3: 下载

```bash
huggingface-cli download --resume-download bigscience/bloom-560m --local-dir bloom-560m  --local-dir-use-symlinks False
```

如果需要 huggingface token 需要加上 `--token hf_xxxx`
