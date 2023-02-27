# Untitled

这篇论文的主要内容是为了解决text-image生成模型不能完全对齐prompt和image的问题。key-idea是提出结合RL（reinforcement learning）的思路。首先收集人类对于<iamge, prompt>（生成的图像和对应的prompt）的评价构建一个<feedback-image-prompt>的数据集，然后用收集的数据集训练一个网络作为reward function，最后利用这个reward function再fine-tune图像生成模型，从而实现生成图像和prompt完全

RLHF方法在NLP上有广泛的运用和很好的效果。在[]中，RLHF使语言生成模型更加遵守指令，较少的凭空捏造事实（拒绝其知识范围之外的问题），减少有害输出。

- 如何搜集数据集的：
    - 本文中只针对三个概念进行了实验：count, color\background, category。
    - 针对上面三个概念构建prompt集，例如“two green dogs in a city”，并生成<image,prompt> pair。
    - 让人类评估每个<image, prompt> pair的一致程度，只有“good”和“not good”两种评价选择。
- 如何训练reward function的：
    - data augmentation scheme:
        - 对于每个评价为"good"的<image, prompt> pair，生成N-1个不同语义的prompt组成增强训练集，比如对于prompt="a red dog"的增强集有 "a blue dog", "a green dog"
  
            $\mathcal{D}^{\mathrm{txt}}= \{(\mathbf{x},{(\mathbf{z}_j)}^N_1,i')\} $
            
    - reward function $r_\phi(x; z)$
        - 损失函数：
            
            $\mathcal{L}^{\mathrm{reward}}(\phi) = \mathcal{L}^{\mathrm{MSE}}(\phi) + \lambda\mathcal{L}^{pc}(\phi)$
            
            其中
            
            - $\mathcal{L}^{\mathrm{MSE}}(\phi)=\mathbb{E}{(\mathbf{x,z},y)\sim \mathcal{D}^{\mathrm{human}}}[ ( y - r\phi(\mathbf{x,z}))^2 ]$
            
            - $\mathbf{x,z}$ 是image $\mathbf{x}$ 和text $\mathbf{z}$ 在CLIP中的embedding
            - $\mathcal{L}^{\mathrm{pc}}=\mathbb{E}_{ (\mathbf{x}, (\mathbf{z}_j)_j^N, i')\sim\mathcal{D}^{\mathrm{txt}}} [\mathcal{L}^{\mathrm{CE}}]$

  
              $\mathcal{L}^{\mathrm{CE}}[(P_\phi(i\vert \mathbf{x}, (\mathbf{z}_j)_j^N),i')]$


            - L^PC 是auxiliary loss
            - L^CE 里是augmentation的loss
        - auxiliary loss
        
            ![image](https://user-images.githubusercontent.com/53488014/221588007-a2a614c2-ad39-4361-adaa-7972d67b3282.png)
        
            - $i'$：index of original prompt
            - where T > 0 is the temperature.
    - 如何用reward function 训练 DM的
        - $p$：DM model
        - $\theta$：DM model 的参数
        - loss function:
            - ![image](https://user-images.githubusercontent.com/53488014/221588379-31d64637-515b-44a6-b5e8-76fb44c3e032.png)
            - $D^{\mathrm{model}} = D^{\mathrm{human}} \bigcup D^{\mathrm{unlabel}}$ 是生成图像集
            - $D^{\mathrm{pre}}$:is the pre-training dataset
- 实验结果总结
    - 生成图像和文本的一致程度提高，但是图像清晰度下滑
    - 目前还没有真正做到和RL算法结合
  
