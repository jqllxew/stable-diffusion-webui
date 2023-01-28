# Stable Diffusion web UI
#### 原项目地址[在这里](https://github.com/AUTOMATIC1111/stable-diffusion-webui)

最后更新时间2023.1.28 这里代码基本没有任何改动，
我只是新增了汉化Chinese-English.json与requirements_my.txt还有模型与插件， 
之所以创建这个仓库是做一个版本收录以及原项目目前存在的问题，
如果你对自己安装不感兴趣，你可以去下载别人打包好的一键端。

### Python 
3.10.7 

### CUDA
不需要多说，[CUDA](https://developer.nvidia.com/cuda-downloads) 是前提条件

### Pytorch
需要注意的是，安装的CUDA需要对应Pytorch版本，具体请参考 [Pytorch START LOCALLY](https://pytorch.org/get-started/locally/)
可以看到目前最高支持cuda11.7版本
```bash
pip install torch torchvision --extra-index-url https://download.pytorch.org/whl/cu117 --proxy=xx
```

### 其他依赖
推荐用requirements_my.txt，因为原本的requirements.txt会在下载basicsr包卡住，
这是因为basicsr还需要安装其他的包， \
还有原依赖中的gradio==3.16.2可能在请求响应时导致以下异常
```
ERROR:    Exception in ASGI application
Traceback (most recent call last):
  File "C:\stable-diffusion-webui\venv\lib\site-packages\fastapi\encoders.py", line 152, in jsonable_encoder
    data = dict(obj)
TypeError: 'coroutine' object is not iterable
```
具体[issues看这里](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/6966) \
目前解决方法是换成gradio==3.15.0

### 外部依赖与模型、插件
对应根目录repositories与models、extensions文件夹 \
按照modules/paths.py 代码中看来
```
possible_sd_paths = [os.path.join(script_path, 'repositories/stable-diffusion-stability-ai'), '.', os.path.dirname(script_path)]
...
path_dirs = [
    (sd_path, 'ldm', 'Stable Diffusion', []),
    (os.path.join(sd_path, '../taming-transformers'), 'taming', 'Taming Transformers', []),
    (os.path.join(sd_path, '../CodeFormer'), 'inference_codeformer.py', 'CodeFormer', []),
    (os.path.join(sd_path, '../BLIP'), 'models/blip.py', 'BLIP', []),
    (os.path.join(sd_path, '../k-diffusion'), 'k_diffusion/sampling.py', 'k_diffusion', ["atstart"]),
]
```
下面这些是需要的
- [stable-diffusion-stability-ai](https://github.com/Stability-AI/stablediffusion)
- [taming-transformers](https://github.com/CompVis/taming-transformers)
- [CodeFormer](https://github.com/sczhou/CodeFormer)
- [BLIP](https://github.com/salesforce/BLIP)
- [k-diffusion](https://github.com/crowsonkb/k-diffusion)

你也可以直接从[我这下载](https://pan.baidu.com/s/1MCXTdSLCUcCBHQm2kq5nPw?pwd=zona) 

里面包含了models所有模型文件，Stable-diffusion目录下的扩散模型按需下载

### 关于 xformers

可以让生成图片速度加快一定单位速度(it/s) \
并且节省内存，意味着可以提高GPU生成图片的分辨率上限

linux安装可以直接参考 [xformers](https://github.com/facebookresearch/xformers) \
conda install xformers -c xformers/label/dev
就完事了

windows需要使用源码安装
```bash
git clone https://github.com/facebookresearch/xformers.git
cd xformers
git submodule update --init --recursive # 更新子模块
python setup.py build develop # 构建开发包
```
build develop 是需要C++环境的，需要安装Visual Studio Installer安装C++桌面开发 \
之后配置环境变量，例如：
```
%xx%VC\Tools\MSVC\xx.xx.xx\bin\Hostx64\x86
```
确保cl.exe命令能正常调用
