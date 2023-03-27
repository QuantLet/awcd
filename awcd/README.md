[<img src="https://github.com/QuantLet/Styleguide-and-FAQ/blob/master/pictures/banner.png" width="1100" alt="Visit QuantNet">](http://quantlet.de/)

## [<img src="https://github.com/QuantLet/Styleguide-and-FAQ/blob/master/pictures/qloqo.png" alt="Visit QuantNet">](http://quantlet.de/) **awcd** [<img src="https://github.com/QuantLet/Styleguide-and-FAQ/blob/master/pictures/QN2.png" width="60" alt="Visit QuantNet 2.0">](http://quantlet.de/)

```yaml

Name of Quantlet: awcd

Published in: Phd Thesis

Description:  'Implementation of the Adaptive Weights Community Detection algorithm.'

Keywords: community-detection, kullback-leibler, nonparametric, adaptive-weights, gap-coefficient, graph-clustering, overlapping-communities

Author: Larisa Adamyan, Kirill Efimov, Vladimir Spokoiny

Submitted [python]: Tue, February 12 2019 by Larisa Adamyan

Output: 'plot of 3 windows: 1. the true weight matrix of a graph (white means 1, black is 0), 2. the adjacency matrix,  3. the final weight matrix detected by AWCD.'

```

![Picture1](Figure_2.png)

### PYTHON Code
```python

import numpy as np
import matplotlib as mpl
mpl.use('TkAgg')
from src.AWCD import AWCD
from src.awcd_help import clustering_from_weights_sparse
from utils.graph_generator import generate_sbm
from utils.scores import NMI, modularity_calc

def run_awcd_example():
	'''
	Run AWCD on SBM with edge density parameters t_in and t_out.
	Plot the weight matrix on each step.
	Print the NMI and Modularity values for the final result.
	:return:
	'''
	n_list = [100, 100, 100, 100]
	t_in = 0.2
	to_out = 0.07
	theta = np.array(
		[[t_in, to_out, to_out, to_out],
		 [to_out, t_in, to_out, to_out],
		 [to_out, to_out, t_in, to_out],
		 [to_out, to_out, to_out, t_in]]
		)
	C_true, A, n = generate_sbm(n_list, theta)
	params = {
		'distance': 'wa', # wa (W * A) for fast calculation, 'ww' (W * W) for full calculation.
		'steps': 10 # number of AWCD update steps
		}
	weights = AWCD(A, l=20, sparse=True, show_step=True, show_finish=False, C_true=C_true, params=params)
	clustering = clustering_from_weights_sparse(weights)
	print ('NMI {}'.format(NMI(C_true, clustering, n=n)))
	print ('modulairty {}'.format(modularity_calc(weights, sparse=True, n=n)))

run_awcd_example()

```

automatically created on 2023-03-27