---
layout: post
comments: true
categories: Reasoning
title: "Rooted-Join-Tree Construnction"
---


For computing multiple marginals efficiently in the <em><span style="font-family: 'comic sans ms', sans-serif;">Shenoy-Shafer </span></em>architecture, a binary join tree has been proposed in [1].

For a given <em><span style="font-family: 'comic sans ms', sans-serif;">valuation network</span></em> $$ \{ \Psi, \{ \Omega_X \}_{X \in \Psi},\{\tau_1,\dots,\tau_m \}, \downarrow, \otimes \}$$, if we want to compute marginal for a subset $$ t \subseteq \Psi$$, we can use the <span style="font-family: 'comic sans ms', sans-serif;"><em>Fusion algorithm </em></span> .

<span style="font-size: 14px;"><em><span style="font-family: 'comic sans ms', sans-serif;">Fusion algorithm: Suppose $$\{\{\tau_1,\dots,\tau_m\},\downarrow,\otimes\} $$is a VN where $$\tau_i$$ is a valuation for $$t_i$$, and suppose $$\downarrow$$ and $$\otimes$$ satisfy Axioms A1-A3 [1] . Let $$ \Psi $$ denote $$t_1~\cup\dots\cup ~t_m$$. Suppose $$t \subseteq \Psi$$, and suppose $$X_1X_2\dots~X_n$$ is a sequence of variables in $$\Psi - t$$. Then</span></em></span>

<span style="font-family: 'comic sans ms', sans-serif; font-size: 14px;"><em>$$( \tau_1 \otimes \dots \otimes \tau_m)^{\downarrow~t} =~\large\otimes \bigg\{Fus_{X_n} \Big\{ \dots Fus_{X_2} \Big\{ Fus_{X_1} \{ \tau_{1} ,\dots, \tau_{m}\}~\Big\}~\Big\}~\bigg\}$$ </em></span>

With this fusion Algorithm, we can compute the marginal of the joint valuation one by one. It is obviously inefficient that it involves much repetition of effort. Then they use a join tree with message passing to mimic the fusion algorithm.

A join tree is a tree whose nodes are subsets of $$\Psi$$ such that if a variable is in two distinct nodes, then it is in every node on the path between the two nodes. In [1], a procedure is given out. I follow the algorithm and implement it with Python. The input $$\Psi$$ and $$\Phi$$ are given by the Chest clinic example in [1]. It is worth to noting that the deletion sequence is $$XASDBLE$$ which is derived by the one-step lookahead heuristic approach. Below is the source code.


```python
import JoinTree as jt
variables = set({'A','T','E','X','D','L','S','B'})

valuations = []
valuations.append(jt.Valuation(frozenset({'A'})))
valuations.append(jt.Valuation(frozenset({'A'}),observed=True))
valuations.append(jt.Valuation(frozenset({'S'})))
valuations.append(jt.Valuation(frozenset({'A','T'})))
valuations.append(jt.Valuation(frozenset({'S','L'})))
valuations.append(jt.Valuation(frozenset({'S','B'})))
valuations.append(jt.Valuation(frozenset({'T','L','E'})))
valuations.append(jt.Valuation(frozenset({'E','X'})))
valuations.append(jt.Valuation(frozenset({'E','B','D'})))
valuations.append(jt.Valuation(frozenset({'D'}),observed=True))

subsets = set()
for i in valuations:
    subsets.add(i.variables)

def get_superset(superset,t):
    for i in superset:
        if t in i:
            yield i

def construct( variables, subsets,object):
    nodes = set()
    edges = set()
    variables.difference_update(object)

    heuristic_count = ['E','L','B','D','S','A','X']

    while(len(subsets)&gt;1):

        picked=heuristic_count.pop()
        s = reduce(frozenset .union,get_superset(subsets,picked))

        s_all = set([i for i in get_superset(subsets,picked)])

        #update nodes
        nodes.update(s_all)

        s_set = set()
        s_set.add(s)
        nodes.update(s_set)

        temp = set()
        temp.add(picked)

        _s=set(s)
        s_no_y = set()       #{s-{Y}}
        s_no_y.add(frozenset(_s.difference(temp)))
        nodes.update(s_no_y)

        #update edge

        u_s = subsets.difference(s_set)
        temp = set([ (i,s) for i in get_superset(u_s,picked) ])
        single = s.difference(set(picked))

        edges.update(temp)
        edges.update(set([(s,single)]))

        variables.difference_update(set(picked))
        subsets.difference_update(s_all)
        subsets.update(s_no_y)

    nodes.update(subsets)
    return nodes,edges

nodes, edges=construct(variables,subsets,'T')
```

By using the visualization tool GraphViz, you can get the graph:


<div style="text-align:center"><img src ="http://n-moving.com/wp-content/uploads/2015/09/Digraph.gv_1.jpg" /></div>


**You can also download the source code from [here](http://pan.baidu.com/s/1pJN3s8N)**

[1] P. P. Shenoy, “Binary join trees for computing marginals in the Shenoy-Shafer architecture,” Int. J. Approx. Reason., vol. 17, no. 2–3, pp. 239–263, Aug. 1997.
