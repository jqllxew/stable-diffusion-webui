# Stable Diffusion web UI
#### 原项目地址[在这里](https://github.com/AUTOMATIC1111/stable-diffusion-webui)

### 2023.2.14
- 解决以--nowebui参数启动时没有执行nsfw脚本([差分](https://github.com/jqllxew/stable-diffusion-webui/commit/a4c75b143485eba23f46e8ef4f640e974517c301#diff-e093b909cce8fa9f0d7a77571607f8a1f9733c18d6e68ca2d03699c962df6fb5))
- 配合略微[魔改的nsfw扩展](https://github.com/jqllxew/stable-diffusion-webui-nsfw-censor)
  实现api调用t2i与i2i时只返回nsfw_res参数用于判断而不会黑图([差分](https://github.com/jqllxew/stable-diffusion-webui/commit/5309641965b7e41dc0dfe57587b265567a649aeb))
- 解决重现历史参数Error([差分](https://github.com/jqllxew/stable-diffusion-webui/commit/36d34026f47b75d9ad42119da23c326ea2384f29))
### 2023.1.30
- 新增汉化Chinese-English.json
- 新增requirements_my.txt 方便直接安装
- 修改gradio==3.16.2 为 3.15.0

之所以创建这个仓库是做一个版本收录以及解决原项目目前存在的问题(应该不算魔改吧)，
如果你对自己安装不感兴趣也可以去下载别人打包好的一键端。

## Python 
3.10.7 

## CUDA
[下载安装](https://developer.nvidia.com/cuda-downloads)

## Pytorch
需要注意的是，安装的CUDA需要对应Pytorch版本，具体请参考 [Pytorch START LOCALLY](https://pytorch.org/get-started/locally/)
可以看到目前最高支持cuda11.7版本
```bash
pip install torch torchvision --extra-index-url https://download.pytorch.org/whl/cu117 --proxy=xx
```

## 其他依赖
```bash
pip install -r requirements_my.txt --proxy=xx
```
推荐用requirements_my.txt，因为原本的requirements.txt可能会在下载basicsr包卡住，
这是因为basicsr还需要安装其他的包，还有原依赖中的gradio==3.16.2可能在请求响应时导致以下异常
```
ERROR:    Exception in ASGI application
Traceback (most recent call last):
  File "C:\stable-diffusion-webui\venv\lib\site-packages\fastapi\encoders.py", line 152, in jsonable_encoder
    data = dict(obj)
TypeError: 'coroutine' object is not iterable
```
具体[issues看这里](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/6966) \
目前解决方法是换成gradio==3.15.0

## 外部依赖与模型、插件
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
里面包含了要放至项目根目录的repositories、models、extensions 3个文件夹

models 中的 Stable-diffusion目录下的扩散模型可以按需下载

## xformers

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
build develop 需要C++环境，安装Visual Studio Installer安装C++桌面开发 \
之后配置环境变量，例如：
```
%xx%VC\Tools\MSVC\xx.xx.xx\bin\Hostx64\x86
```
确保cl.exe命令能正常调用

构建完毕之后加入--xformers启动参数启动
```bash
python webui.py --xformers
```
若不再出现 No module 'xformers'. Proceeding without it. 就代表安装成功了