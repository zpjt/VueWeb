# VueWeb
开始玩玩vue,当然用TS
# 每次发请求获取菜单数据的后，并且渲染到dom上后，设置每个子菜单容器dom的高度，怎么做？
1、 在update生命周期里要识别是通过api改变的数据，
解决方式： 
    1用 watch 观察数据变动，注意与react 区别，不用返回新数据直接在节点上改动，也就是 .语法， 只在重新获取数据的时候赋值。
    2通过vue nextTick方法（可以在任何地方调用） ，也就是在dom全挂载后执行dom操作。不然dom没有挂载获取不到元素
