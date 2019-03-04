<template>
    <div class="tables-basic">
        <b-breadcrumb>
            <b-breadcrumb-item>您的位置</b-breadcrumb-item>
            <b-breadcrumb-item active>数据报表</b-breadcrumb-item>
        </b-breadcrumb>


        <button></button>
                <Widget
                        title="<h5> <span class='fw-semi-bold'>用户月活跃量统计</span></h5>"
                        customHeader settings close
                >
                    <div id="userActivity"></div>

                </Widget>

        <Widget
                title="<h5> <span class='fw-semi-bold'>每天营收额统计</span></h5>"
                customHeader settings close
        >
            <div id="revenueStatistics" style="width: 100%;height:400px;"></div>

        </Widget>

    </div>
</template>

<script>

    import Widget from '@/components/Widget/Widget';
    import echarts from 'echarts';
    export default {
        name: "Statements",
        components: { Widget },
        data() {
            return{
                userActivity : {},
                revenueStatistics : {},
            }
        },
        methods: {

            con(){
              console.log(this.userActivity)
            },

            echartInit(){
                // var echarts = require('echarts');
                // 基于准备好的dom，初始化echarts实例
                var userActivity = echarts.init(document.getElementById('userActivity'));
                // 绘制图表
                userActivity.setOption({

                    legend: {
                        data:['活跃量','新增用户数']
                    },
                    tooltip: {},
                    xAxis: {
                        name: "日期",
                        data: ['2019-01', '2019-02','2019-03','2019-04','2019-05','2019-06','2019-07','2019-08','2019-09','2019-10','2019-11','2019-12',],
                        // max: 15,
                        // interval: 30,
                        splitNumber: 12,
                        type: 'category',
                    },
                    yAxis: {
                        name: "活跃量/人",
                        type: "value"
                    },
                    series: [{
                        color: 'rgba(64, 103, 160)',
                        name: '活跃量',
                        type: 'bar',
                        barWidth: 35,
                        data: [5, 20, 36, 10, 10, 20, 5, 20, 36, 10, 10, 20],
                        label: {
                            show: true,
                            position: 'top',
                        }
                    },
                        {
                            color: '#3285ff',
                            name: '新增用户数',
                            type: 'line',
                            data: [5, 4, 6, 0, 1, 9, 5, 4, 6, 2, 5, 3],

                        }
                    ]
                });

                var dates = ["2019-01-01",
                    "2019-01-02",
                    "2019-01-03",
                    "2019-01-04",
                    "2019-01-05",
                    "2019-01-06",
                    "2019-01-07",
                    "2019-01-08",
                    "2019-01-09",
                    "2019-01-10",
                    "2019-01-11",
                    "2019-01-12",
                    "2019-01-13",
                    "2019-01-14",
                    "2019-01-15",
                    "2019-01-16",
                    "2019-01-17",
                    "2019-01-18",
                    "2019-01-19",
                    "2019-01-20",
                    "2019-01-21",
                    "2019-01-22",
                    "2019-01-23",
                    "2019-01-24",
                    "2019-01-25",
                    "2019-01-26",
                    "2019-01-27",
                    "2019-01-28",
                    "2019-01-29",
                    "2019-01-30",
                    "2019-01-31",
                    "2019-02-01",
                    "2019-02-02",
                    "2019-02-03",
                    "2019-02-04",
                    "2019-02-05",
                    "2019-02-06",
                    "2019-02-07",
                    "2019-02-08",
                    "2019-02-09",
                    "2019-02-10",
                    "2019-02-11",
                    "2019-02-12",
                    "2019-02-13",
                    "2019-02-14",
                    "2019-02-15",
                    "2019-02-16",
                    "2019-02-17",
                    "2019-02-18",
                    "2019-02-19",
                    "2019-02-20",
                    "2019-02-21",
                    "2019-02-22",
                    "2019-02-23",
                    "2019-02-24",
                    "2019-02-25",
                    "2019-02-26",
                    "2019-02-27",
                    "2019-02-28",
                    "2019-03-01",
                    "2019-03-02",
                    "2019-03-03",
                    "2019-03-04",
                    "2019-03-05",
                    "2019-03-06",
                    "2019-03-07",
                    "2019-03-08",
                    "2019-03-09",
                    "2019-03-10",
                    "2019-03-11",
                    "2019-03-12",
                    "2019-03-13",
                    "2019-03-14",
                    "2019-03-15",
                    "2019-03-16",
                    "2019-03-17",
                    "2019-03-18",
                    "2019-03-19",
                    "2019-03-20",
                    "2019-03-21",
                    "2019-03-22",
                    "2019-03-23",
                    "2019-03-24",
                    "2019-03-25",
                    "2019-03-26",
                    "2019-03-27",
                    "2019-03-28",
                    "2019-03-29",
                    "2019-03-30",
                    "2019-03-31",]
                var datas = [24,
                    90,
                    8 ,
                    26,
                    26,
                    17,
                    79,
                    13,
                    81,
                    43,
                    20,
                    70,
                    23,
                    19,
                    97,
                    19,
                    54,
                    87,
                    69,
                    0 ,
                    27,
                    99,
                    85,
                    37,
                    55,
                    25,
                    73,
                    75,
                    18,
                    19,
                    47,
                    89,
                    25,
                    82,
                    63,
                    90,
                    69,
                    90,
                    31,
                    25,
                    57,
                    37,
                    65,
                    16,
                    56,
                    6 ,
                    49,
                    24,
                    77,
                    65,
                    94,
                    31,
                    53,
                    36,
                    63,
                    49,
                    76,
                    87,
                    21,
                    46,
                    1 ,
                    58,
                    91,
                    73,
                    35,
                    19,
                    60,
                    36,
                    13,
                    80,
                    44,
                    49,
                    76,
                    10,
                    79,
                    96,
                    54,
                    72,
                    68,
                    0 ,
                    39,
                    68,
                    80,
                    82,
                    26,
                    82,
                    37,
                    32,
                    12,
                    4  ]

                var revenueStatistics = echarts.init(document.getElementById('revenueStatistics'));
                // 绘制图表
                revenueStatistics.setOption({

                    legend: {
                        data:['营收额']
                    },

                    tooltip: {
                        trigger: 'axis'
                    },
                    xAxis: {
                        name: "日期",
                        data: dates,
                        type: 'category',
                        boundaryGap: false,
                    },
                    yAxis: {
                        name: "营收额/元",
                        type: "value",

                    },
                    series: [
                        {
                            color: '#3285ff',
                            name: '营收额',
                            type: 'line',
                            data: datas,
                            sampling: 'average',
                            itemStyle: {
                                color: 'rgb(255, 70, 131)'
                            },
                            areaStyle: {
                                color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
                                    offset: 0,
                                    color: 'rgb(255, 158, 68)'
                                }, {
                                    offset: 1,
                                    color: 'rgb(255, 70, 131)'
                                }])
                            },

                        }
                    ]
                });


                this.userActivity = userActivity;
                this.revenueStatistics = revenueStatistics;


            }
        },
        mounted: function(){
             this.echartInit();

        },
    }








</script>

<style src="./Statements.scss" lang="scss" scoped />
