<template>
  <div class="conntent" style="margin-top:0.5rem;">
    <HeaderSame :headerObj="headerObj"></HeaderSame>
    <div class="delatedetail" @click="delatelist">
      <img src="../assets/img/3314.png" alt>
    </div>
    <div>
      <div class="toptitle">轴信息</div>
      <div class="detailinfo">
        <div>
          <div>上浆单</div>
          <span>{{getDatalist.entity.WarpRisingCode}}</span>
        </div>
        <div>
          <div>整经轴号(输出)</div>
          <span>{{getDatalist.entity.BeamCode}}</span>
        </div>
        <div>
          <div>经轴长度</div>
          <span>{{getDatalist.entity.Length}}</span>
        </div>
        <div>
          <div>套扣个数</div>
          <span>{{getDatalist.entity.STCount}}</span>
        </div>
        <div>
          <div>备注</div>
          <span>{{getDatalist.entity.Remark}}</span>
        </div>
      </div>
    </div>
    <div v-for="(tag,index) in getDatalist.emps" :key="index">
      <div class="toptitle">责任人{{index+1}}</div>
      <div class="detailinfo">
        <div>
          <div>责任人</div>
          <span>{{tag.EmpName}}</span>
        </div>
        <div>
          <div>开始时间</div>
          <span>{{tag.BeginTime}}</span>
        </div>
        <div>
          <div>结束时间</div>
          <span>{{tag.EndTime}}</span>
        </div>
        <div>
          <div>米数</div>
          <span>{{tag.Lenght}}</span>
        </div>
        <div>
          <div>索套个数</div>
          <span>{{tag.DoNum}}</span>
        </div>
        <div>
          <div>班别</div>
          <span>{{tag.ClassBanName}}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import HeaderSame from "./common/sameHeader.vue";
export default {
  components: { HeaderSame },
  name: "applydetail",
  data() {
    return {
      headerObj: {
        title: "并轴工单详情",
        img: "",
        text: ""
      },
      getDatalist: {
        entity: [],
        emps: []
      }
    };
  },
  methods: {
    // 删除
    delatelist: function() {
      this.$confirm("是否确认删除？", "", {
        // confirmButtonText: '确定',
        // cancelButtonText: '取消',
        type: ""
      })
        .then(() => {
          this.$axios({
            method: "post",
            url:
               "/WarpingOrder/DelWarpReBeamDetail",
            data: {
              id: this.$route.query.id
            }
          })
            .then(res => {
              // console.log(res);
              this.$router.push("bingzhou");
              this.$message({
                showClose: true,
                message: "删除成功",
                type: "success",
                center: true
              });
            })
            .catch(error => {
              console.log(error);
            });
        })
        .catch(() => {
          this.$message({
            showClose: true,
            type: "info",
            message: "已取消删除"
          });
        });
    },
    getData: function() {
      this.$axios({
        method: "post",
        url:
           "/WarpingOrder/GetReBeamDetailDataByID",
        data: {
          id: this.$route.query.id
        }
      })
        .then(res => {
          console.log(res.data.data);
          this.getDatalist.entity = res.data.data.entity;
          this.getDatalist.emps = res.data.data.emps;
        })
        .catch(error => {
          console.log(error);
        });
    }
  },
  created() {
    this.getData();
  }
};
</script>

<style lang="less" scoped>
.delatedetail {
  position: absolute;
  margin-left: 3.25rem;
  margin-top: -0.38rem;
  z-index: 99999;
  > img {
    height: 0.2rem;
  }
}
.conntent {
  font-size: 0.17rem;
  .toptitle {
    // width: 3.6rem;
    height: 0.35rem;
    background: rgba(243, 243, 243, 1);
    opacity: 1;
    font-size: 0.15rem;
    font-family: Microsoft YaHei UI;
    padding-left: 0.15rem;
    font-weight: 400;
    line-height: 0.35rem;
    color: rgba(153, 153, 153, 1);
  }
  .detailinfo {
    > div:not(:last-child) {
      border-bottom: 0.01rem solid #d5d5d5;
    }
    > div {
      height: 0.5rem;
      line-height: 0.5rem;
      //   width: 3.3rem;
      padding: 0 0.15rem 0 0;
      margin-left: 0.15rem;
      > div {
        //   width: 2.7rem;
        height: 0.5rem;
        display: inline-block;
        //   background: red;
        color: #999;
        font-family: Microsoft YaHei UI;
      }
      > span {
        float: right;
      }
    }
  }
}
</style>