# 在服务器上 GridSearch 超参数的记录

此文件记录了在 bash 脚本 param_select.sh 指令下搜索最佳参数的结果。

## 搜索 word_embedding 和 slot_embedding 的维度

剩余未调节参数：使用优化算法 adagrad、batch 大小 32、训练轮数 800、Encoder 层数 1、LSTM 隐层大小 64。

| w/s 维度          | 128/64   | 128/32   | 128/16   |  128/8   | 64/64     | 64/32      | 64/16    | 64/8   |
| :-----------      | :----:   | :---:    | :---:    |  :---:   | :---:     | :---:      | :----:   | :---:  |
|   **SF**   F_1    | 0.917340 | 0.932424 | 0.929261 | 0.932256 | 0.920261  | 0.933458   | 0.932752 | 0.934452 |
|   **ID** Accuracy | 0.950000 | 0.928000 | 0.928000 | 0.930000 | 0.914000  | 0.910000   | 0.926000 | 0.908000 |

| w/s 维度           | 32/64    | 32/32    |  32/16  | 32/8    | 
| :-----------       |:----:    |:----:    |:----:   | :----:   |
|   **SF**   F_1    | 0.922693 | 0.915964 | 0.920299 | 0.926204  |
|   **ID** Accuracy | 0.910000 | 0.926000 | 0.910000 | 0.930000  |

经过权衡计算量、Slot Filling 的 F1 值、Intent Detection 的精确度，选择参数 w/s 维度 = 64/16。

## 搜索 batch 大小和 LSTM 隐层大小
剩余未调节参数：使用优化算法 adagrad、训练轮数 **2000**(保证在 batch 很小时也能完整读过 3+ 次数据)、Encoder 层数 1。

| b/h 大小          | 8/64   | 8/128   | 8/256   |  16/64   | 16/128     | 16/256      | 32/64    | 32/128   |
| :-----------      | :----:   | :---:    | :---:    |  :---:   | :---:     | :---:      | :----:   | :---:  |
|   **SF**   F_1    | 0.915603 | 0.926466 | 0.925096 | 0.928660 | 0.932214  | 0.919440  | 0.940150 | 0.941323 |
|   **ID** Accuracy | 0.934000 | 0.946000 | 0.942000 | 0.928000 | 0.918000  | 0.922000   | 0.946000 | 0.936000 |
| 收敛轮数      | 1560    |   1590  |  1680   |  1980    |   960      |    1260  |  1320    |   480    |

|  b/h 大小           | 32/256    | 64/64    |  64/128  | 64/256    | 
| :-----------       |:----:    |:----:    |:----:   | :----:   |
|   **SF**   F_1    | 0.911811 | 0.945274 | 0.952912 | 0.927970  |
|   **ID** Accuracy | 0.916000 | 0.936000 | 0.942000 | 0.890000  |
| 收敛轮数           | 1050|  690   | 540      |  870     |

通过诸方面比较(注意 **SF** 的效果应该优先考虑)，应该选择 batch 大小为 64, 隐层 hidden 维度大小为 128。