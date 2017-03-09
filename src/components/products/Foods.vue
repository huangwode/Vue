<template>
    <div>
        <el-breadcrumb separator="/">
            <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
            <el-breadcrumb-item>商品管理</el-breadcrumb-item>
        </el-breadcrumb>
        <router-link :to="{name:'FoodsEditor'}" class="el-button el-button--danger">新增</router-link>
        <el-table :data="data" stripe border style="width: 100%">
            <el-table-column align="center" type="index" label="序号" width="80">
            </el-table-column>
            <el-table-column prop="name" label="图片" width="100">
                <template scope="scope">
                    <img class="p-img" :src="scope.row.img" alt="">
                </template>
            </el-table-column>
            <el-table-column prop="name" label="名字" width="180">
            </el-table-column>
            <el-table-column prop="type" label="分类" width="180">
            </el-table-column>
            <el-table-column prop="description" label="备注">
            </el-table-column>
            <el-table-column label="操作">
                <template scope="scope">
                    <el-button size="small" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
                    <el-button size="small" type="danger" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
                </template>
            </el-table-column>
        </el-table>

        <el-dialog title="提示" v-model="dialogVisible" size="tiny">
            <span>{{dialogMsg}}</span>
            <span slot="footer" class="dialog-footer">
                <el-button @click="dialogVisible = false;this.delItem={}">取 消</el-button>
                <el-button type="primary" @click="doDel">确 定</el-button>
            </span>
        </el-dialog>
    </div>
</template>
<script>
    export default {
        data() {
            return {
                data: [
                    { name: '阿根廷黑椒牛排', img: 'http://penkuoer.com/404.jpeg', type: '简餐', price: 29.81, description: '西式简餐' },
                    { name: '飘香拌面', img: 'http://penkuoer.com/404.jpeg', type: '沙县精选', price: 4.00, description: '以沙县经典小吃为主' }
                ],
                dialogVisible:false,
                delItem:{}, //点击删除后临时保存数据
                dialogMsg:''
            }
        },
        methods: {
            handleEdit(index, row) {
                console.log(index, row);
            },
            handleDelete(index, row) {
                this.dialogVisible = true
                this.dialogMsg = `确认删除:${row.name}`
                // console.log(index, row);
                this.delItem = row
            },
            doDel:function(){//确认删除
                this.dialogVisible = false
                console.log(this.delItem)
            }
        }
    }

</script>
<style>
    .p-img {
        max-width: 60px;
    }
</style>