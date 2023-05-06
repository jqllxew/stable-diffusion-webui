# Stable Diffusion web UI
#### 原地址[在这里](https://github.com/AUTOMATIC1111/stable-diffusion-webui)

### 2023.2.14
- 解决以--nowebui参数启动时没有执行nsfw脚本([差分](https://github.com/jqllxew/stable-diffusion-webui/commit/a4c75b143485eba23f46e8ef4f640e974517c301#diff-e093b909cce8fa9f0d7a77571607f8a1f9733c18d6e68ca2d03699c962df6fb5))
- 配合略微[魔改的nsfw扩展](https://github.com/jqllxew/stable-diffusion-webui-nsfw-censor)
  实现api调用t2i与i2i时只返回nsfw_res参数用于判断而不会黑图([差分](https://github.com/jqllxew/stable-diffusion-webui/commit/5309641965b7e41dc0dfe57587b265567a649aeb))
- 解决重现历史参数Error([差分](https://github.com/jqllxew/stable-diffusion-webui/commit/36d34026f47b75d9ad42119da23c326ea2384f29))

## Python
3.8

- [stable-diffusion-stability-ai](https://github.com/Stability-AI/stablediffusion)
- [taming-transformers](https://github.com/CompVis/taming-transformers)
- [CodeFormer](https://github.com/sczhou/CodeFormer)
- [BLIP](https://github.com/salesforce/BLIP)
- [k-diffusion](https://github.com/crowsonkb/k-diffusion)

## install & run

```bash
git clone --branch colab https://github.com/jqllxew/stable-diffusion-webui.git sd_colab && cd sd_colab
mkdir repositories && cd repositories 
wget https://huggingface.co/TheLastBen/dependencies/resolve/main/sd_rep.tar.zst
tar --zstd -xf sd_rep.tar.zst
mv ./sd/stablediffusion ./
# torch 2.0
pip install torch==2.0.0+cu117 torchvision --extra-index-url https://download.pytorch.org/whl/cu117
pip install xformers==0.0.18
# torch 1.13.1
pip install torch==1.13.1+cu117 torchvision --extra-index-url https://download.pytorch.org/whl/cu117
pip install xformers==0.0.16
pip install triton
# 
pip install -r requirements_colab.txt
python webui.py --xformers --api --disable-safe-unpickle
```
