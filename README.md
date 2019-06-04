## 简介

​	PCMCI因果分析。



## 依赖

​	Tigramite



## 包含内容





## 函数说明

`dtigramite.pcmci.PCMCI(dataframe, cond_ind_test, selected_variables=None, verbosity=0)`

​	PCMCI是一种针对大规模时间序列数据集的两步因果发现方法。第一步是条件选择，然后是MCI条件独立性测试。实现基于[1](<https://arxiv.org/abs/1702.07007v2>)中的算法1和算法2。

+ Examples

```
>>> import numpy
>>> from tigramite.pcmci import PCMCI
>>> from tigramite.independence_tests import ParCorr
>>> import tigramite.data_processing as pp
>>> numpy.random.seed(42)
>>> # Example process to play around with
>>> # Each key refers to a variable and the incoming links are supplied as a
>>> # list of format [((driver, lag), coeff), ...]
>>> links_coeffs = {0: [((0, -1), 0.8)],
                    1: [((1, -1), 0.8), ((0, -1), 0.5)],
                    2: [((2, -1), 0.8), ((1, -2), -0.6)]}
>>> data, _ = pp.var_process(links_coeffs, T=1000)
>>> # Data must be array of shape (time, variables)
>>> print data.shape
(1000, 3)
>>> dataframe = pp.DataFrame(data)
>>> cond_ind_test = ParCorr()
>>> pcmci = PCMCI(dataframe=dataframe, cond_ind_test=cond_ind_test)
>>> results = pcmci.run_pcmci(tau_max=2, pc_alpha=None)
>>> pcmci._print_significant_links(p_matrix=results['p_matrix'],
                                     val_matrix=results['val_matrix'],
                                     alpha_level=0.05)
## Significant parents at alpha = 0.05:
    Variable 0 has 1 parent(s):
        (0 -1): pval = 0.00000 | val = 0.623
    Variable 1 has 2 parent(s):
        (1 -1): pval = 0.00000 | val = 0.601
        (0 -1): pval = 0.00000 | val = 0.487
    Variable 2 has 2 parent(s):
        (2 -1): pval = 0.00000 | val = 0.597
        (1 -2): pval = 0.00000 | val = -0.511
```

