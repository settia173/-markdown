### 安装基于PyTorch的轻量级微调推理框架ms-swift

pip install swift

```
一些命令

swift web-ui --help
//不懂的命令查看help和文档

export ALL_PROXY=''
export all_proxy=''

//清空端口

watch -n 1 nvidia-smi 
//每一秒观看gpu显存情况
```

下载国产DeepSeek VL 7B

从github下载，huggingface下载7B权重。

cli 部署dpvl

--model_path "/home/aeon/Desktop/ds/deepseek-vl-7b-chat"

![](../assets/2025-01-15-13-14-16-1.png)

### 处理数据

查看官方对自定义数据集的要求，写脚本得到jason

![](../assets/2025-01-15-13-15-01-2025-01-15%2012-28-34%20的屏幕截图.png)

回答部分还有问题，拿到串该split多余的符号

/home/aeon/Desktop/dataset/xraydata/DATA/chest_xray8/output.json

官方提供caption.sh进行微调，大约2days，2epoch。

![](../assets/2025-01-15-13-15-28-2025-01-14%2014-01-40%20的屏幕截图.png)

watch -n 1 nvidia-smi    //每一秒观看gpu显存情况

![](../assets/2025-01-15-13-16-34-2025-01-15%2010-09-23%20的屏幕截图.png)![](../assets/2025-01-15-13-15-48-2025-01-14%2019-47-53%20的屏幕截图.png)

![](../assets/2025-01-15-13-15-58-2025-01-15%2008-21-45%20的屏幕截图.png)

train了18.5小时，evl loss和loss都没再变低了，stop。

###### CUDA_VISIBLE_DEVICES=0 swift export --ckpt_dir '/home/aeon/Desktop/ds/output/v0-20250114-135424/checkpoint-2500' --model_path '/home/aeon/Desktop/ds/deepseek-vl-7b-chat'  --model-type 'deepseek_vl'  --merge_lora true

## 融合

CUDA_VISIBLE_DEVICES=0 swift export \
    --model /home/aeon/Desktop/ds/deepseek-vl-7b-chat \
    --adapters /home/aeon/Desktop/ds/output/v0-20250114-135424/checkpoint-2500 \
    --merge_lora true

推理

CUDA_VISIBLE_DEVICES=0 swift infer \
    --ckpt_dir /home/aeon/Desktop/ds/output/v0-20250114-135424/checkpoint-2500-merged \
    --load_dataset_config true

CUDA_VISIBLE_DEVICES=0 swift deploy \
    --host 0.0.0.0 \
    --port 8000 \
    --adapters lora1=swift/test_lora lora2=swift/test_lora2 \
    --infer_backend vllm

CUDA_VISIBLE_DEVICES=0 swift web-ui \

    --host 0.0.0.0 \
    --port 8000 \

    --infer_backend vllm \

    --model /home/aeon/Desktop/ds/output/v0-20250114-135424/checkpoint-2500-merged

python cli_chat.py --model_path "/home/aeon/Desktop/ds/output/v0-20250114-135424/checkpoint-2500-merged"

"describe this pictrue </home/aeon/Desktop/dataset/00000001_000.png>"

部署到界面galio

CUDA_VISIBLE_DEVICES=0 \
swift app \
  --model '/home/aeon/Desktop/ds/output/v0-20250114-135424/checkpoint-2500-merged' \
  --infer_backend pt \
  --stream true \
  --max_new_tokens 2048

![](../assets/2025-01-15-13-17-00-2025-01-15%2012-23-04%20的屏幕截图.png)

似乎不是很准。

![](../assets/2025-01-15-13-17-12-2025-01-15%2012-25-53%20的屏幕截图.png)

它还是说英文

![](../assets/2025-01-15-13-17-20-2025-01-15%2012-26-58%20的屏幕截图.png)

结尾总有一个符号】？

<img src="../assets/2025-01-15-13-17-30-2025-01-15%2012-28-34%20的屏幕截图.png" title="" alt="" data-align="inline">

处理数据集的时候忘记split这个符号，以及去掉中间的‘’引用号

。。。。

![](../assets/2025-01-15-13-17-45-2025-01-15%2012-40-21%20的屏幕截图.png)

7B占用有点高
