<template>
    <div class="tables-basic">
        <b-breadcrumb>
            <b-breadcrumb-item>您的位置</b-breadcrumb-item>
            <b-breadcrumb-item active>商品管理</b-breadcrumb-item>
        </b-breadcrumb>
        <h2 class="page-title">Tables - <span class="fw-semi-bold">Static</span></h2>
        <b-row>
            <b-col>
                <Widget
                        title="<h5>Table <span class='fw-semi-bold'>Styles</span></h5>"
                        customHeader settings close
                >
                    <div class="table-resposive">
                        <table class="table">
                            <thead>
                            <tr>
                                <th class="hidden-sm-down">#</th>
                                <th>订单流水号</th>
                                <th class="hidden-sm-down">用户</th>
                                <th class="hidden-sm-down">总价</th>
                                <th class="hidden-sm-down">包含商品</th>
                                <th class="hidden-sm-down">增值服务</th>
                                <th class="hidden-sm-down">备注</th>
                            </tr>
                            </thead>
                            <tbody>
                            <tr v-for="row in tableStyles" :key="row.id">
                                <td>{{row.id}}</td>
                                <td>
                                    {{row.user}}
                                </td>
                                <td>
                                    {{row.wxID}}
                                </td>
                                <td>
                                    {{row.senior}}
                                </td>
                                <td class="text-semi-muted">
                                    {{row.integral}}
                                </td>
                                <td class="text-semi-muted">
                                    {{row.tellPhone}}
                                </td>
                                <td class="text-semi-muted">
                                    {{row.email}}
                                </td>
                                <td class="text-semi-muted">
                                    {{row.contact}}
                                </td>
                            </tr>
                            </tbody>
                        </table>
                    </div>
                    <div class="clearfix">
                        <div class="float-right">
                            <b-button variant="default" class="mr-xs" size="sm">添加</b-button>
                            <b-button variant="default" class="mr-xs" size="sm">添加</b-button>
                            <b-button variant="default" class="mr-xs" size="sm">添加</b-button>
                            <b-button variant="default" class="mr-xs" size="sm">添加</b-button>
                            <b-dropdown variant="inverse" class="mr-xs" size="sm" text="Clear" right>
                                <b-dropdown-item>Clear</b-dropdown-item>
                                <b-dropdown-item>Move ...</b-dropdown-item>
                                <b-dropdown-item>Something else here</b-dropdown-item>
                                <b-dropdown-divider />
                                <b-dropdown-item>Separated link</b-dropdown-item>
                            </b-dropdown>
                        </div>
                        <!--<p>Basic table with styled content</p>-->

                    </div>
                </Widget>
            </b-col>
        </b-row>
    </div>
</template>

<script>
    import axios from 'axios'
    import $ from 'jquery';
    import Vue from 'vue';
    import Widget from '@/components/Widget/Widget';
    import 'imports-loader?jQuery=jquery,this=>window!jquery-sparkline'; // eslint-disable-line

    export default {
        name: 'Orders',
        components: { Widget },
        data() {
            return {
                tableStyles: [
                    {
                        id: 1,
                        user: 'admin',
                        wxID: 'weixinid',
                        senior: 3,
                        integral: 300,
                        tellPhone: '13012345678',
                        email: '13012345678',
                        contact: '上海市浦东新区丁香路750号'
                    },
                    {
                        id: 2,
                        user: 'admin',
                        wxID: 'weixinid',
                        senior: 3,
                        integral: 300,
                        tellPhone: '13012345678',
                        email: '13012345678',
                        contact: '上海市浦东新区丁香路750号'
                    },
                    {
                        id: 3,
                        user: 'admin',
                        wxID: 'weixinid',
                        senior: 3,
                        integral: 300,
                        tellPhone: '13012345678',
                        email: '13012345678',
                        contact: '上海市浦东新区丁香路750号'
                    },


                ],
            };
        },
        methods: {
            parseDate(date) {
                const dateSet = date.toDateString().split(' ');
                return `${date.toLocaleString('en-us', { month: 'long' })} ${dateSet[2]}, ${dateSet[3]}`;
            },
            checkAll(ev, checkbox) {
                const checkboxArr = (new Array(this[checkbox].length)).fill(ev.target.checked);
                Vue.set(this, checkbox, checkboxArr);
            },
            changeCheck(ev, checkbox, id) {
                this[checkbox][id] = ev.target.checked;
                if (!ev.target.checked) {
                    this[checkbox][0] = false;
                }
            },
            getRandomData() {
                const result = [];

                for (let i = 0; i <= 8; i += 1) {
                    result.push(Math.floor(Math.random() * 20) + 1);
                }

                return result;
            },
            initCharts() {
                const colors = ['#547fff', '#9964e3', '#f55d5d', '#ffc247', '#3abf94'];
                $.each($('.sparkline-chart'), (id, chart) => {
                    $(chart).sparkline(this.getRandomData(), {
                        type: 'bar',
                        barColor: colors[Math.floor(Math.random() * colors.length)],
                    });
                });
            },
        },
        mounted() {
            var _this= this;
            this.initCharts();
            axios.post('http://localhost:8087/queryUsers', {
                params:{
                    // user:"admin"
                }},
                {headers: {
                    'Access-Control-Allow-Origin' : 'http://localhost:8087'
                }}

            ).then(function(res){
                // console.log(res)
                _this.tableStyles = res.data;
            }).catch(function(err){
                console.log(err)
            })

        },

    };
</script>

<style src="./Orders.scss" lang="scss" scoped />
