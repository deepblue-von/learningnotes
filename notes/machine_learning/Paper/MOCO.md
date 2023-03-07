# transformation

<img src="MOCO.assets/image-20230303143259925.png" alt="image-20230303143259925" style="zoom:50%;" />

将一张图片做裁剪和数据增广，经过裁剪之后语义信息不变

## 动量(momentum)

y~t~ = m ·y~t-1~ + (1-m)x~t~

不想让当前时刻的输出完全依赖当前时刻的输入

# 对比学习

<img src="MOCO.assets/image-20230303143147921.png" alt="image-20230303143147921" style="zoom:50%;" />

# NCE loss

noise contrastive estimation