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
                    <label class="input-group-text" for="VIPSelect">星级</label>
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
                        customHeader  close
                >
                    <div class="table-resposive">
                        <table class="table table-striped">
                            <thead>
                            <tr>
                                <th class="hidden-sm-down">
                                #
                                </th>
                                <th>用户名</th>
                                <!--<th>微信号</th>-->
                                <th class="hidden-sm-down">星级</th>
                                <th class="hidden-sm-down">积分</th>
                                <th class="hidden-sm-down">手机号</th>
                                <th class="hidden-sm-down">邮箱</th>
                                <th class="hidden-sm-down">联系地址</th>
                            </tr>
                            </thead>
                            <tbody>
                            <tr v-for="row in originalArray" :key="row.id">
                                <td>{{row.id}}</td>
                                <td>
                                    {{row.user}}
                                </td>
                                <!--<td>-->
                                    <!--{{row.wxID}}-->
                                <!--</td>-->
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
                            <b-button variant="default" class="mr-xs" size="sm">冻结/解冻</b-button>
                        </div>
                        <!--<p>Basic table with styled content</p>-->

                    </div>
                </Widget>
            </b-col>
        </b-row>



        <Widget title="<h5><span class='fw-semi-bold'>详细信息</span></h5>"
                customHeader  close
        >
            <ul class="nav nav-tabs" id="myTab" role="tablist">
                <li class="nav-item">
                    <a class="nav-link active" id="home-tab" data-toggle="tab" href="#home" role="tab" aria-controls="home" aria-selected="true">基本信息</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" id="profile-tab" data-toggle="tab" href="#profile" role="tab" aria-controls="profile" aria-selected="false">资产信息</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" id="contact-tab" data-toggle="tab" href="#contact" role="tab" aria-controls="contact" aria-selected="false">星级</a>
                </li>
            </ul>
            <div class="tab-content" id="myTabContent">
                <div class="tab-pane fade show active" id="home" role="tabpanel" aria-labelledby="home-tab">Raw denim you probably haven't heard of them jean shorts Austin. Nesciunt tofu stumptown aliqua, retro synth master cleanse. Mustache cliche tempor, williamsburg carles vegan helvetica. Reprehenderit butcher retro keffiyeh dreamcatcher synth. Cosby sweater eu banh mi, qui irure terry richardson ex squid. Aliquip placeat salvia cillum iphone. Seitan aliquip quis cardigan american apparel, butcher voluptate nisi qui.</div>
                <div class="tab-pane fade" id="profile" role="tabpanel" aria-labelledby="profile-tab">Food truck fixie locavore, accusamus mcsweeney's marfa nulla single-origin coffee squid. Exercitation +1 labore velit, blog sartorial PBR leggings next level wes anderson artisan four loko farm-to-table craft beer twee. Qui photo booth letterpress, commodo enim craft beer mlkshk aliquip jean shorts ullamco ad vinyl cillum PBR. Homo nostrud organic, assumenda labore aesthetic magna delectus mollit. Keytar helvetica VHS salvia yr, vero magna velit sapiente labore stumptown. Vegan fanny pack odio cillum wes anderson 8-bit, sustainable jean shorts beard ut DIY ethical culpa terry richardson biodiesel. Art party scenester stumptown, tumblr butcher vero sint qui sapiente accusamus tattooed echo park.</div>
                <div class="tab-pane fade" id="contact" role="tabpanel" aria-labelledby="contact-tab">Etsy mixtape wayfarers, ethical wes anderson tofu before they sold out mcsweeney's organic lomo retro fanny pack lo-fi farm-to-table readymade. Messenger bag gentrify pitchfork tattooed craft beer, iphone skateboard locavore carles etsy salvia banksy hoodie helvetica. DIY synth PBR banksy irony. Leggings gentrify squid 8-bit cred pitchfork. Williamsburg banh mi whatever gluten-free, carles pitchfork biodiesel fixie etsy retro mlkshk vice blog. Scenester cred you probably haven't heard of them, vinyl craft beer blog stumptown. Pitchfork sustainable tofu synth chambray yr.</div>
            </div>


        </Widget>


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
        name: 'Users',
        components: { Widget },
        data() {
            return {
                searchNames: '',
                handledArray: [],
                originalArray: [
                    {
                        id: 1,
                        user: 'admin',
                        wxID: 'weixinid',
                        senior: 'VIP1',
                        integral: 300,
                        tellPhone: '13012345678',
                        email: '13012345678',
                        contact: '上海市浦东新区丁香路750号'
                    },
                    {
                        id: 2,
                        user: 'admin',
                        wxID: 'weixinid',
                        senior: 'VIP2',
                        integral: 300,
                        tellPhone: '13012345678',
                        email: '13012345678',
                        contact: '上海市浦东新区丁香路750号'
                    },
                    {
                        id: 3,
                        user: 'admin',
                        wxID: 'weixinid',
                        senior: 'VIP3',
                        integral: 300,
                        tellPhone: '13012345678',
                        email: '13012345678',
                        contact: '上海市浦东新区丁香路750号'
                    },


                ],
            };
        },
        methods: {
            // selectVIP: function(vip) {
            //     console.log(vip);
            //     // alert(vip);
            // },
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
                _this.originalArray = res.data;
                _this.handledArray = res.data;
            })

        },

    };
</script>

<style src="./Users.scss" lang="scss" scoped />
