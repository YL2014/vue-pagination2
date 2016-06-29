# vue-pagination2

##基于vue的一个分页组件，参考github上的一个项目做得修改，不算原创，方便结合vuex使用

####参考项目地址：[https://github.com/cycgit/vue-pagination](https://github.com/cycgit/vue-pagination)

> 说明：
> ```bash
total：总的记录数
pageSize: 每页显示条数
pageIndex: 当前页码
all: 总页数
```

在参考项目里，`all`是直接传的，我在这里根据自己的业务通过`total`进行的计算，当然也可以直接传(传的话注意去掉我的那坨计算代码...)

###使用示例
```
<template>
        <pagination :pageIndex.sync="1" :total.sync="total" @gopage="listen"></pagination>
</template>

<script>
import Pagination from './Pagination.vue'
export default {
    data () {
        return {
            total: 1
        }
    },
    components: {
        Pagination
    },
    methods: {
        listen (index) {
            console.log(index)
        }
    },

}
</script>

<style lang="sass">
</style>
```