---
title: DeepLearning-06-LoadMNIST
date: 2018-06-23 12:26:46
categories: DeepLearning
tags:
- load data
- MNIST

---

## FILE FORMATS FOR THE MNIST DATABASE

The data is stored in a very simple file format designed for storing vectors and multidimensional matrices. General info on this format is given at the end of this page, but you don't need to read that to use the data files.

All the integers in the files are stored in the MSB first (high endian) format used by most non-Intel processors. Users of Intel processors and other low-endian machines must flip the bytes of the header.

There are 4 files:

`train-images-idx3-ubyte: training set images` 
`train-labels-idx1-ubyte: training set labels` 
`t10k-images-idx3-ubyte:  test set images` 
`t10k-labels-idx1-ubyte:  test set labels`

The training set contains 60000 examples, and the test set 10000 examples.

The first 5000 examples of the test set are taken from the original NIST training set. The last 5000 are taken from the original NIST test set. The first 5000 are cleaner and easier than the last 5000.

### TRAINING SET LABEL FILE (train-labels-idx1-ubyte):

| [offset] |         [type] |                       [value] |   [description] |
| -------: | -------------: | ----------------------------: | --------------: |
|     0000 | 32 bit integer | 0x00000801(2049) magic number |     (MSB first) |
|     0004 | 32 bit integer |                         60000 | number of items |
|     0008 |  unsigned byte |                            ?? |           label |
|     0009 |  unsigned byte |                            ?? |           label |
| ........ |                |                               |                 |
|     xxxx |  unsigned byte |                            ?? |           label |

`The labels values are 0 to 9.`

### TRAINING SET IMAGE FILE (train-images-idx3-ubyte):

| [offset] |         [type] |                       [value] |     [description] |
| -------: | -------------: | ----------------------------: | ----------------: |
|     0000 | 32 bit integer | 0x00000803(2051) magic number |                   |
|     0004 | 32 bit integer |                         60000 |  number of images |
|     0008 | 32 bit integer |                            28 |    number of rows |
|     0012 | 32 bit integer |                            28 | number of columns |
|     0016 |  unsigned byte |                            ?? |             pixel |
|     0017 |  unsigned byte |                            ?? |             pixel |
| ........ |                |                               |                   |
|     xxxx |  unsigned byte |                            ?? |             pixel |

Pixels are organized row-wise. Pixel values are 0 to 255. 0 means background (white), 255 means foreground (black).

### TEST SET LABEL FILE (t10k-labels-idx1-ubyte):

| [offset] |         [type] |                       [value] |   [description] |
| -------: | -------------: | ----------------------------: | --------------: |
|     0000 | 32 bit integer | 0x00000801(2049) magic number |     (MSB first) |
|     0004 | 32 bit integer |                         10000 | number of items |
|     0008 |  unsigned byte |                            ?? |           label |
|     0009 |  unsigned byte |                            ?? |           label |
| ........ |                |                               |                 |
|     xxxx |  unsigned byte |                            ?? |           label |

`The labels values are 0 to 9.`

### TEST SET IMAGE FILE (t10k-images-idx3-ubyte):

| [offset] |         [type] |                       [value] |     [description] |
| -------: | -------------: | ----------------------------: | ----------------: |
|     0000 | 32 bit integer | 0x00000803(2051) magic number |                   |
|     0004 | 32 bit integer |                         10000 |  number of images |
|     0008 | 32 bit integer |                            28 |    number of rows |
|     0012 | 32 bit integer |                            28 | number of columns |
|     0016 |  unsigned byte |                            ?? |             pixel |
|     0017 |  unsigned byte |                            ?? |             pixel |
| ........ |                |                               |                   |
|     xxxx |  unsigned byte |                            ?? |             pixel |

Pixels are organized row-wise. Pixel values are 0 to 255. 0 means background (white), 255 means foreground (black). 

------

## THE IDX FILE FORMAT

the IDX file format is a simple format for vectors and multidimensional matrices of various numerical types.

The basic format is

`magic number` 
`size in dimension 0` 
`size in dimension 1` 
`size in dimension 2` 
`.....` 
`size in dimension N` 
`data`

The magic number is an integer (MSB first). The first 2 bytes are always 0.

The third byte codes the type of the data: 
0x08: unsigned byte 
0x09: signed byte 
0x0B: short (2 bytes) 
0x0C: int (4 bytes) 
0x0D: float (4 bytes) 
0x0E: double (8 bytes)

The 4-th byte codes the number of dimensions of the vector/matrix: 1 for vectors, 2 for matrices....

The sizes in each dimension are 4-byte integers (MSB first, high endian, like in most non-Intel processors).

The data is stored like in a C array, i.e. the index in the last dimension changes the fastest. 

---

Happy hacking.

---

# load by loop

循环加载数据集，

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
import struct
from datetime import datetime
import time
import matplotlib.pyplot as plt
import numpy as np

# 数据加载器基类
class Loader(object):
    def __init__(self, path, count):
        """
        初始化加载器
        path: 数据文件路径
        count: 文件中的样本个数
        """
        self.path = path
        self.count = count

    def get_file_content(self):
        """
        读取文件内容
        """
        f = open(self.path, 'rb')
        content = f.read()
        f.close()
        return content

    def to_int(self, byte):
        """
        将unsigned byte字符转换为整数
        """
        return struct.unpack('B', byte)[0]


# 图像数据加载器
class ImageLoader(Loader):
    def get_picture(self, content, index):
        """
        内部函数，从文件中获取图像
        TEST SET IMAGE FILE (t10k-images-idx3-ubyte):
        [offset] [type]          [value]          [description]
        0000     32 bit integer  0x00000803(2051) magic number
        0004     32 bit integer  10000            number of images
        0008     32 bit integer  28               number of rows
        0012     32 bit integer  28               number of columns
        0016     unsigned byte   ??               pixel
        0017     unsigned byte   ??               pixel
        ........
        xxxx     unsigned byte   ??               pixel
        Pixels are organized row-wise. Pixel values are 0 to 255. 0 means background (white), 255 means foreground (black).
        """
        start = index * 28 * 28 + 16
        picture = []
        for i in range(28):
            picture.append([])
            for j in range(28):
                # picture[i].append(self.to_int(content[start + i * 28 + j]))
                picture[i].append(content[start + i * 28 + j])
        return picture

    def get_one_sample(self, picture):
        """
        内部函数，将图像转化为样本的输入向量
        数据降维，[[a,b],...,[c,d]] -> [a,b,...,c,d]
        """
        sample = []
        for i in range(28):
            for j in range(28):
                sample.append(picture[i][j])
        return sample

    def load(self):
        """
        加载数据文件，获得全部样本的输入向量
        """
        content = self.get_file_content()
        data_set = []
        for index in range(self.count):
            data_set.append(
                self.get_one_sample(
                    self.get_picture(content, index)))
        return data_set


# 标签数据加载器
class LabelLoader(Loader):
    def load(self):
        """
        加载数据文件，获得全部样本的标签向量
        TEST SET LABEL FILE (t10k-labels-idx1-ubyte):
        [offset] [type]          [value]          [description]
        0000     32 bit integer  0x00000801(2049) magic number (MSB first)
        0004     32 bit integer  10000            number of items
        0008     unsigned byte   ??               label
        0009     unsigned byte   ??               label
        ........
        xxxx     unsigned byte   ??               label
        The labels values are 0 to 9.
        """
        content = self.get_file_content()
        labels = []
        for index in range(self.count):
            labels.append(self.norm(content[index + 8]))
        return labels

    def norm(self, label):
        """
        内部函数，将一个值转换为10维标签向量
        数据降维，[[a,b],...,[c,d]] -> [a,b,...,c,d]
        """
        label_vec = []
        # label_value = self.to_int(label)
        label_value = label
        for i in range(10):
            if i == label_value:
                label_vec.append(0.9)
            else:
                label_vec.append(0.1)
        return label_vec


def get_training_data_set():
    """
    获得训练数据集
    """
    image_loader = ImageLoader('train-images.idx3-ubyte', 10000)
    label_loader = LabelLoader('train-labels.idx1-ubyte', 10000)
    return image_loader.load(), label_loader.load()


def get_test_data_set():
    """
    获得测试数据集
    """
    image_loader = ImageLoader('t10k-images.idx3-ubyte', 5000)
    label_loader = LabelLoader('t10k-labels.idx1-ubyte', 5000)
    return image_loader.load(), label_loader.load()

if __name__ == '__main__':
    start_time = time.time()
    images, labels = get_training_data_set()
    img = np.array(images[0]).reshape(28, 28)
    end_time = time.time()
    print('Run time: {}'.format(end_time-start_time))
    print('image shape: {}, label shape: {}'.format((len(images),len(images[0])), len(labels)))
    print(img[14])
    plt.imshow(img, cmap='gray')
    plt.show()
```

打印结果，

```
Run time: 2.680000066757202
image shape: (10000, 784), label shape: 10000
[  0   0   0   0   0   0   0   0   0   0   0   0   0  81 240 253 253 119
  25   0   0   0   0   0   0   0   0   0]
```

# load by Numpy

使用Numpy加载，

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
import struct
# from bpVec import *
from datetime import datetime
import time
import matplotlib.pyplot as plt
import numpy as np

# 数据加载器基类
class Loader(object):
    def __init__(self, path):
        """
        initialize Loader
        path: file path
        count: sample count
        data_type: data type
        dims: data dimensions
        """
        self.path = path
        self.data_type = None
        self.dims = None
        self.shape = None

    def load_raw(self):
        """ A function that can read MNIST's idx file format into numpy arrays.
        The MNIST data files can be downloaded from here:
        http://yann.lecun.com/exdb/mnist/

        This relies on the fact that the MNIST dataset consistently uses
        unsigned char types with their data segments.
        
        Reference:
        https://gist.github.com/tylerneylon/ce60e8a06e7506ac45788443f7269e40
        """
        with open(self.path, 'rb') as f:
            zero, self.data_type, self.dims = struct.unpack('>HBB', f.read(4))
            self.shape = tuple(struct.unpack('>I', f.read(4))[0] for d in range(self.dims))
            return np.fromstring(f.read(), dtype = np.uint8).reshape(self.shape)
    
    def load(self, norm=False):
        """
        convert data to vector
        """
        if norm == True:
            ret = self.load_raw() / 255
        else:
            ret = self.load_raw()
        if self.dims > 1:
            return ret.reshape(self.shape[0], -1)
        else:
            return ret

# 图像数据加载器
class ImageLoader(Loader):
    """
    Get all images
    TEST SET IMAGE FILE (t10k-images-idx3-ubyte):
    [offset] [type]          [value]          [description]
    0000     32 bit integer  0x00000803(2051) magic number
    0004     32 bit integer  10000            number of images
    0008     32 bit integer  28               number of rows
    0012     32 bit integer  28               number of columns
    0016     unsigned byte   ??               pixel
    0017     unsigned byte   ??               pixel
    ........
    xxxx     unsigned byte   ??               pixel
    Pixels are organized row-wise. Pixel values are 0 to 255. 0 means background (white), 255 means foreground (black).
    """
    def __init__(self, path):
        super(ImageLoader, self).__init__(path)

    def get_one_image(self, index):
        assert self.dims > 1
        return self.load_raw()[index]

    def show_one_image(self, image):
        """
        plt show one image
        """
        plt.imshow(image, cmap='gray')
        plt.show()


# 标签数据加载器
class LabelLoader(Loader):
    """
    Get all labels
    Convert labels to one-hot vectors
    TEST SET LABEL FILE (t10k-labels-idx1-ubyte):
    [offset] [type]          [value]          [description]
    0000     32 bit integer  0x00000801(2049) magic number (MSB first)
    0004     32 bit integer  10000            number of items
    0008     unsigned byte   ??               label
    0009     unsigned byte   ??               label
    ........
    xxxx     unsigned byte   ??               label
    The labels values are 0 to 9.
    """
    def __init__(self, path):
        super(LabelLoader, self).__init__(path)

    def norm(self, label, label_count=10):
        """
        (sample_count, )-->(sample_count, label_count)
        """
        row_count = self.shape[0]
        column_count = label_count
        label_one_hot = np.zeros((row_count, column_count))
        
        label_one_hot[range(row_count), self.load()] = 1
        
        return label_one_hot

def get_training_data_set(sample_count=100):
    """
    get training data set
    """
    image_population = ImageLoader('train-images.idx3-ubyte').load()
    label_population = LabelLoader('train-labels.idx1-ubyte').load()
    assert len(image_population) == len(label_population)
    population_count = len(image_population)
    index = np.random.choice(population_count, sample_count)
    return image_population[index], label_population[index]


def get_test_data_set(sample_count=50):
    """
    get test data set
    """
    image_population = ImageLoader('t10k-images.idx3-ubyte').load()
    label_population = LabelLoader('t10k-labels.idx1-ubyte').load()
    assert len(image_population) == len(label_population)
    population_count = len(image_population)
    index = np.random.choice(population_count, sample_count)
    return image_population[index], label_population[index]


if __name__ == '__main__':
    start_time = time.time()
    images, labels = get_training_data_set(10000)
    img = np.array(images[0]).reshape(28, 28)
    end_time = time.time()
    print('Run time: {}'.format(end_time-start_time))
    print('image shape: {}, label shape: {}'.format(images.shape, labels.shape))
    print(img[14])
    plt.imshow(img, cmap='gray')
    plt.show()
```

打印结果，

```
Run time: 0.0839998722076416
image shape: (10000, 784), label shape: (10000,)
[  0   0   0   0   0   0   0   0   0   0   0   0  93 253 253 255 253 253
 241  39   0   0   0   0   0   0   0   0]
```

# Reference

[THE MNIST DATABASE of handwritten digits](http://yann.lecun.com/exdb/mnist/)

https://github.com/hanbt/learn_dl/blob/master/mnist.py