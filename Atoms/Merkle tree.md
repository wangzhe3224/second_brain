---
aliases: [默克尔树, Hash Tree]
---

Merkle Tree.

是一种典型的二叉树结构，由一个根节点、一组中间节点和一组叶节点组成。默克尔树最早由 Merkle Ralf 在 1980 年提出，曾广泛用于文件系统和 P2P 系统中。

应用：

- 通过构建集合的默克尔树，并提供该元素各级兄弟节点中的 Hash 值，可以不暴露集合完整内容而证明某元素存在。
- 快速比较数据
- 快速定位修改
- 零知识证明
