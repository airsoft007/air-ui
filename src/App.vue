<template>
    <div>
        <div class="panel">
            <h2>冻结表格组件</h2>
            <p>
                本例程演示如何采用单table标签实现表格顶部标题行、左侧列、右侧列的冻结。因为element ui/element plus的
                表格组件使用了多table实现冻结表格，拖动滚动条时需要控制多table的联动，当数据量略大时严重卡顿，性能极差。
                采用单table实现的表格，由原生div容器产生滚动条，不存在多表联动问题，性能高。
            </p>
        </div>

        <div class="main">
            <AirTable ref="airTable" :columns="columns" :dataSource="dataSource" :fixTop="true" :fixLeft="3"
                :fixRight="true" :checkable="true" :topSum="true" :bottomSum="true" :initSelectedRows="selectedRows"
                :orderBy="orderBy" @selectChanged="selectChanged" @orderByChanged="orderByChanged">
                <template #rowButtons="{ row }">
                    <button type="button" title="查看" @click="view(row)">查看</button>
                    <button type="button" title="修改" @click="edit(row)">修改</button>
                    <button type="button" title="审核" @click="check(row)">审核</button>
                    <button type="button" title="删除" @click="del(row)">删除</button>

                    <!-- <a href="#" title="查看" @click="view(row)">查看</a>
                    <a href="#" title="修改" @click="edit(row)">修改</a>
                    <a href="#" title="审核" @click="check(row)">审核</a>
                    <a href="#" title="删除" @click="del(row)">删除</a> -->
                </template>
            </AirTable>
        </div>

        <div class="panel">
            <button type="button" @click="clearData">清空数据</button>

            <button type="button" @click="addData">添加数据</button> 数据：{{ dataSource.length }} 行

            <button type="button" @click="addHeader">添加列头</button>

            <button type="button" @click="getSelected">获取选中行的2个方法</button>

            源码下载 for Vue3：<a href="./air-ui.rar">冻结表格示例</a>
        </div>
    </div>


</template>

<script setup>
import { ref, reactive } from 'vue'
import AirTable from './components/table/air-table.vue'

// 组件对象引用
const airTable = ref(null)

// 列
const columns = [{
    name: "id",
    title: "主键，不显示",
    dataType: "String",
    show: false,
    colspan: 1,
    key: true
}, {
    name: "fieldName1",
    title: "测试列名1",
    dataType: "String",
    show: true,
    colspan: 1
}, {
    name: "fieldName2",
    title: "测试列名2",
    dataType: "String",
    show: true,
    colspan: 1
}, {
    name: "fieldName3",
    title: "测试列名3",
    dataType: "String",
    show: true,
}, {
    name: "fieldName4",
    title: "测试列名4",
    dataType: "Number",
    show: true,
    sum: true
}, {
    name: "fieldName5",
    title: "测试列名名字长一点5",
    dataType: "String",
    show: true,
    colspan: 1
}, {
    name: "fieldName6",
    title: "这一列应该不显示6，前面的列必须是colspan为1",
    dataType: "String",
    show: false,
    colspan: 1
}, {
    name: "fieldName7",
    title: "测试列名7",
    dataType: "Number",
    show: true,
    colspan: 1,
    sum: true
}, {
    name: "fieldName8",
    title: "测试列名长长长长长长长长亿点8",
    dataType: "String",
    show: true,
    colspan: 1
}, {
    name: "fieldName9",
    title: "测试列名9",
    dataType: "Number",
    show: true,
    colspan: 1,
    sum: true
}, {
    name: "fieldName10",
    title: "测试列名10",
    dataType: "String",
    show: true,
    colspan: 1
}, {
    name: "fieldName11",
    title: "测试列名11",
    dataType: "String",
    show: true,
    colspan: 1
}, {
    name: "fieldName12",
    title: "测试列名12",
    dataType: "String",
    show: true,
    colspan: 1
}, {
    name: "fieldName13",
    title: "测试列名13",
    dataType: "String",
    show: true,
    colspan: 1
}, {
    name: "fieldName14",
    title: "测试列名14",
    dataType: "String",
    show: true,
    colspan: 1
}, {
    name: "fieldName15",
    title: "测试列名15",
    dataType: "String",
    show: true,
    colspan: 1
}, {
    name: "fieldName16",
    title: "测试列名16",
    dataType: "String",
    show: true,
    colspan: 1
}, {
    name: "fieldName17",
    title: "测试列名17",
    dataType: "String",
    show: true,
    colspan: 1
}, {
    name: "fieldName18",
    title: "测试列名18",
    dataType: "String",
    show: true,
    colspan: 1
}, {
    name: "fieldName19",
    title: "测试列名19",
    dataType: "String",
    show: true,
    colspan: 1
}, {
    name: "fieldName20",
    title: "测试列名20",
    dataType: "String",
    show: true,
    colspan: 1
}]

// 数据
const dataSource = reactive([{
    id: "123456",
    fieldName1: "软烂鲜香的红酒炖牛肉软烂鲜香的红酒炖牛肉abcdef123456",
    fieldName2: "是",
    fieldName3: "已审核",
    fieldName4: 123456.78,
    fieldName5: "2019-07-11 03:32:59",
    fieldName6: "2019-07-29 16:49:31",
    fieldName7: 56789123.12,
    fieldName8: "来央视频《巅峰食刻》一口到达勃艮第",
    fieldName9: 987654,
    fieldName10: "测试数据10",
    fieldName11: "测试数据11",
    fieldName12: "测试数据12",
    fieldName13: "测试数据13",
    fieldName14: "测试数据14",
    fieldName15: "测试数据15",
    fieldName16: "测试数据16",
    fieldName17: "测试数据17",
    fieldName18: "测试数据18",
    fieldName19: "测试数据19",
    fieldName20: "测试数据20",
}, {
    id: "234567",
    fieldName1: "软烂鲜香的红酒炖牛肉软烂鲜香的红酒炖牛肉abcdef123456",
    fieldName2: "是",
    fieldName3: "已审核",
    fieldName4: 123456.78,
    fieldName5: "2019-07-11 03:32:59",
    fieldName6: "2019-07-29 16:49:31",
    fieldName7: 56789123.12,
    fieldName8: "来央视频《巅峰食刻》一口到达勃艮第",
    fieldName9: 987654,
    fieldName10: "测试数据10",
    fieldName11: "测试数据11",
    fieldName12: "测试数据12",
    fieldName13: "测试数据13",
    fieldName14: "测试数据14",
    fieldName15: "测试数据15",
    fieldName16: "测试数据16",
    fieldName17: "测试数据17",
    fieldName18: "测试数据18",
    fieldName19: "测试数据19",
    fieldName20: "测试数据20",
}, {
    id: "345678",
    fieldName1: "软烂鲜香的红酒炖牛肉软烂鲜香的红酒炖牛肉abcdef123456",
    fieldName2: "是",
    fieldName3: "已审核",
    fieldName4: 123456.78,
    fieldName5: "2019-07-11 03:32:59",
    fieldName6: "2019-07-29 16:49:31",
    fieldName7: 56789123.12,
    fieldName8: "来央视频《巅峰食刻》一口到达勃艮第",
    fieldName9: 987654,
    fieldName10: "测试数据10",
    fieldName11: "测试数据11",
    fieldName12: "测试数据12",
    fieldName13: "测试数据13",
    fieldName14: "测试数据14",
    fieldName15: "测试数据15",
    fieldName16: "测试数据16",
    fieldName17: "测试数据17",
    fieldName18: "测试数据18",
    fieldName19: "测试数据19",
    fieldName20: "测试数据20",
}, {
    id: "456788",
    fieldName1: "软烂鲜香的红酒炖牛肉软烂鲜香的红酒炖牛肉abcdef123456",
    fieldName2: "是",
    fieldName3: "已审核",
    fieldName4: 123456.78,
    fieldName5: "2019-07-11 03:32:59",
    fieldName6: "2019-07-29 16:49:31",
    fieldName7: 56789123.12,
    fieldName8: "来央视频《巅峰食刻》一口到达勃艮第",
    fieldName9: 987654,
    fieldName10: "测试数据10",
    fieldName11: "测试数据11",
    fieldName12: "测试数据12",
    fieldName13: "测试数据13",
    fieldName14: "测试数据14",
    fieldName15: "测试数据15",
    fieldName16: "测试数据16",
    fieldName17: "测试数据17",
    fieldName18: "测试数据18",
    fieldName19: "测试数据19",
    fieldName20: "测试数据20",
}, {
    id: "567567446",
    fieldName1: "软烂鲜香的红酒炖牛肉软烂鲜香的红酒炖牛肉",
    fieldName2: "是",
    fieldName3: "已审核",
    fieldName4: 123456.78,
    fieldName5: "2019-07-11 03:32:59",
    fieldName6: "2019-07-29 16:49:31",
    fieldName7: 56789123.12,
    fieldName8: "测试文字测试文字测试文字",
    fieldName9: 987654,
    fieldName10: "测试数据10",
    fieldName11: "测试数据11",
    fieldName12: "测试数据12",
    fieldName13: "测试数据13",
    fieldName14: "测试数据14",
    fieldName15: "测试数据15",
    fieldName16: "测试数据16",
    fieldName17: "测试数据17",
    fieldName18: "测试数据18",
    fieldName19: "测试数据19",
    fieldName20: "测试数据20",
}])

// 排序列（单列）

let orderBy = reactive({
    // columnName: "fieldName1",
    // direction: "desc"
})
const orderByChanged = (newOrderBy) => {
    orderBy = newOrderBy
    // do sth.
    console.log("排序：", orderBy)
}

// 获取选中行，存放在selectedRows中
let selectedRows = [{
    id: "123456",
    fieldName1: "软烂鲜香的红酒炖牛肉软烂鲜香的红酒炖牛肉abcdef123456",
    fieldName2: "是",
    fieldName3: "已审核",
    fieldName4: 123456.78,
    fieldName5: "2019-07-11 03:32:59",
    fieldName6: "2019-07-29 16:49:31",
    fieldName7: 56789123.12,
    fieldName8: "来央视频《巅峰食刻》一口到达勃艮第",
    fieldName9: 987654,
    fieldName10: "测试数据10",
    fieldName11: "测试数据11",
    fieldName12: "测试数据12",
    fieldName13: "测试数据13",
    fieldName14: "测试数据14",
    fieldName15: "测试数据15",
    fieldName16: "测试数据16",
    fieldName17: "测试数据17",
    fieldName18: "测试数据18",
    fieldName19: "测试数据19",
    fieldName20: "测试数据20",
}, {
    id: "234567",
    fieldName1: "软烂鲜香的红酒炖牛肉软烂鲜香的红酒炖牛肉abcdef123456",
    fieldName2: "是",
    fieldName3: "已审核",
    fieldName4: 123456.78,
    fieldName5: "2019-07-11 03:32:59",
    fieldName6: "2019-07-29 16:49:31",
    fieldName7: 56789123.12,
    fieldName8: "来央视频《巅峰食刻》一口到达勃艮第",
    fieldName9: 987654,
    fieldName10: "测试数据10",
    fieldName11: "测试数据11",
    fieldName12: "测试数据12",
    fieldName13: "测试数据13",
    fieldName14: "测试数据14",
    fieldName15: "测试数据15",
    fieldName16: "测试数据16",
    fieldName17: "测试数据17",
    fieldName18: "测试数据18",
    fieldName19: "测试数据19",
    fieldName20: "测试数据20",
}, {
    id: "345678",
    fieldName1: "软烂鲜香的红酒炖牛肉软烂鲜香的红酒炖牛肉abcdef123456",
    fieldName2: "是",
    fieldName3: "已审核",
    fieldName4: 123456.78,
    fieldName5: "2019-07-11 03:32:59",
    fieldName6: "2019-07-29 16:49:31",
    fieldName7: 56789123.12,
    fieldName8: "来央视频《巅峰食刻》一口到达勃艮第",
    fieldName9: 987654,
    fieldName10: "测试数据10",
    fieldName11: "测试数据11",
    fieldName12: "测试数据12",
    fieldName13: "测试数据13",
    fieldName14: "测试数据14",
    fieldName15: "测试数据15",
    fieldName16: "测试数据16",
    fieldName17: "测试数据17",
    fieldName18: "测试数据18",
    fieldName19: "测试数据19",
    fieldName20: "测试数据20",
}, {
    id: "456788",
    fieldName1: "软烂鲜香的红酒炖牛肉软烂鲜香的红酒炖牛肉abcdef123456",
    fieldName2: "是",
    fieldName3: "已审核",
    fieldName4: 123456.78,
    fieldName5: "2019-07-11 03:32:59",
    fieldName6: "2019-07-29 16:49:31",
    fieldName7: 56789123.12,
    fieldName8: "来央视频《巅峰食刻》一口到达勃艮第",
    fieldName9: 987654,
    fieldName10: "测试数据10",
    fieldName11: "测试数据11",
    fieldName12: "测试数据12",
    fieldName13: "测试数据13",
    fieldName14: "测试数据14",
    fieldName15: "测试数据15",
    fieldName16: "测试数据16",
    fieldName17: "测试数据17",
    fieldName18: "测试数据18",
    fieldName19: "测试数据19",
    fieldName20: "测试数据20",
}, {
    id: "567567446",
    fieldName1: "软烂鲜香的红酒炖牛肉软烂鲜香的红酒炖牛肉",
    fieldName2: "是",
    fieldName3: "已审核",
    fieldName4: 123456.78,
    fieldName5: "2019-07-11 03:32:59",
    fieldName6: "2019-07-29 16:49:31",
    fieldName7: 56789123.12,
    fieldName8: "测试文字测试文字测试文字",
    fieldName9: 987654,
    fieldName10: "测试数据10",
    fieldName11: "测试数据11",
    fieldName12: "测试数据12",
    fieldName13: "测试数据13",
    fieldName14: "测试数据14",
    fieldName15: "测试数据15",
    fieldName16: "测试数据16",
    fieldName17: "测试数据17",
    fieldName18: "测试数据18",
    fieldName19: "测试数据19",
    fieldName20: "测试数据20",
}]
const selectChanged = (rows) => {
    selectedRows = rows
    // console.log("选中：", selectedRows)
}

// 生成min到max之间的随机整数
let getRandomInt = (min, max) => {
    min = Math.ceil(min)
    max = Math.floor(max)
    return Math.floor(Math.random() * (max - min + 1)) + min
}



// --------------------------------------测试----------------------


//  添加数据
const addData = () => {
    let arr=[]
    for (let i = 0; i < 100; i++) {
        arr.push({
            id: getRandomInt(1, 100000000),
            fieldName1: "软烂鲜香的红酒炖牛肉软烂鲜香的红酒炖牛肉abcdef123456",
            fieldName2: "是",
            fieldName3: "已审核",
            fieldName4: 123456.78,
            fieldName5: "2019-07-11 03:32:59",
            fieldName6: "2019-07-29 16:49:31",
            fieldName7: 56789123.12,
            fieldName8: "来央视频《巅峰食刻》一口到达勃艮第",
            fieldName9: getRandomInt(1, 10000),
            fieldName10: "测试数据10",
            fieldName11: "测试数据11",
            fieldName12: "测试数据12",
            fieldName13: "测试数据13",
            fieldName14: "测试数据14",
            fieldName15: "测试数据15",
            fieldName16: "测试数据16",
            fieldName17: "测试数据17",
            fieldName18: "测试数据18",
            fieldName19: "测试数据19",
            fieldName20: "测试数据20",
        })
    }
    dataSource.push(...arr)
}

// 添加列头
const addHeader = () => {
    let arr=[]
    for (let i = 0; i < 100; i++) {
        arr.push({
            id: getRandomInt(1, 100000000),
            fieldName1: "软烂鲜香的红酒炖牛肉软烂鲜香的红酒炖牛肉abcdef123456",
            fieldName2: "是",
            fieldName3: "已审核",
            fieldName4: 123456.78,
            fieldName5: "2019-07-11 03:32:59",
            fieldName6: "2019-07-29 16:49:31",
            fieldName7: 56789123.12,
            fieldName8: "来央视频《巅峰食刻》一口到达勃艮第",
            fieldName9: getRandomInt(1, 10000),
            fieldName10: "测试数据10",
            fieldName11: "测试数据11",
            fieldName12: "测试数据12",
            fieldName13: "测试数据13",
            fieldName14: "测试数据14",
            fieldName15: "测试数据15",
            fieldName16: "测试数据16",
            fieldName17: "测试数据17",
            fieldName18: "测试数据18",
            fieldName19: "测试数据19",
            fieldName20: "测试数据20",
        })
    }
    dataSource.push(...arr)
}

// 清空数据
const clearData = () => {
    dataSource.splice(0, dataSource.length)
}

const getSelected = () => {
    // 方法一
    console.log(selectedRows)
    // 方法二
    console.log(airTable.value.getSelectedRows())
}

// 右侧按钮事件
const view = (row) => {
    console.log("view : ", row)
}

const edit = (row) => {
    console.log("edit : ", row)
}

const check = (row) => {
    console.log("check : ", row)
}

const del = (row) => {
    console.log("del : ", row)
}
</script>



<!-- <style src="./common.css"></style> -->

<style scoped>

.main {
    width: 80%;
    height: 500px;
    margin: 10px auto 0;
    overflow: hidden;
}

.panel {
    width: 80%;
    height: auto;
    margin: 10px auto 0;
    overflow: hidden;
}

.panel button {
    margin-right: 20px;
}

.air-table {
    width: 100%;
    height: 100%;
}
</style>
