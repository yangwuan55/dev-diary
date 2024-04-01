kotlin flow是一个kotlin官方的响应式框架。
在初步接触这个框架时可能碰到三个函数：flatmapConcat flatmapMerged flatmapLatest.
如果使用不当会造成非常奇怪的问题。


- flatmapConcat：这个操作符会将 Flow 中的每个元素转换为一个新的 Flow，并且会按照元素的顺序依次连接这些 Flow。这意味着，直到前一个 Flow 完全发射完所有元素后，才会开始处理下一个 Flow。  
- flatmapMerged：这个操作符也会将 Flow 中的每个元素转换为一个新的 Flow，但是它会并发地发射这些 Flow 的元素。这意味着，所有的 Flow 都会同时开始发射元素，而不是等待前一个 Flow 完全发射完所有元素。  
- flatmapLatest：这个操作符与 flatmapMerged 类似，也会并发地发射 Flow 的元素，但是它只会收集最新的 Flow 的元素。这意味着，如果在一个 Flow 还没有发射完所有元素的情况下，又有一个新的 Flow 开始发射元素，那么旧的 Flow 就会被取消，只有最新的 Flow 会被收集


经验之谈：
1. flatmapConcat flatmapMerged中尽量返回冷流，这两种操作符通常是为了处理每个元素交给一个新的流进行处理，如果返回热流，可能会导致多个订阅者订阅同一个热流，这样会导致数据错乱。
2. flatmapLatest中尽量返回热流。flatmapLatest会自动取消旧的流，只保留最新的流，这个特性天生的可以处理热流，在某些场景下我们需要把数据的变化转换为某个热流的变化，这时候就可以使用flatmapLatest。
