Accelerating SqueezeNet on FPGA  - Megha Arora, Samyukta Lanka

# Proposal

## Summary
We are going to accelerate SqueezeNet[0] (a CNN variant) on an FPGA.

## Background 
We chose DNNs because they are revolutionizing many areas of computing today and there is parallelism that the underlying model possesses. The sequence of multiply/add operations (like FMA) to calculate the input to each layer in the network are highly parallelizable. In particular, there are multiple axis of parallelism we plan to explore - 

Layer parallelism - different layers being processed in parallel (low level of parallelism) [maybe use pipelining to exploit this]
Node parallelism - Here the axis of parallelism will be each neuron (there can be millions in a DNN); most prominent in this problem. Entails high level of parallelism and FPGA ‘cells’ are ideal for this
Data parallelism - Computing the dot product of the input and the weights at each step in parallel (including Convolution in parallel).

SqueezeNet was created to combat the large number of parameters required for CNNs. Based on AlexNet[1] , the purpose was to reduce the memory required without losing accuracy. We will be accelerating SqueezeNet on the Zybo Zync-7020. 

Insert: SqueezeNet Architecture

## Challenge
All the different levels of parallelism vary drastically and will be traded off against each other. Some are more prominent in the problem as compared to others. Varying amount of effort and benefit is involved in each, which would require extensive amount of platform knowledge (FPGA in this case) and testing multiple implementations! Deciding which one to prioritize at what cost will be challenging! Also, some types of parallelism may not be suitable for us to implement on the FPGA depending on the design (eg: exploiting bit-level parallelism on FPGA).

One of the major challenges that we will be dealing with is being memory bound on the FPGA. The specific device that we have has 256 KB of on chip memory with options for external memory. The size of SqueezeNet is much larger than this and would be difficult to fit on the chip. This means we will also have to ensure data locality and reuse!

## Resources
We will use the Zybo Zync-7020 FPGA for this task.
We do not have a baseline FPGA implementation to refer to - the implementation of which will be our first goal. We will be referring to the following implementations/papers extensively -
 
SqueezeNet: https://arxiv.org/pdf/1602.07360.pdf
Implementation of SqueezeNet-like CNN on FPGA: https://github.com/dgschwend/zynqnet
FPGA Implementations of Neural Networks: http://lab.fs.uni-lj.si/lasin/wp/IMIT_files/neural/doc/Omondi2006.pdf


## Goals and Deliverables
Get a working baseline FPGA implementation that exceeds CPU performance without compromising accuracy. <Plan to Achieve>
Fit everything in on-chip memory <Hope to achieve>

Demo - 
Have two laptops side by side and run classification of the neural network on different images where one laptop is running the CPU implementation and the other is hooked up to the FPGA. If the speedup is good enough, we should be able to really show the benefits of the FPGA implementation. 
We will also include speedup graphs to show the average trends 


## Platform
General purpose processors fail to fully exploit the parallelism inherent in DNNs. For instance it can take upto 3 weeks to train a 22 layer CNN for image classification (ImageNet) using 1 NVIDIA K20 GPU. The drawback of using an ASIC neurocomputer for this task also has its own challenges. First, devices that work on special applications always lag behind the mass-produced devices in terms of innovation. Given the variability in ANN architectures, the ASIC neurocomputer cannot be extended to many different architectures without involving significant cost!
FPGAs on the other hand offer the best balance of cost:performance and flexibility for this domain as compared to ASIC neurocomputers or state-of-the-art multi-purpose processors. Essentially, they can be used as ‘semi-custom’ machines for neural networks.


## Schedule

Week (finish till)     Milestone

April 10th             Proposal 

April 16th             Convolutions and Max Pooling Layers Working Separately on FPGA

April 23rd             Full SqueezeNet Working on FPGA

April 30th             Acceleration + Optimizations

May 7th                Benchmarking

May 12th               Result Collation + Report Writing


## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/lankas/15-618Project/edit/master/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/lankas/15-618Project/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
