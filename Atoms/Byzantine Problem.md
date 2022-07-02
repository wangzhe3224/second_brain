---
aliases: [拜占庭问题,拜占庭将军问题]
---

讨论的是在少数节点有可能作恶（消息可能被伪造）的场景下，如何达成共识问题。拜占庭容错（[[Byzantine fault tolerance]]）讨论的是容忍拜占庭错误的共识算法。

拜占庭问题之前，早在 1975 年，学术界就已经开始两将军问题的讨论（《Some constraints and tradeofis in the design of network communications》）：两个将军要通过信使来达成进攻还是撤退的约定，但信使可能迷路或被敌军阻拦（消息丢失或伪造），如何达成一致？这是典型的异步双方共识模型，根据 FLP 不可能原理，这个问题不存在通用解。

