<template>
    <div class="tables-basic">
        <b-breadcrumb>
            <b-breadcrumb-item>您的位置</b-breadcrumb-item>
            <b-breadcrumb-item active>数据报表</b-breadcrumb-item>
        </b-breadcrumb>
        <!--<h2 class="page-title"><span class="fw-semi-bold">用户列表</span></h2>-->
        <div id="screen">
            <div class="input-group mb-3">
                <div class="input-group-prepend">
                    <span class="input-group-text" id="inputGroup-sizing-default">用户</span>
                </div>
                <input type="text" class="form-control" v-model="searchNames" v-on:input ="search" aria-label="Default" aria-describedby="inputGroup-sizing-default">
            </div>
            <div class="input-group mb-3">
                <div class="input-group-prepend">
                    <label class="input-group-text" >星级</label>
                </div>
                <select class="custom-select" id="VIPSelect">
                    <option value="all" selected>全部</option>
                    <option value="VIP1" >VIP1</option>
                    <option value="VIP2" >VIP2</option>
                    <option value="VIP3" >VIP3</option>
                </select>
            </div>
            <button type="button" v-on:click="search" class="btn btn-sm btn-outline-secondary ">查询</button>
        </div>


        <b-row>
            <b-col>
                <Widget title="<h5><span class='fw-semi-bold'>用户列表</span></h5>"
                        customHeader
                >

                    <div class="table-resposive">
                        <table class="table table-striped">
                            <thead>
                            <tr>
                                <th class="hidden-sm-down">
                                    #
                                </th>
                                <th>用户名</th>
                                <th class="hidden-sm-down">手机号</th>
                                <th class="hidden-sm-down">邮箱</th>
                                <th class="hidden-sm-down">联系地址</th>
                                <th class="hidden-sm-down">注册日期</th>
                                <th class="hidden-sm-down">状态</th>
                            </tr>
                            </thead>
                            <tbody>
                            <tr  v-for="row in originalArray" :key="row.id">
                                <td>{{row.id}}</td>
                                <td v-on:click="showDetail" :data-id="row.id">
                                    {{row.userName}}
                                </td>
                                <td>
                                    {{row.cellPhone}}
                                </td>
                                <td class="text-semi-muted">
                                    {{row.email}}
                                </td>
                                <td class="text-semi-muted">
                                    {{row.contactAddress}}
                                </td>
                                <td class="text-semi-muted">
                                    {{row.registerDate}}
                                </td>
                                <td class="text-semi-muted">
                                    {{row.status == 0 ? "正常" : "注销"}}
                                    <!--item.day>0?item.resource:''-->
                                </td>
                            </tr>
                            </tbody>
                        </table>
                    </div>

                    <el-pagination
                            @size-change="handleSizeChange"
                            @current-change="handleCurrentChange"
                            :current-page.sync="currentPage"
                            :page-size="pageSize"
                            :page-sizes="[5, 10, 15, 20]"
                            layout="sizes, prev, pager, next, jumper"
                            :total="total">
                    </el-pagination>


                </Widget>
            </b-col>
        </b-row>



        <Widget v-show="isShow"  title="<h5><span class='fw-semi-bold'>详细信息</span></h5>"
                customHeader
        >
            <div v-on:click="closeDetail"  class="icon-list-item"><span v-on:click="closeDetail"  class="glyphicon glyphicon-remove" /></div>
            <el-tabs v-model="activeName" type="card" @tab-click="handleClick">
                <el-tab-pane label="用户管理" name="userManager">

                    <el-form ref="form" :model="form" label-width="80px" >
                        <el-form-item label="用户名">
                            <el-input class="userImput" v-model="form.name"></el-input>
                        </el-form-item>
                        <el-form-item label="手机号">
                            <el-input  class="userImput" v-model="form.cellPhone"></el-input>
                        </el-form-item>
                        <el-form-item label="邮箱">
                            <el-input class="userImput" v-model="form.email"></el-input>
                        </el-form-item>
                        <el-form-item label="联系地址">
                            <el-input class="userImput" v-model="form.contactAddress"></el-input>
                        </el-form-item>
                        <el-form-item label="注册日期">
                            <el-input class="userImput" v-model="form.registerDate"></el-input>
                        </el-form-item>
                        <el-form-item label="状态">
                            <el-input class="userImput" v-model="form.status"></el-input>
                        </el-form-item>

                    </el-form>

                </el-tab-pane>
                <el-tab-pane label="配置管理" name="propertyManager">资产信息</el-tab-pane>
                <el-tab-pane label="角色管理" name="seniorManager">星级</el-tab-pane>

            </el-tabs>



        </Widget>

    </div>





</template>

<script>
    import 'bootstrap/dist/css/bootstrap.min.css'
    import 'bootstrap/dist/js/bootstrap.min.js'
    // import axios from 'axios'
    import $ from 'jquery';
    // import Vue from 'vue';
    import Widget from '@/components/Widget/Widget';
    import 'imports-loader?jQuery=jquery,this=>window!jquery-sparkline'; // eslint-disable-line

    export default {
        name: 'Users',
        components: { Widget },
        data() {
            return {
                isShow: false,
                form: {
                    name: '',
                    cellPhone: '',
                    email: '',
                    date2: '',
                    delivery: false,
                    type: [],
                    resource: '',
                    desc: ''
                },
                currentPage: 5,
                pageSize : 5,
                total: 10,
                activeName: 'userManager',
                value1: '',
                searchNames: '',
                handledArray: [],
                originalArray: [],
                detail:{
                    callPhone : "ertrytrervb"
                },
            };
        },
        methods: {

            /**
             * 展示详情
             */
            showDetail(event){
                this.isShow = true
                console.log(event.currentTarget.dataset.id)
                // console.log(event.srcElement.key)
            },
            closeDetail(){
                this.isShow = false
            },
            onSubmit() {
                console.log('submit!');
            },

            handleClick(tab, event) {

                console.log(tab, event);
            },
            /**
             * 给变每页显示数量
             */
            handleSizeChange(val) {
                // console.log(`每页 ${val} 条`);
                this.pageSize = val;
                this.acquireCustoner(1,val);
            },

            /**
             *翻页
             */
            handleCurrentChange(val) {
                // console.log(`当前页: ${val}`);
                this.acquireCustoner(val,this.pageSize );
            },
            searchName() {
                let handledArray = [];
                let originalArray = this.originalArray;
                for (let co in originalArray){
                    let user = originalArray[co];
                    let name = user.user;
                    if(name.includes(this.searchNames)){
                        handledArray.push(user);
                    }
                }
                this.handledArray = handledArray;
            },
            search() {
                let userName = this.searchNames;
                // console.log(userName);
                let vip = $("#VIPSelect").val();
                // console.log(vip);
                let handledArray = [];
                let originalArray = this.originalArray;
                let temp = []
                for (let co in originalArray){
                    let user = originalArray[co];
                    let senior = user.senior;
                    if ('all' == vip || senior.includes(vip)){
                        temp.push(user);
                    }

                }
                for (let co in temp){
                    let user = originalArray[co];
                    let name = user.user;
                    if(name.includes(userName)){
                        handledArray.push(user);
                    }
                }
                this.handledArray = handledArray;
            },

            /**
             * 分页查询用户信息
             * @param pageNum
             * @param pageSize
             */
            acquireCustoner(pageNum, pageSize){
                var params = new URLSearchParams();
                params.append("pageNum",pageNum);
                params.append("pageSize",pageSize);
                var url = 'http://localhost:4431/acquireCustoner';
                this.$axios({
                        method: 'post',
                        url:url,
                        data:params
                    },{headers: {
                            'Access-Control-Allow-Origin' : 'http://localhost:4431',
                        }}
                ).then((res)=>{
                    // console.log(res.data)
                    this.originalArray = res.data.list;
                    this.pageSize = res.data.pageSize;
                    this.total = res.data.total;
                });
            },

        },
        mounted() {

            var _this= this;
            _this.acquireCustoner(1,5);

        },

    };
</script>

<style src="./Users.scss" lang="scss" scoped />
