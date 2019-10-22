<template lang="">
  <div>
    <div>
      <el-row :gutter="20">
        <el-col :span="4">
          <div class="grid-content bg-purple">
            <ul class="listflow">
              <li>
                <p class="titile">
                  今日进程
                </p>
                <p class="nums">
                  500
                </p>
              </li>
            </ul>
          </div>
        </el-col>
        <el-col :span="4">
          <div class="grid-content bg-purple">
            <ul class="listflow">

              <li>
                <p class="titile">
                  总进程
                </p>
                <p class="nums">
                  500
                </p>
              </li>
            </ul>
          </div>
        </el-col>
        <el-col :span="4">
          <div class="grid-content bg-purple">
            <ul class="listflow">
              <li>
                <p class="titile">

              <li>
                <p class="titile">
                  总金额
                </p>
                <p class="nums">
                  100
                </p>
              </li>

            </ul>
          </div>
        </el-col>
        <el-col :span="4">
          <div class="grid-content bg-purple">
            <ul class="listflow">

              </li>
              <li>
                <p class="titile">
                  在线天数
                </p>
                <p class="nums">
                  50
                </p>
              </li>

            </ul>
          </div>
        </el-col>
      </el-row>
    </div>
    <div class="setGetAppPoolList" style="margin-top:3%">

    </div>
    <el-table class="tablist" :data="tableData" max-height="700" style="width: 100%">
      <!-- <el-table-column  prop="id"  label="Id" > </el-table-column> -->
      <el-table-column prop="name" label="日期"> </el-table-column>
      <el-table-column prop="phone" label="进程数新/扣/重"> </el-table-column>
      <el-table-column prop="state" label="单价"> </el-table-column>
      <el-table-column prop="classHour" label="结算金额">
      </el-table-column>
      <el-table-column prop="ladder" label="实际结算金额">
      </el-table-column>
      <el-table-column fixed="right" label="操作" width="350">
        <template slot-scope="scope">
          <el-button @click="handleinfo(scope.row)" type="primary" size="small">详情信息</el-button>
        </template>
      </el-table-column>
    </el-table>
    <div class="block">
      <el-pagination @current-change="handleCurrentChange" :current-page.sync="currentPage" layout="prev, pager, next"
        :page-size="pageSize" :hide-on-single-page="true" :total="total"></el-pagination>
    </div>

  </div>
</template>

<script>
  export default {
    data() {
      return {
        form: {},
        tableData: [],
        pageNum: 0,
        pageSize: 10,
        total: 10,
        payrow: "",
        currentPage: 1,
        dialogTableVisible: false
      };
    },
    mounted() {
      this.getstudentlist();
    },
    methods: {
      handleinfo(row) {
        var id = row.number;
        var params = {
          id: id
        };
        this.$router.push({
          path: "/list",
          // name: 'mallList',
          query: {
            id: "id"
          }
        });
      },
      onSubmit() { },
      resetSearch() {
        this.endTime = 99999999999;
        this.startTime = 0;
        this.form.month = "";
      },

      add0(m) {
        return m < 10 ? "0" + m : m;
      },
      format(shijianchuo) {
        var shijianchuo = shijianchuo * 1000;
        //shijianchuo是整数，否则要parseInt转换
        var time = new Date(shijianchuo);

        var y = time.getFullYear();
        var m = time.getMonth() + 1;
        var d = time.getDate();
        var h = time.getHours();
        var mm = time.getMinutes();
        var s = time.getSeconds();
        return (
          y +
          "-" +
          this.add0(m) +
          "-" +
          this.add0(d) +
          " " +
          this.add0(h) +
          ":" +
          this.add0(mm) +
          ":" +
          this.add0(s)
        );
      },
      handleCurrentChange(val) {
        this.currentPage = val;
        this.getstudentlist();
      },

      getstudentlist() {
        var params = {
          page: this.currentPage,
          limit: this.pageSize
        };
        this.axios
          .get("/public/index.php/channelStati", { params: params })
          .then(res => {
            console.log(res);
            this.tableData = res.data.list;
            this.total = res.data.list.total;
            this.todayprocess = res.data.todayprocess; //今天进程
            this.yesprocess = res.data.yesprocess; //昨天进程
            this.totalchannel = res.data.totalchannel; //总渠道
            this.channel_active = res.data.channel_active; //活跃渠道
            this.todayterminal = res.data.todayterminal; //今日终端
            this.yesterminal = res.data.yesterminal; //昨日终端
          })
          .catch(err => {
            console.error(err);
          });
      }
    }
  };
</script>
<style lang="">
</style>