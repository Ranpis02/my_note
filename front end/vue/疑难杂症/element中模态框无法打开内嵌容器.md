解决方案：在 el-dialog 中添加 `:append-to-body="true"` 即可

> 官方的解释为：该属性用来控制 Dialog 自身是否插入至 body 元素上。嵌套的 Dialog 必须指定该属性并赋值为 true



除此之外，`el-dialog` 还可能会出现的一个问题就是遮罩层的覆盖问题，可以考虑如下属性的配置

```html
:modal-append-to-body="false"
```

