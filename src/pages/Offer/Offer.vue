<template>
    <div class="tables-basic">
        <b-breadcrumb>
            <b-breadcrumb-item>您的位置</b-breadcrumb-item>
            <b-breadcrumb-item active>销售品管理</b-breadcrumb-item>
        </b-breadcrumb>
        <div id="screen">
            <div class="input-group mb-3">
                <div class="input-group-prepend">
                    <span class="input-group-text">销售品名</span>
                </div>
                <input type="text" class="form-control" v-model="searchNames"  aria-label="Default" aria-describedby="inputGroup-sizing-default">
            </div>
			<div class="input-group mb-3">
			   <div class="input-group-prepend">
			       <label class="input-group-text" for="StatusSelect">状态</label>
			   </div>
			   <select class="custom-select" id="StatusSelect">
			       <option value="0" selected>全部</option>
			       <option value="1" >已上架</option>
			       <option value="2" >已下架</option>
			   </select>
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
                                <th class="hidden-sm-down">#</th>
                                <th class="hidden-sm-down">销售品名</th>
                                <th class="hidden-sm-down">类型</th>
                                <th class="hidden-sm-down">价格</th>
                                <th class="hidden-sm-down">状态</th>
                                <th class="hidden-sm-down">描述</th>
								<th class="hidden-sm-down">备注</th>
								<th class="hidden-sm-down">&nbsp;&nbsp;&nbsp;&nbsp;</th>
                            </tr>
                            </thead>
                            <tbody>
                            <tr v-for="row in originalArray.list">
                                <td>
									{{row.offer_ID}}
								</td>
                                <td>
                                    {{row.offer_NAME}}
                                </td>
                                <td>
                                    {{row.offer_TYPE=="M"? "监控服务":"增值服务"}}
                                </td>
								<td>
									￥{{row.pricing}}
								</td>
                                <td>
                                    {{row.status_CD}}
                                </td>
                                <td>
                                    {{row.offer_DESC}}
                                </td>
								<td>
								    {{row.remark}}
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
							<!-- 新增 -->
							<button type="button" style="margin-left: 40%;height: 30px;" class="btn btn-primary" data-toggle="modal" data-target="#addOfferModalLong">新增</button>
							<div class="modal fade" id="addOfferModalLong" tabindex="-1" role="dialog" aria-labelledby="exampleModalLongTitle" aria-hidden="true">
							  <div class="modal-dialog" role="document">
								<div class="modal-content">
								  <div class="modal-header">
									<h5 class="modal-title" id="exampleModalLongTitle">新增商品</h5>
									<button type="button" class="close" data-dismiss="modal" aria-label="Close">
									  <span aria-hidden="true">&times;</span>
									</button>
								  </div>
								  <div class="modal-body" >
									<el-form ref="form" :model="form" label-width="80px">
									  <el-form-item label="活动名称">
										<el-input v-model="form.name"></el-input>
									  </el-form-item>
									  <el-form-item label="活动区域">
										<el-select v-model="form.region" placeholder="请选择活动区域">
										  <el-option label="区域一" value="shanghai"></el-option>
										  <el-option label="区域二" value="beijing"></el-option>
										</el-select>
									  </el-form-item>
									  <el-form-item label="活动时间">
										<el-col :span="11">
										  <el-date-picker type="date" placeholder="选择日期" v-model="form.date1" style="width: 100%;"></el-date-picker>
										</el-col>
										<el-col class="line" :span="2">-</el-col>
										<el-col :span="11">
										  <el-time-picker type="fixed-time" placeholder="选择时间" v-model="form.date2" style="width: 100%;"></el-time-picker>
										</el-col>
									  </el-form-item>
									  <el-form-item label="即时配送">
										<el-switch v-model="form.delivery"></el-switch>
									  </el-form-item>
									  <el-form-item label="活动性质">
										<el-checkbox-group v-model="form.type">
										  <el-checkbox label="美食/餐厅线上活动" name="type"></el-checkbox>
										  <el-checkbox label="地推活动" name="type"></el-checkbox>
										  <el-checkbox label="线下主题活动" name="type"></el-checkbox>
										  <el-checkbox label="单纯品牌曝光" name="type"></el-checkbox>
										</el-checkbox-group>
									  </el-form-item>
									  <el-form-item label="特殊资源">
										<el-radio-group v-model="form.resource">
										  <el-radio label="线上品牌商赞助"></el-radio>
										  <el-radio label="线下场地免费"></el-radio>
										</el-radio-group>
									  </el-form-item>
									  <el-form-item label="活动形式">
										<el-input type="textarea" v-model="form.desc"></el-input>
									  </el-form-item>
									  <el-form-item>
										<el-button type="primary" @click="onSubmit">立即创建</el-button>
										<el-button>取消</el-button>
									  </el-form-item>
									</el-form>
								  </div>
								  <div class="modal-footer">
									<button type="button" class="btn btn-secondary" data-dismiss="modal">取消</button>
									<button type="button" class="btn btn-primary">新增</button>
								  </div>
								</div>
							  </div>
							</div>
							<!-- //新增 -->
							
							
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
    import $ from 'jquery';
    import Vue from 'vue';
    import Widget from '@/components/Widget/Widget';
    import 'imports-loader?jQuery=jquery,this=>window!jquery-sparkline'; // eslint-disable-line

    export default {
        name: 'Offer',
        components: { Widget },
        data() {
            return {
                searchNames: '',
				searchSerial: '',
                handledArray: [],
                originalArray: [],
				form: {
				  name: '',
				  region: '',
				  date1: '',
				  date2: '',
				  delivery: false,
				  type: [],
				  resource: '',
				  desc: ''
				}
            };
        },
        methods: {
            search() {
                let userName = this.searchNames;
				let status = $("#StatusSelect").val();
				const url ='http://localhost:4431/offer/queryAllOffers';
				var params = new URLSearchParams();
				params.append("OFFER_NAME",userName);
				params.append("STATUS_CD",status);
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
				const url ='http://localhost:4431/offer/queryAllOffers';
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
				const url ='http://localhost:4431/offer/queryAllOffers';
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
				const url ='http://localhost:4431/offer/queryAllOffers';
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
			},
			onSubmit() {
				console.log('submit!');
			}
			
        },
        mounted() {
            var _this= this;
            axios.post('http://localhost:4431/offer/queryAllOffers', {
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

<style src="./Offer.scss" lang="scss" scoped />
