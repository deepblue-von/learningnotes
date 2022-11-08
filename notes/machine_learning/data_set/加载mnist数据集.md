# 加载mnist数据集

```python
import struct
import numpy as np
import matplotlib.pyplot as plt


def load_mnist():
    # 读取训练集标签
    with open('../data/mnist/train-labels.idx1-ubyte', 'rb') as train_lbpath:
        train_labels_magic, train_labels_num = struct.unpack('>II',
                                                    train_lbpath.read(8))
        train_labels = np.fromfile(train_lbpath, dtype=np.uint8)
    # 读取训练集图片
    with open('../data/mnist/train-images.idx3-ubyte', 'rb') as train_imgpath:
        train_images_magic, train_image_num, train_rows, train_cols = \
            struct.unpack('>IIII', train_imgpath.read(16))
        train_images = np.fromfile(train_imgpath, dtype=np.uint8).reshape(
            train_image_num, train_rows * train_cols)

    # 读取测试集标签
    with open('../data/mnist/t10k-labels.idx1-ubyte', 'rb') as test_lbpath:
        test_labels_magic, test_labels_num = struct.unpack('>II',
                                                           test_lbpath.read(8))
        test_labels = np.fromfile(test_lbpath, dtype=np.uint8)
    # 读取测试集图片
    with open('../data/mnist/t10k-images.idx3-ubyte', 'rb') as test_imgpath:
        test_images_magic, test_image_num, test_rows, test_cols = \
            struct.unpack('>IIII', test_imgpath.read(16))
        test_images = np.fromfile(test_imgpath, dtype=np.uint8).reshape(
            test_image_num, test_rows * test_cols)
    return train_labels, train_images, test_labels, test_images



t_train, x_train, t_test, x_test = load_mnist()

# labels, images, labels_magic, labels_num, images_magic, image_num,\
#                                     rows, cols = load_mnist()
#
# # 打印数据信息
#
# print(f"labels_magic is {labels_magic}\n",
#       f"labels_num is {labels_num}\n",
#       f"labels is {labels}\n")
#
# print(f"images_magic id {images_magic}\n",
#       f"images_num id {image_num}\n",
#       f"rows is {rows}\n"
#       f"cols is {cols}\n"
#       f"images is {images}\n")


# 分别从测试集和训练集取出一张图片
choose_num = np.random.randint(9)
train_label = t_train[choose_num]
train_image = x_train[choose_num].reshape(28, 28)

test_label = t_test[choose_num]
test_image = x_test[choose_num].reshape(28, 28)
plt.imshow(train_image)
plt.title(f"the label is :{train_label}")
plt.show()
plt.imshow(test_image)
plt.title(f"the label is :{test_label}")
plt.show()
```

