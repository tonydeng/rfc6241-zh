# 6.3 子树过滤处理

过滤器输出（所选节点集合）最初是空的。

每个子树过滤器可以包含一个或多个数据模型片段，这些片段代表将在过滤器输出中选择的数据模型的部分（包含所有子节点）。

服务器将每个子树数据片段与服务器支持的内部数据模型进行比较。如果整个子树数据片段过滤器（从过滤器中指定的根到最内层的元素开始）与受支持的数据模型的相应部分完全匹配，那么该节点及其所有子节点将包含在结果数据中。

服务器将所有具有相同父节点（同级组）的节点一起处理，从根节点开始到叶节点。过滤器中的根元素被认为是在相同的同级集合中（假设它们在相同的名称空间中），即使它们没有共同的父级。

对于每个兄弟集合，服务器确定过滤器输出中包含（或可能包括）哪些节点，以及从过滤器输出中排除（修剪）哪些兄弟子树。服务器首先确定兄弟集合中存在哪些类型的节点，并根据其类型的规则处理节点。如果选择了兄弟集合中的任何节点，则该过程递归地应用于每个所选节点的兄弟集合。该算法继续，直到在过滤器中指定的所有子树中的所有兄弟集均已被处理。
