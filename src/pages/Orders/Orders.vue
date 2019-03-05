<template>
    <div class="tables-basic">
        <b-breadcrumb>
            <b-breadcrumb-item>您的位置</b-breadcrumb-item>
            <b-breadcrumb-item active>订单管理</b-breadcrumb-item>
        </b-breadcrumb>
        <!--<h2 class="page-title"><span class="fw-semi-bold">用户列表</span></h2>-->
        <div id="screen">
            <div class="input-group mb-3">
                <div class="input-group-prepend">
                    <span class="input-group-text">用户</span>
                </div>
                <input type="text" class="form-control" v-model="searchNames"  aria-label="Default" aria-describedby="inputGroup-sizing-default">
            </div>
            <div class="input-group mb-3">
                <div class="input-group-prepend">
                    <span class="input-group-text">流水号</span>
                </div>
                <input type="text" class="form-control" v-model="searchSerial"  aria-label="Default" aria-describedby="inputGroup-sizing-default">
            </div>
            <div class="clock">
                <el-date-picker 
                  v-model="start_end_time"
                  type="datetimerange"
                  :picker-options="pickerOptions2"
                  range-separator="至"
                  start-placeholder="开始日期"
                  end-placeholder="结束日期"
                  size="small"
                  align="right" > 
                </el-date-picker>
            </div>
            <button type="button" v-on:click="search" class="btn btn-sm btn-outline-secondary ">查询</button>
        </div>


        <b-row>
            <b-col>
                <Widget title="<h5><span class='fw-semi-bold'>订单列表</span></h5>"
                        customHeader  close
                >
                    <div class="table-resposive">
                        <table class="table table-striped">
                            <thead>
                            <tr>
                                <th class="hidden-sm-down" style="-moz-column-rule-width: medium;">流水号</th>
                                <th class="hidden-sm-down" style="width: 150px">用户名</th>
                                <th class="hidden-sm-down">消费</th>
                                <th class="hidden-sm-down">订单内容</th>
                                <th class="hidden-sm-down">日期</th>
                                <th class="hidden-sm-down">订单状态</th>
                            </tr>
                            </thead>
                            <tbody>
                            <tr v-for="row in originalArray.list">
                                <td>
                                    {{row.serial}}
                                </td>
                                <td>
                                    {{row.user_name}}
                                </td>
                                <td>
                                    {{row.price}}
                                </td>
                                <td>
                                    {{row.context}}
                                </td>
                                <td>
                                    {{row.order_date}}
                                </td>
                                <td>
                                    {{row.status}}
                                </td>
                            </tr>
                            </tbody>
                        </table>
                    </div>

                        <div class="clearfix">
                            <nav class="page-bar" aria-label="Page navigation example">
                                <ul class="pagination">
                                    <li class="page-item">
                                        <a class="page-link" href="#" aria-label="Previous">
                                            <span aria-hidden="true" v-on:click="prePage" >&laquo;</span>
                                            <span class="sr-only">Previous</span>
                                        </a>
                                    </li>
                                    <li v-for="pages in originalArray.navigatepageNums" class="page-item"><a class="page-link"  v-on:click="thisPage({pages})" >{{pages}}</a></li>
                                    <li class="page-item">
                                        <a class="page-link" href="#" aria-label="Next">
                                            <span  v-on:click="nextPage" aria-hidden="true">&raquo;</span>
                                            <span class="sr-only">Next</span>
                                        </a>
                                    </li>
                                </ul>
                            </nav>
                        </div>

                </Widget>
            </b-col>
        </b-row>

    </div>

</template>

<script>
    import 'bootstrap/dist/css/bootstrap.min.css'
    import 'bootstrap/dist/js/bootstrap.min.js'
    import axios from 'axios'
    // import $ from 'jquery';
    import Vue from 'vue';
    import Widget from '@/components/Widget/Widget';
    import 'imports-loader?jQuery=jquery,this=>window!jquery-sparkline'; // eslint-disable-line

    export default {
        name: 'Orders',
        components: { Widget },
        data() {
            return {
                searchNames: '',
                searchSerial: '',
                handledArray: [],
                originalArray: [],
                pickerOptions2: {
                  shortcuts: [{
                    text: '最近一周',
                    onClick(picker) {
                      const end = new Date();
                      const start = new Date();
                      start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
                      picker.$emit('pick', [start, end]);
                    }
                  }, {
                    text: '最近一个月',
                    onClick(picker) {
                      const end = new Date();
                      const start = new Date();
                      start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
                      picker.$emit('pick', [start, end]);
                    }
                  }, {
                    text: '最近三个月',
                    onClick(picker) {
                      const end = new Date();
                      const start = new Date();
                      start.setTime(start.getTime() - 3600 * 1000 * 24 * 90);
                      picker.$emit('pick', [start, end]);
                    }
                  }]
                },
                value4: [new Date(2000, 10, 10, 10, 10), new Date(2000, 10, 11, 10, 10)],
                start_end_time: ''
            };
        },
        methods: {
            search() {
                let userName = this.searchNames;
                let serial = this.searchSerial;
                let startendtime = this.start_end_time;
                const url ='http://localhost:4431/orders/queryAllOrders';
                var params = new URLSearchParams();
                params.append("USER_NAME",userName);
                params.append("SERIAL",serial);
                params.append("startendtime",startendtime);
                this.$axios({
                    method: 'post',
                    url:url,
                    data:params
                },{headers: {
                        'Access-Control-Allow-Origin' : 'http://localhost:4431',
                    }}
                    ).then((res)=>{
                    this.originalArray = res.data;
                });
            },
            prePage() {
                const url ='http://localhost:4431/orders/queryAllOrders';
                var params = new URLSearchParams();
                params.append("PageNum",this.originalArray.pageNum-1);
                this.$axios({
                    method: 'post',
                    url:url,
                    data:params
                },{headers: {
                        'Access-Control-Allow-Origin' : 'http://localhost:4431',
                    }}
                    ).then((res)=>{
                    this.originalArray = res.data;
                });
            },
            nextPage() {
                const url ='http://localhost:4431/orders/queryAllOrders';
                var params = new URLSearchParams();
                params.append("PageNum",this.originalArray.pageNum+1);
                this.$axios({
                    method: 'post',
                    url:url,
                    data:params
                },{headers: {
                        'Access-Control-Allow-Origin' : 'http://localhost:4431',
                    }}
                    ).then((res)=>{
                    this.originalArray = res.data;
                });
			},
			thisPage(pages) {
                const url ='http://localhost:4431/orders/queryAllOrders';
                var params = new URLSearchParams();
                params.append("PageNum",pages.pages);
                this.$axios({
                    method: 'post',
                    url:url,
                    data:params
                },{headers: {
						'Access-Control-Allow-Origin' : 'http://localhost:4431',
                    }}
                    ).then((res)=>{
                    this.originalArray = res.data;
                });
			}
        },
        mounted() {
            var _this= this;
            axios.post('http://localhost:4431/orders/queryAllOrders', {
                params:{

                }},
                {headers: {
                    'Access-Control-Allow-Origin' : 'http://localhost:4431'
                }}

            ).then(function(res){
                _this.originalArray = res.data;
                _this.handledArray = res.data;
            })

        },

    };
</script>

<style src="./Orders.scss" lang="scss" scoped />
