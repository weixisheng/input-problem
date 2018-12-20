### Vue使用总结

- 全局注册组件
```javascript
  import upperFirst from 'lodash/upperFirst'
  import camelCase from 'lodash/camelCase'
  // require.context 第二个参数是否查询其子目录
  const requireCom = require.context('./components', false, /\w+\.(vue)$/)
  requireCom.keys().forEach(fileName => {
    const comConfig = requireCom(fileName)
    const comName = upperFirst(
      camelCase(
        // 剥去文件名开头的 `./` 和结尾的扩展名
        fileName.replace(/^\.\/(.*)\.\w+$/, '$1')
      )
    )
    Vue.component(comName, comConfig.default || comConfig)
  })
```
- 生成iconfont样式类名 
```javascript
  // node generateCSS.js
  let fs = require('fs')
  let path = require('path')
  let rootPath = path.resolve(__dirname, './src/assets/iconfont/iconfont.css')

  fs.readFile(`${rootPath}`, 'utf-8', (err, data) => {
    if (err) throw err
    let d = data.match(/(icon-[^.]+)(?=:\w+)/gm)
    let res = JSON.stringify({'icon': d}, null, 4)
    fs.writeFile(
      path.resolve(__dirname, './src/assets/iconfont/iconfont.json'),
      res,
      err => {
        if (err) throw err
        console.log('成功')
      }
    )
  })
```
- 基于elementUI封装一个table&pagination组件
```vue
<template>
  <div>
    <el-table :data="tableData" border :size="size" v-loading="tableLoading" v-bind="$attrs">
      <template v-for="(column, index) in columns">
        <!-- <slot name="front-slot"></slot> -->
        <!-- 复选框 -->
        <!-- <el-table-column :key="index" v-if="column.type === 'selection'" type="selection" width="55"></el-table-column> -->
        <!-- 序号 -->
        <!-- <el-table-column :key="index" v-else-if="column.type === 'index'" type="index" width="50" label="序号"></el-table-column> -->
        <!-- 展开列 -->
        <!-- <el-table-column :key="index" v-else-if="column.type === 'expand'" type="expand"> -->
        <!-- 具体内容 -->
        <el-table-column :key="index" :align="column.align||'center'" :label="column.title" :show-overflow-tooltip="column.tooltip" :min-width="column.minWidth" :width="column.width">
          <template slot-scope="scope">
            <!-- 仅仅显示文字 -->
            <template v-if="!column.hidden">
              <!-- 如果hidden为true的时候 那么当前格可以不显示，可以选择显示自定义的slot-->
              <!-- 操作按钮 -->
              <template v-if="column.type === 'operate'">
                <el-button v-for="(operate, index) in column.operates" :key="index" @click="handleClick(operate, scope.$index, scope.row)" :type="operate.type" :size="operate.size||'mini'" plain>{{operate.name}}</el-button>
              </template>
              <span v-else>
                {{scope.row[column.key]}}
              </span>
            </template>
            <!-- 使用slot的情况下 -->
            <template v-if="column.slot">
              <slot :name="column.slot" :scope="scope"></slot>
            </template>
          </template>
        </el-table-column>
      </template>
      <!--默认的slot -->
      <slot />
    </el-table>
    <el-pagination v-if="showPagination && tableData.length>0" @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="page" :page-sizes="pageSizes" :page-size="pageSize" background :layout="layout" :total="totalCount">
    </el-pagination>
  </div>
</template>
<script>
export default {
  name: 'MyTablePage',
  props: {
    tableLoading: {
      type: Boolean,
      default: false
    },
    // 核心数据
    tableData: {
      type: Array,
      default: () => []
    },
    // columns
    columns: {
      type: Array,
      required: true,
      default: () => []
    },
    totalCount: {
      type: Number,
      default: 0
    },
    size: {
      type: String,
      default: 'medium'
    },
    showPagination: {
      type: Boolean,
      default: true
    },
    layout: {
      type: String,
      default: 'total, sizes,prev, pager, next, jumper'
    },
    page: {
      type: Number,
      default: 1
    },
    pageSize: {
      type: Number,
      default: 10
    },
    pageSizes: {
      type: Array,
      default: () => [10, 20, 30, 50]
    }
  },
  data () {
    return {
      pagination: {}
    }
  },
  methods: {
    handleCurrentChange (_val) {
      this.pagination.page = _val
      this.$emit('update:page', _val)
      this.fetchData()
    },
    handleSizeChange (_val) {
      this.pagination.pageSize = _val
      this.$emit('update:pageSize', _val)
      this.handleCurrentChange(1)
    },
    fetchData () {
      this.$emit('getData')
    },
    // 处理点击事件
    handleClick (action, index, data) {
      // emit事件
      this.$emit(`${action.emitKey}`, index, data)
    }
  }
}
</script>
```
```vue
<my-table-page @edit="handleEdit"
      @del="handleDelete"
      :table-loading="tableLoading"
      :columns="headers"
      :table-data="tableData"
      :total-count="totalCount"
      :page.sync="searchForm.page"
      :page-size.sync="searchForm.pagesize"
      @getData="getTableData"
      ref="table">
      <template slot="timeSlot" slot-scope="{scope}">
        <span>{{ scope.row.addTime}}</span>
      </template>
 </my-table-page>
 ...
 headers: [
  {
    key: 'name',
    title: '名称'
  },
  {
    slot: 'timeSlot',
    title: '时间'
  },
  {
    title: '操作',
    type: 'operate',
    width: '180',
    operates: [
      {
        name: '编辑',
        emitKey: 'edit',
        type: 'primary'
      },
      {
        name: '删除',
        emitKey: 'del',
        type: 'danger'
      }
    ]
  }
 ]
```
