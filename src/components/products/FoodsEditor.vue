<template>
    <div>
        <el-breadcrumb separator="/">
            <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
            <el-breadcrumb-item :to="{name:'Foods'}">商品管理</el-breadcrumb-item>
            <el-breadcrumb-item>编辑</el-breadcrumb-item>
        </el-breadcrumb>
        <el-row justify="center" type="flex">
            <el-col :span="16">

                <el-form :model="goods" :rules="rules" ref="mainForm" label-width="100px">
                    <el-form-item label="名称" prop="name">
                        <el-input placeholder="请输入名称" v-model="goods.name"></el-input>
                    </el-form-item>
                    <el-form-item label="分类" prop="type">
                        <el-select v-model="goods.type" placeholder="请选择">
                            <el-option v-for="item in types" :label="item.value" :value="item.value">
                            </el-option>
                        </el-select>
                    </el-form-item>
                    <el-form-item label="价格" prop="price">
                        <el-input placeholder="请输入价格" v-model.number="goods.price"></el-input>
                    </el-form-item>
                    <el-form-item label="图片">
                        <!--
                            图片上传使用本地服务器 上传文件后显示的地址需要拼接一下服务地址
                        -->
                        <el-upload class="avatar-uploader" action="http://localhost:3000/common/file/uploadfile" :show-file-list="false" :on-success="handleAvatarScucess"
                            :before-upload="beforeAvatarUpload">
                            <img v-if="goods.img" :src="'http://localhost:3000/'+goods.img" class="avatar">
                            <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                            </el-upload>
                    </el-form-item>
                    <el-form-item label="描述" prop="description">
                        <el-input placeholder="请输入描述信息" type="textarea" autosize v-model="goods.description"></el-input>
                    </el-form-item>
                    <el-form-item>
                        <el-button @click="subHandle">提交</el-button>
                    </el-form-item>
                </el-form>
            </el-col>
        </el-row>
    </div>
</template>
<script>
    export default {
        data() {
            //自定义验证规则
            var validatePrice = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error('请输入价格'));
                } else {
                    if(isNaN(value)){
                        callback(new Error('价格必须为数字'));
                    }
                    else{
                        if(value<0 || value>200){
                            callback(new Error('价格必须介于0到200之间'));
                        }
                        else{
                            callback()
                        }
                    }
                }
            }
            return {
                goods: {
                    name: '',
                    description: '',
                    type: '',
                    img: '',
                    price: 0
                },
                rules: { //添加表单的验证规则
                    name: [
                        { required: true, message: '名称不能为空', trigger: 'blur' },
                        { min: 2, message: '名字文字的最小长度为2' }
                    ],
                    price: validatePrice,
                    type: { required: true, message: '分类信息不能为空' },
                    description: {
                        required: true,
                        message: '名字描述不能为空'
                    }
                },
                types: [
                    { "value": "简餐" },
                    { "value": "商务套餐" },
                    { "value": "沙县精选" },
                    { "value": "龙岩必点" },
                    { "value": "瓦罐系列" },
                    { "value": "川菜" }
                ]
            }
        },
        methods: {
            subHandle: function () {
                console.log(this.$refs['mainForm'])
                this.$refs['mainForm'].validate(valid => {
                    if (valid) {
                        alert('验证通过')
                    }
                    else {
                        console.log('vaildate error!')
                    }
                })
                console.dir(this.type)
            },
            handleAvatarScucess(res, file) {
                console.log(res)
                console.log(file)
                this.goods.img = res.url//URL.createObjectURL(file.raw)
            },
            beforeAvatarUpload(file) {
                const isJPG = file.type === 'image/jpeg';
                const isLt2M = file.size / 1024 / 1024 < 2;

                if (!isJPG) {
                    this.$message.error('上传图片只能是 JPG 格式!')
                }
                if (!isLt2M) {
                    this.$message.error('上传图片大小不能超过 2MB!')
                }
                return isJPG && isLt2M;
            }
        }
    }

</script>
<style>
    .avatar-uploader .el-upload {
        border: 1px dashed #d9d9d9;
        border-radius: 6px;
        cursor: pointer;
        position: relative;
        overflow: hidden
    }
    
    .avatar-uploader .el-upload:hover {
        border-color: #20a0ff
    }
    
    .avatar-uploader .avatar-uploader-icon {
        font-size: 28px;
        color: #8c939d;
        width: 178px;
        height: 178px;
        line-height: 178px;
        text-align: center
    }
    
    .avatar-uploader .avatar {
        width: 178px;
        height: 178px;
        display: block
    }
</style>