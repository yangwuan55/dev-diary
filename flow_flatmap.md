kotlin flow是一个kotlin官方的响应式框架。
在初步接触这个框架时可能碰到三个函数：flatmapConcat flatmapMerged flatmapLatest.
如果使用不当会造成非常奇怪的问题。

在举例子之前我需要接上两个响应式的概念叫做：热流和冷流。
热流：永远不会停止，需要使用者去管理生命周期。
冷流：类似一个方法的执行，执行完毕生命周期自动结束。

一个比较极端的情况，一个flow中包含了另一个flow。我们的诉求可能是要将flow展开。

```

flowof(flowof(0,1,2),flowof(3,4,5))

```

这时候我们可能会这么做：

```

flowof(flowof(0,1,2),flowof(3,4,5))
  .flatmapConcat {
    it
  }
...
```

这个例子中flatmapConcat flatmapMerged flatmapLatest没有区别，但是如果我们把flowof换成一个复杂点的flow

```

flow {
  var count = 0
  while(true) {
    delay(1000)
    emit(count ++)
  }
}

```


