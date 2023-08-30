Swin-Unet consists of encoder, bottleneck, decoder and skip connections. The basic unit of Swin-Unet is Swin Transformer block。

The transformed patch tokens pass through several Swin Transformer blocks and patch merging layers to generate the hierarchical feature representations.

- patch merging layer is responsible for down-sampling and increasing dimension
- Swin Transformer block is responsible for feature representation learning
- Inspired by U-Net, we design a symmetric transformer-based decoder.
- The decoder is composed of Swin Transformer block and patch expanding layer.
- In contrast to patch merging layer, a patch expanding layer is specially designed to perform up-sampling.

![](images/Pasted%20image%2020230717202139.png)

dsa