# scikit-image: Image processing in Python

[![Image.sc forum](https://img.shields.io/badge/dynamic/json.svg?label=forum&url=https%3A%2F%2Fforum.image.sc%2Ftags%2Fscikit-image.json&query=%24.topic_list.tags.0.topic_count&colorB=brightgreen&suffix=%20topics&logo=data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA4AAAAOCAYAAAAfSC3RAAABPklEQVR42m3SyyqFURTA8Y2BER0TDyExZ+aSPIKUlPIITFzKeQWXwhBlQrmFgUzMMFLKZeguBu5y+//17dP3nc5vuPdee6299gohUYYaDGOyyACq4JmQVoFujOMR77hNfOAGM+hBOQqB9TjHD36xhAa04RCuuXeKOvwHVWIKL9jCK2bRiV284QgL8MwEjAneeo9VNOEaBhzALGtoRy02cIcWhE34jj5YxgW+E5Z4iTPkMYpPLCNY3hdOYEfNbKYdmNngZ1jyEzw7h7AIb3fRTQ95OAZ6yQpGYHMMtOTgouktYwxuXsHgWLLl+4x++Kx1FJrjLTagA77bTPvYgw1rRqY56e+w7GNYsqX6JfPwi7aR+Y5SA+BXtKIRfkfJAYgj14tpOF6+I46c4/cAM3UhM3JxyKsxiOIhH0IO6SH/A1Kb1WBeUjbkAAAAAElFTkSuQmCC)](https://forum.image.sc/tags/scikit-image)
[![Stackoverflow](https://img.shields.io/badge/stackoverflow-Ask%20questions-blue.svg)](https://stackoverflow.com/questions/tagged/scikit-image)
[![project chat](https://img.shields.io/badge/zulip-join_chat-brightgreen.svg)](https://skimage.zulipchat.com)
[![Scientific Python Ecosystem Coordination](https://img.shields.io/badge/SPEC-0,1,4,6,7,8-green?labelColor=%23004811&color=%235CA038)](https://scientific-python.org/specs/)

- **Website (including documentation):** [https://scikit-image.org/](https://scikit-image.org)
- **Documentation:** [https://scikit-image.org/docs/stable/](https://scikit-image.org/docs/stable/)
- **User forum:** [https://forum.image.sc/tag/scikit-image](https://forum.image.sc/tag/scikit-image)
- **Developer forum:** [https://discuss.scientific-python.org/c/contributor/skimage](https://discuss.scientific-python.org/c/contributor/skimage)
- **Source:** [https://github.com/scikit-image/scikit-image](https://github.com/scikit-image/scikit-image)

## Installation

- **pip:** `pip install scikit-image`
- **conda:** `conda install -c conda-forge scikit-image`

Also see [installing `scikit-image`](https://github.com/scikit-image/scikit-image/blob/main/INSTALL.rst).

## License

See [LICENSE.txt](https://github.com/scikit-image/scikit-image/blob/main/LICENSE.txt).

## Citation

If you find this project useful, please cite:

> Stéfan van der Walt, Johannes L. Schönberger, Juan Nunez-Iglesias,
> François Boulogne, Joshua D. Warner, Neil Yager, Emmanuelle
> Gouillart, Tony Yu, and the scikit-image contributors.
> _scikit-image: Image processing in Python_. PeerJ 2:e453 (2014)
> https://doi.org/10.7717/peerj.453



## 2025-10-16----------------------------以下为自己添加
# 从源码进行编译scikit-image
从github 克隆源码，然后建议虚拟环境进行python的编译（这里与C++不同）
- step 1:git clone XXX
- step 2:创建虚拟环境：conda create -n skimage_envs python=3.11，进入代码文件夹：cd ~/scikit-image，打开终端并进入虚拟环境：conda activate skimage_envs
- step 3:安装spin:pip stall spin,编译：spin install -v(每次修改了_skeletonize_lee_cy.pyx.in代码，编译会有4步，最后生成.so文件，代表修改和编译成功)
- 如果spin build过程中遇到没有src/skimage/restoration/unwrap_2d_ljmu.c等文件，去giyhub源码仓库看，大概率是克隆不完整，单独把这个文件下载到对应路径下。
- 注：编译之前，需要把skimage-image目录下的_skeletonize_lee_cy.pyx.in文件替换home/pc/scikit-image/src/skimage/morphology/_skeletonize_lee_cy.pyx.in这个文件。


# 比较重要的几个代码文件：/home/pc/scikit-image/src/skimage/morphology目录下的:
- __init__.py 里面包含了这个skimage.morphology导出的模块信息
- _skeletonize.py 里面有几个主要的2D、3D骨架提取函数，细化函数，中轴提取函数
- _skeletonize_lee_cy.pyx.in 最重要的文件，整个是用cython语言实现的用lee的方法提取的三维骨架函数，_compute_thin_image是主函数，里面包含了重要的判断简单点函数（连通性不变）、欧拉特性函数、端点、边界点函数.

# 一个解决最后提取的三维骨架为空的：
- 问题：可能是与image的维度的奇偶数有关
- https://github.com/scikit-image/scikit-image/issues/3757
