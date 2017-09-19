最近俩月实在是忙到死。

## 出现需求

最近在做智能钢琴后台管理系统，遇到了这么个需求：

> 需求：有这么个总览页面，页面中是个表格，要求能够在本页面中编辑表格中的一条数据。

转换成对前端的需求，就是：

*在一个总览页面，有一个table grid，grid中每一条数据后都有一个编辑按钮，点击之后打开一个本页面中的弹窗，弹窗中是一个表单，表单会被渲染上数据，可修改，修改之后的信息可以提交给指定的api。*

## 解决思路

因为使用 vue 2.0 框架，我的elementSpa （逃），所以应该按照 vue 的思路来设计工程。中间的许多组件用elementui来实现了。

首先，在这个页面中，我需要一个表格来展示数据，一个嵌套form的表单来对数据进行编辑。所以，我的页面结构应该是这样的（代码简写，后同）：

```
    <template>
        <!-- table部分，data是数据，columns是列信息 -->
        <table
        data="gridOption.data"
        column="gridOption.columns"
        ></table>
        <!-- 实现的弹出窗口，可以自己写，这里用的el-dialog组件 -->
        <dialog>
            <!-- form绑定 -->
            <form v-model="form">
            </form>
        </dialog>
    <template>
    <script>
        export default{
            data(){
                return {
                    gridOption:{
                        data: [],
                        columns: [{
                            key: '',
                            label: ''
                        }]
                    },
                    form: {}
                }
            }
        }
    </script>
```
这样子写，手动配置一下columns和form需求的数据类型，获取下api数据写入到data就可以了。

但是，每做一个页面就得把前面的东西复制一遍。所以接下来就需要把table和dialog封装成组件。







