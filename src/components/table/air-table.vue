<template>
    <!-- 表格组件开始，air-table-container容器overflow:auto产生滚动条 -->
    <div class="air-table" ref="wrapper">

        <div class="air-table-empty" v-if="!dataSource.length">
            <p>没有符合条件的记录！</p>
        </div>

        <div class="air-table-container" ref="container">

            <table ref="table">
                <thead>
                    <tr>
                        <th v-if="checkable" :class="{ 'fix-left-cell': fixLeft }">
                            <div :class="{ 'shadow-left': fixLeft && shadowLeft }">
                                <input type="checkbox" v-model="select.selectAll" title="全选/全清">
                            </div>
                        </th>

                        <th v-for="column in validColumns" :key="column.name" :class="column.cellClass"
                            :colspan="column.colspan">

                            <div :class="{ 'shadow-left': column.cellClass && shadowLeft }">
                                <span class="title cursor-hand" v-if="column.colspan <= 1"
                                    @click="changeOrderBy(column.name)">{{
                                        column.title }}</span>
                                <span class="title" v-else>{{ column.title }}</span>
                                <span class="sort" v-if="column.colspan <= 1">
                                    <i class="asc cursor-hand" @click="changeOrderBy(column.name, 'asc')"
                                        :class="{ active: orderBy.columnName == column.name && orderBy.direction === 'asc' }">▲</i>
                                    <i class="desc cursor-hand" @click="changeOrderBy(column.name, 'desc')"
                                        :class="{ active: orderBy.columnName == column.name && orderBy.direction === 'desc' }">▼</i>
                                </span>
                            </div>
                        </th>

                        <th :class="{ 'fix-right-cell': fixRight }">
                            <div :class="{ 'shadow-right': fixRight && shadowRight }">
                                操作
                            </div>
                        </th>
                    </tr>
                </thead>

                <tbody>

                    <tr v-if="topSum && dataSource.length" class="sum">
                        <td v-if="checkable">
                            <div></div>
                        </td>

                        <td v-for="column in validColumns" :key="column.name">
                            <div>
                                {{ sumRow[column.name] }}
                            </div>
                        </td>

                        <td>
                            <div></div>
                        </td>
                    </tr>

                    <tr v-for="row in dataSource" :key="row[keyName]">

                        <td v-if="checkable" :class="{ 'fix-left-cell': fixLeft }">
                            <div :class="{ 'shadow-left': fixLeft && shadowLeft }">
                                <input type="checkbox" :value="row" v-model="select.selectedRows">
                            </div>
                        </td>

                        <td v-for="column in validColumns" :key="column.name" :class="column.cellClass">
                            <div
                                :class="{ 'shadow-left': column.cellClass && shadowLeft, 'align-right': column.dataType == 'Number' }">
                                {{ row[column.name] }}
                            </div>
                        </td>

                        <td :class="{ 'fix-right-cell': fixRight }">
                            <div :class="{ 'shadow-right': fixRight && shadowRight }">
                                <slot name="rowButtons" :row="row"></slot>
                            </div>
                        </td>
                    </tr>
                </tbody>

                <tfoot v-if="bottomSum && dataSource.length">
                    <tr class="sum">
                        <td v-if="checkable">
                            <div></div>
                        </td>

                        <td v-for="column in validColumns" :key="column.name">
                            <div>
                                {{ sumRow[column.name] }}
                            </div>
                        </td>

                        <td>
                            <div></div>
                        </td>
                    </tr>
                </tfoot>

            </table>

        </div>



    </div>
    <!-- 表格组件结束 -->
</template>

<script setup>

import { ref, reactive, computed, watch, onMounted, onUpdated, onBeforeUnmount } from 'vue'
import $ from 'jquery'


//绑定DOM元素
const container = ref(null), table = ref(null)

// 左右冻结列阴影样式
const shadowLeft = ref(false), shadowRight = ref(false)

// 定义组件的属性
const props = defineProps({
    name: String,
    columns: {
        type: Array,
        required: true,
        validator(array) {
            let success = array.filter(item => item.key).length > 0
            if (!success) {
                console.log("Columns中至少需有一列 key:true, 代表该列为主键!")
            }
            return success
        }
    },
    dataSource: {
        type: Array,
        required: true,
    },
    initSelectedRows: {
        type: Array,
        default: []
    },
    orderBy: Object,
    fixTop: {
        type: Boolean,
        default: false
    },
    fixLeft: {
        type: Number,
        default: 0
    },
    fixRight: {
        type: Boolean,
        default: false
    },
    checkable: {
        type: Boolean,
        default: false
    },
    topSum: {
        type: Boolean,
        default: false
    },
    bottomSum: {
        type: Boolean,
        default: false
    },
})




// 获取主键名
const keyName = computed(() => {
    let kname = "id"
    for (let i = 0, len = props.columns.length; i < len; i++) {
        if (props.columns[i].key) {
            kname = props.columns[i].name
            break
        }
    }
    return kname
})

// 显示列
const validColumns = computed(() => {
    let vColumns = []
    let cIndex = 0
    if (props.checkable) cIndex = 1
    props.columns.forEach(col => {
        if (col.show) {
            if (cIndex < props.fixLeft) col.cellClass = "fix-left-cell"
            vColumns.push(col)
            cIndex += 1
        }
    })
    return vColumns
})

// 生成合计行
const sumRow = computed(() => {
    let sRow = {}, columns = props.columns, dataSource = props.dataSource
    for (let i = 0, clen = columns.length; i < clen; i++) {
        if (!columns[i]["sum"]) {
            sRow[columns[i]["name"]] = ""
        } else {
            let sum = 0
            for (let j = 0, rlen = dataSource.length; j < rlen; j++) {
                sum += dataSource[j][columns[i]["name"]] * 1000
            }
            sRow[columns[i]["name"]] = sum / 1000
        }
    }
    return sRow
})



// 左右侧冻结列添加阴影 Vue性能太差
let addShadow1 = () => {
    let scrollLeft = container.value.scrollLeft, scrollTop = container.value.scrollTop
    let containerWidth = container.value.offsetWidth, tableWidth = table.value.offsetWidth
    shadowLeft.value = !!scrollLeft
    shadowRight.value = (scrollLeft + containerWidth + 2 < tableWidth)
}

//左右侧冻结列添加阴影 jQuery性能好
// TODO:用js自己实现，以避免导入jQuery
let addShadow2 = function () {
    if ($(".air-table-container").scrollLeft() == 0) {
        $("table tr .fix-left-cell > div.shadow-left").removeClass("shadow-left")
    } else {
        $("table tr .fix-left-cell > div").addClass("shadow-left")
    }

    if ($(".air-table-container").scrollLeft() + $(".air-table-container").width() + 2 >= $(".air-table table").width()) {
        $(".fix-right-cell > div.shadow-right").removeClass("shadow-right")
    } else {
        $(".fix-right-cell > div").addClass("shadow-right")
    }
}

// 更新左侧冻结列的left定位 jQuery性能好
// TODO:用js自己实现position()，以避免导入jQuery
let updateFixLeft = () => {
    $(".air-table .fix-left-cell").each(function () {
        $(this).css("left", "auto").css("left", $(this).position().left + "px")
    })
}

// 如果要实现多行列头，需在列头行数变化时，重新付top值 jQuery性能好
// TODO:用js自己实现，以避免导入jQuery
// let updateFixTop = () => {
//     $(".air_table thead tr").each(function () {
//         $(this).css("top", "auto").css("top", $(this).position().top + "px");
//     });
// }


onMounted(() => {
    container.value.addEventListener('scroll', addShadow2)
    addShadow2()
    updateFixLeft()
})

let isDataSourceChanged = false
onUpdated(() => {
    //数据源发生变化时（增加、删除行）
    if (isDataSourceChanged) {
        addShadow2()
        updateFixLeft()
        isDataSourceChanged = false
    }
})


onBeforeUnmount(() => {
    container.value.removeEventListener('scroll', addShadow2)
})

// 定义触发事件
const emit = defineEmits(['selectChanged', 'orderByChanged'])

// 选择行   TODO:删除行时有BUG，需改进
let flag = true
const select = reactive({
    selectAll: false, // (props.initSelectedRows.length == props.dataSource.length && props.dataSource.length != 0),
    selectedRows: [] //props.initSelectedRows
})

watch(() => select.selectAll, (newValue, oldValue) => {
    if (newValue) {
        if (select.selectedRows.length != props.dataSource.length) {
            select.selectedRows = props.dataSource
        }
    } else {
        if (select.selectedRows.length) {
            select.selectedRows = []
        }
    }
})

watch(() => select.selectedRows, (newValue, oldValue) => {
    if (newValue.length == props.dataSource.length && props.dataSource.length != 0) {
        select.selectAll = true
        flag = true
    } else {
        select.selectAll = false
        flag = false
    }
    // 触发父组件selectChanged事件，向父组件传递选中的数据行
    emit('selectChanged', newValue)
})

// 监听数据源变化
watch(() => props.dataSource.length, (newValue, oldValue) => {
    isDataSourceChanged = true
})

// 暴露方法给父组件，返回当前选中的数据行。父组件通过ref引用.value.getSelectedRows()调用
const getSelectedRows = () => {
    return select.selectedRows
}
defineExpose({ getSelectedRows })

// 排序
const changeOrderBy = (columnName, direction) => {
    // console.log(columnName, direction)
    if (!direction) {
        // 点击标题切换
        if (columnName == props.orderBy.columnName) {
            if (props.orderBy.direction == "asc") {
                props.orderBy.direction = "desc"
            } else if (props.orderBy.direction == "desc") {
                props.orderBy.columnName = ""
                props.orderBy.direction = ""
            } else {
                props.orderBy.direction = "asc"
            }
        } else {
            props.orderBy.columnName = columnName
            props.orderBy.direction = "asc"
        }
    } else {
        // 点击箭头节换
        props.orderBy.columnName = columnName
        props.orderBy.direction = direction
    }

    // 触发父组件orderByChanged事件，向父组件传递orderBy
    emit('orderByChanged', props.orderBy)
}






</script>


<style src="./air-table.css"></style>
