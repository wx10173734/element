<template>
  <div class="app_container">
    <el-card class="app_container_card">
      <el-row type="flex" align="middle">
        <el-col :span="8">
          <el-date-picker style="width :100%;" v-model="dateRangeValue" type="daterange"
                          unlink-panels range-separator="至" start-placeholder="开始日期" end-placeholder="结束日期"
                          :pick-options="pickerOptions">
          </el-date-picker>
        </el-col>
        <el-col :span="4">
          <el-button type="primary" icon="el-icon-search">搜索</el-button>
        </el-col>
      </el-row>
      <el-row type="flex" justify="left" align="top">
        <el-col :span="12" :xs="24" :sm="24" :md="24" :lg="24" xl="24">
          <el-table :data="regionalsignlefinishlist" :header-cell-style="headerCellStyle" border
                    v-loading="listLoading">
            <template v-for="(header,hkey) in tableHeaders">
              <el-table-column :key="hkey" :label="header.label" align="center">
                <template v-if="header.children">
                  <el-table-column v-for="(label,index) in header.children" :key="index"
                                   :label="label.label" align="center">
                    <template slot-scope="scope">
                      <template v-if="scope.row[label.prop] instanceof Array">
                        <el-row type="flex" v-for="(item,index) in scope.row[label.prop]" :key="index">
                          <el-col :style="{
                          'color' : label.label === '完成率' ? '#00b139' : '',
                           'color' : label.label === '同步率' && index === 0 ? '#ff2230':''
                        }"
                                  :class="{
                          yellow_cell:item === '年同比增长' || item === '环比增长' || item === '同比增长' || item === '完成率',
                          gray_cell:item === '上月同期业绩' || item === '去年同期业绩' || item === '目标'
                        }"
                                  :span="24" v-text="item"></el-col>
                        </el-row>
                      </template>
                      <template v-else>
                        <el-row type="flex">
                          <el-col :style="{'color' : label.label === '完成率' ? '#00b139': ''}" :span="24"
                                  v-text="scope.row[label.prop]">

                          </el-col>
                        </el-row>
                      </template>
                    </template>
                  </el-table-column>
                </template>
              </el-table-column>
            </template>
          </el-table>
          <el-row type="flex" justify="left" align="top">
            <el-col :span="12" :xs="24" :md="24" :lg="24" :xl="24">
              <el-table ref="adminTable" :data="regionalsignlefinishlist" style="width: 100%"
                        @selection-change="handleSelectionChange" :header-cell-style="headerCellStyle2"
                        :cell-style="bodyCellStyle" v-loadling="listLoading" border>
                <el-table-column :label="testtext" align="left" :render-header="renderHeader"></el-table-column>
              </el-table>
            </el-col>
          </el-row>
        </el-col>
      </el-row>
    </el-card>
  </div>
</template>

<script>
  export default {
    name: "Table",
    data() {
      return {
        dateRangeValue: '',
        pickerOptions: {
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

        //表格头
        tableHeaders: [
          {
            label: '发货数据截止2019.7.22',
            children: [
              {
                label: '昨天发货金额(不含明弘直营)',
                prop: 'shipYestAmount'
              },
              {
                label: '当月发货金额(不含明弘直营)',
                prop: 'shipMonthAmount'
              },
              {
                label: '当月明弘直营发货金额',
                prop: 'shipMonthNotAmount'
              },
              {
                label: '月目标',
                prop: 'shipMonthlyTarget'
              },
              {
                label: '完成率',
                prop: 'shipCompletionRate'
              },
              {
                label: '2019截止昨天累计发货金额',
                prop: 'shipThisYearAmount'
              },
              {
                label: '2018截止7月25号累计发货金额',
                prop: 'shipLastYearAmount'
              },
            ]
          },
          {
            label: '门店销售数据截止2019.7.22',
            children: [
              {
                label: '昨天业绩',
                prop: 'storeYestPerformance'
              },
              {
                label: '当月总业绩',
                prop: 'storeMonthTotalResults'
              },
              {
                label: '月同步销售目标',
                prop: 'storeMonthSalesTarget'
              },
              {
                label: '同步率',
                prop: 'storeSyncRate'
              },
              {
                label: '月目标',
                prop: 'storeMonthlyTarget'
              },
              {
                label: '完成率',
                prop: 'storeCompletionRate'
              },
              {
                label: '2019年截止昨日累计业绩',
                prop: 'storeThisYearAmount'
              },
              {
                label: '2018年同期累计业绩',
                prop: 'storeLastYearAmount'
              },
            ]
          },
          {
            label: '直营',
            children: [
              {
                label: '销售额',
                prop: 'direOperSales'
              }
            ]
          },
          {
            label: '运营',
            children: [
              {
                label: '昨天业绩',
                prop: 'direOperation'
              }
            ]
          },
        ],
        regionalsignlefinishlist: [{
          //发货数据
          shipYestAmount: '933.2万',
          shipMonthAmount: '5419.3万',
          shipMonthNotAmount: '847.7万',
          shipMonthlyTarget: '8748.0万',
          shipCompletionRate: '72.93%',
          shipThisYearAmount: '62350.8万',
          shipLastYearAmount: ['62350.8万', '年同比增长', '5.92%'],
          //门店销售数据
          storeYestPerformance: '562.4万',
          storeMonthTotalResults: '22200.3万',
          storeMonthSalesTarget: ['22200.4万', '上月同期业绩', '22200.5万', '去年同期业绩', '18077.2万'],
          storeSyncRate: ['100.36%', '环比增长', '-3.93%', '同比增长', '21.93%'],
          storeMonthlyTarget: '22604.2万',
          storeCompletionRate: '83.43%',
          storeThisYearAmount: '17954.5万',
          storeLastYearAmount: ['13155.9', '年同比增长', '22200.5万', '29.95%'],
          //直营
          direOperSales: ['3248.4万', '目标', '3626.7万', '完成率', '89.57万'],
          //运营
          direOperation: ['18803.4万', '目标', '20009.5万', '完成率', '93.98万'],
        }],
        listLoading: false,
        testtext: '直发数据核算方式 \n-----------------------------------------' +
          '累积发货金额以财务计算为准'
      }
    }
    ,
    methods: {
      headerCellStyle({row, column, rowIndex, columnIndex}) {
        if (columnIndex === 0 && rowIndex === 0) { //指定坐标
          return 'background-color:#B9D7F1'
        } else if ((columnIndex === 1 || columnIndex === 2 || columnIndex === 3) && rowIndex === 0) {
          return 'background-color:#BFBFBF'
        } else if ((columnIndex === 5 || columnIndex === 6 || columnIndex === 13
          || columnIndex === 14) && rowIndex === 1) {
          return 'background-color:#FF0000'  //第二行
        } else if ((columnIndex === 0 || columnIndex === 1 || columnIndex === 2
          || columnIndex === 7 || columnIndex === 8 || columnIndex === 10) && rowIndex === 1) {
          return 'background-color:#FBFF00' //第二行
        } else if ((columnIndex === 3 || columnIndex === 4 || columnIndex === 11
          || columnIndex === 12 || columnIndex === 15 || columnIndex === 16) && rowIndex === 1) {
          return 'background-color:#FFC300'
        } else if ((columnIndex === 9) && rowIndex === 1) {
          return 'background-color:#D9D9D9'
        }
      },
      headerCellStyle2({
                         row,
                         column,
                         rowIndex,
                         columnIndex
                       }) {
        if (rowIndex === 0) { //指定坐标
          return 'background-color:#FBFF00'
        }
      }

    }

  }
</script>

<style lang="scss" scoped>
  .app_container {
    & /deep/ .el-row {
      margin-bottom: 10px;

      &:last-child {
        margin-bottom: 0;
      }
    }

    & /deep/ .el-table .cell {
      padding: 0;
    }

    & /deep/ .yellow_cell {
      border-top: 1px #dfe6ec solid;
      border-bottom: 1px #dfe6ec solid;
      background: #FBFF00;
    }

    & /deep/ .gray_cell {
      border-top: 1px #dfe6ec solid;
      border-bottom: 1px #dfe6ec solid;
      background: #BFBFBF;
    }
  }

  .table_column {
    width: 100%;
  }

  //操作栏样式
  .app_container__card {
    padding: 0 0 30px 0;

    .btn_add {
      float: right;
    }
  }

  //批量操作栏样式
  .batch_operate_container {
    display: inline-block;
    margin-top: 20px;
  }

  //分页栏样式
  .pagination_container {
    float: right;
    margin-top: 20px;
  }

  .el-table.cell {
    white-space: pre-line;
  }
</style>
