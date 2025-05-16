```vue
<el-table
    :data="tableData"
    :header-cell-style="{'text-align':'center'}"
    :cell-style="{'text-align':'center'}"
    style="width: 100%">
</el-table>
```

> 单个表格的内容居中：只需要在[el-table-column](https://so.csdn.net/so/search?q=el-table-column&spm=1001.2101.3001.7020)上加上align=‘center’

```javascript
<el-table-column label="姓名" prop="realname" align="center">
</el-table-column>
```