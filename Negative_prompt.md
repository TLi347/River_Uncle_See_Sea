# what is classifier-free guidance sampling
## classifier guidance:

![image](https://user-images.githubusercontent.com/53488014/222857197-4da481dd-2559-458d-aa51-a454f17f8098.png)

其中：$ \lambda $ 是time step；$\sigma$：是diffusion过程的超参数； $p(\mathbf{z})$：是z的边缘分布

由于classifier guidance将在sample时把score estimate与分类器的梯度混合在一起，因此classifier- guidance的采样可以解释为试图混淆 分类器 和 gradient-based-adversarial-attack。所以很难说，classifier- guidance的成功是不是因为分类器的对抗性。骗过了一个并不健壮的分类器并不代表我们确实逼近了条件分布。

## classifier-free guidance:

不是在图像分类器的梯度方向上进行采样，而是将条件扩散模型的得分估计和一个共同训练的无条件扩散模型混合在一起。

构建一个隐式的分类器梯度，只关于 $P(\mathbf{z\vert c})$ 和 $P(\mathbf{z})$ 而无需一个显式的分类器 $P(\mathbf{c\vert z})$。

![image](https://user-images.githubusercontent.com/53488014/222857263-8d8326ef-41ae-4466-b8d6-a881e314846b.png)

the advantage of classifier-free guidance sampling

# ControlNet vs. T2I vs. Composers

