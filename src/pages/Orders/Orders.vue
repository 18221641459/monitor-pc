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
            };
        },
        methods: {
            search() {
                let userName = this.searchNames;
				let serial = this.searchSerial;
				const url ='http://localhost:4431/orders/queryAllOrders';
				var params = new URLSearchParams();
				params.append("USER_NAME",userName);
				params.append("SERIAL",serial);
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
