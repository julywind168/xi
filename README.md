# xi
Yet another game server framework

## 思路

模仿 Nakama 中的 Match 的封装，每个 Match 即一个 Service(Actor). 重要是如何解决 User 数据竞争的问题。

登陆的 User 由框架管理。每个 Match 包含一个 Group (*User数组)。每次 Match 的 Handler 执行，必须拿到 Group 中所有玩家的所有权，执行完再返回给框架

这样设计之后会有个问题，如果有个 Match 引用了所有 User, 每次加锁执行则相当不合理。这样可以引入一个 Lock-Free-Match, 这种 match 对 User 是只读的并且不关心它的版本。