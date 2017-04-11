# Accelerating SqueezeNet on FPGA

<p align="right"><b>Megha Arora & Samyukta Lanka</b></p>
## Summary
We will accelerate SqueezeNet\[[0][0]\] (a CNN variant) on an FPGA

## Background 
We chose DNNs because they are revolutionizing many areas of computing today and there is parallelism that the underlying model possesses. The sequence of multiply/add operations (like FMA) to calculate the input to each layer in the network are highly parallelizable. In particular, there are multiple axis of parallelism we plan to explore - 

Layer parallelism - different layers being processed in parallel (low level of parallelism) [maybe use pipelining to exploit this]

Node parallelism - Here the axis of parallelism will be each neuron (there can be millions in a DNN); most prominent in this problem. Entails high level of parallelism and FPGA ‘cells’ are ideal for this

Data parallelism - Computing the dot product of the input and the weights at each step (including convolution) in parallel.

An extremely popular DNN is Covolutional Neural Network(CNN) which is extensively used in the domain of computer vision. SqueezeNet was created to combat the large number of parameters required for CNNs. Based on AlexNet\[[1][1]\] , the purpose was to reduce the memory required without losing accuracy. We will be accelerating SqueezeNet on the Zybo Zync-7020 FPGA. 

![SqueezeNet Architecture](https://ai2-s2-public.s3.amazonaws.com/figures/2016-11-08/0919b3ab82ff8a052490389f217c480c273f2b92/1-Figure2-1.png)
Figure 1: The SqueezeNet Architecture

## Challenge
All the different levels of parallelism vary drastically and will be traded off against each other. Some are more prominent in the problem as compared to others. Varying amount of effort and benefit is involved in each, which would require extensive amount of platform knowledge (FPGA in this case) and testing multiple implementations! Deciding which one to prioritize at what cost will be challenging! Also, some types of parallelism may not be suitable for us to implement on the FPGA depending on the design (eg: exploiting bit-level parallelism on FPGA).

One of the major challenges that we will be dealing with is being memory bound on the FPGA. The specific device that we have has 256 KB of on-chip memory with options for external memory. The size of SqueezeNet is much larger than this and would be difficult to fit on the chip. This means we will also have to ensure data locality and reuse.

## Resources
We will use the Zybo Zync-7020 FPGA for this task.
We do not have a codebase to refer to; the initial FPGA implementation will be our first goal. We will be referring to the following implementations/papers extensively -
 
SqueezeNet\[[0][0]\]

Implementation of SqueezeNet-like CNN on FPGA\[[2][2]\]

FPGA Implementations of Neural Networks\[[3][3]\]


## Goals and Deliverables
Plan to Achieve: Get a **working baseline FPGA implementation** that exceeds CPU performance without compromising accuracy. 

Hope to Achieve: Fit everything in on-chip memory

Demo: Simultaneously show SqueezeNet in action on multiple images with one version running the CPU implementation and the other running the FPGA implementation. We will also present the performance of our implementation as compared to [existing benchmarks](https://github.com/soumith/convnet-benchmarks) through speedup graphs. 


## Platform
General purpose processors fail to fully exploit the parallelism inherent in DNNs. For instance it can take upto 3 weeks to train a 22 layer CNN for image classification (ImageNet) using 1 NVIDIA K20 GPU. The drawback of using an ASIC neurocomputer for this task also has its own challenges. First, devices that work on special applications always lag behind the mass-produced devices in terms of innovation. Given the variability in ANN architectures, the ASIC neurocomputer cannot be extended to many different architectures without involving significant cost!

FPGAs on the other hand offer the best balance of cost:performance and flexibility for this domain as compared to ASIC neurocomputers or state-of-the-art multi-purpose processors. Essentially, they can be used as **‘semi-custom’ machines for neural networks**.


## Schedule



| Date                    | Milestone                                                          |
| ------------------------|:-------------------------------------------------------------------|
| April 10<sup>th</sup>   | Proposal                                                           |
| April 16<sup>th</sup>   | Convolutions and Max Pooling Layers Working Separately on FPGA     |
| April 23<sup>th</sup>   | Full SqueezeNet Working on FPGA                                    |
| April 30<sup>th</sup>   | Acceleration + Optimizations                                       |
| May 7<sup>th</sup>      | Benchmarking                                                       |
| May 11<sup>th</sup>     | Result Collation + Report Writing                                  |


## References
\[0\]: https://arxiv.org/pdf/1602.07360.pdf

\[1\]: https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf

\[2\]: https://github.com/dgschwend/zynqnet

\[3\]: http://lab.fs.uni-lj.si/lasin/wp/IMIT_files/neural/doc/Omondi2006.pdf

[0]: https://arxiv.org/pdf/1602.07360.pdf
[1]: https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf
[2]: https://github.com/dgschwend/zynqnet
[3]: http://lab.fs.uni-lj.si/lasin/wp/IMIT_files/neural/doc/Omondi2006.pdf


