<template>
  <div class="bg1">
    <HeaderSame :headerObj="headerObj"></HeaderSame>
    <div class="basic">
      <div>
        <div>经轴编号</div>
        <div>
          <input type="text" placeholder="请输入" class="jzbianhao" v-model="shaDetails.BeamCode">
        </div>
      </div>
      <div>
        <div>断纱</div>
        <div>
          <input type="text" placeholder="请输入" class="duansha" v-model="shaDetails.YarnBroken">
        </div>
      </div>
      <div>
        <div>毛羽数</div>
        <div>
          <input type="text" placeholder="请输入" class="maoyunum" v-model="shaDetails.BeamHairinessNum">
        </div>
      </div>
      <div>
        <div>对头次数</div>
        <div class="rt">请在责任人中输入</div>
      </div>
      <div id="remark">
        <div>备注</div>
        <div>
          <textarea class="beizhu" v-model="shaDetails.Remark"></textarea>
        </div>
      </div>
    </div>
    <div class="add-item" v-for="(i , index)  in fuzeersLists">
      <p>责任人{{index+1}}</p><div class="delateitem" @click="delateitem(index,'1')">删除</div>
      <div class="basic" id="addsDetail">
        <div>
          <div>责任人</div>
          <div class="rt">
            <select class="zrpeople">
              <option :value="i.EmpID">{{i.EmpName}}</option>
              <option v-for="(item,tag) in empList" :value="item.Value" :key="tag">{{item.Text}}</option>
            </select>
            <img src="../assets/img/819.png">
          </div>
        </div>
        <div>
          <div id="tongNum">米数</div>
          <div class="rt">
            <input type="text" placeholder="请输入" class="pemilength" v-model="i.Lenght">
          </div>
        </div>
        <div>
          <div id="tongNum">对头次数</div>
          <div class="rt">
            <input type="text" placeholder="请输入" class="duitou" v-model="i.DoNum">
          </div>
        </div>
        <div>
          <div>班别</div>
          <div class="rt">
            <select class="classBan">
              <option :value="i.ClassBan">{{i.ClassBanName}}</option>
              <option v-for="(item,flag) in drpList" :value="item.Value" :key="flag">{{item.Text}}</option>
            </select>
            <img src="../assets/img/819.png">
          </div>
        </div>
        <div>
          <div>开始时间</div>
          <div class="rt">
            <el-date-picker
              v-model="startRadios[index]"
              type="datetime"
              size="small"
              placeholder="选择日期"
              class="startTime"
              value-format="yyyy-MM-dd HH:mm"
              :editable="false"
            ></el-date-picker>
          </div>
        </div>
        <div style="border-bottom: none;">
          <div>结束时间</div>
          <div class="rt">
            <el-date-picker
              v-model="endRadios[index]"
              type="datetime"
              size="small"
              placeholder="选择日期"
              class="startTime"
              value-format="yyyy-MM-dd HH:mm"
              :editable="false"
            ></el-date-picker>
          </div>
        </div>
      </div>
    </div>
    <div class="add-item" v-for="(i , index)  in num" :key="index">
      <p>责任人{{index+fuzeersLists.length+1}}</p><div class="delateitem" @click="delateitem(index,'2')">删除</div>
      <div class="basic" id="addsDetail">
        <div>
          <div>责任人</div>
          <div class="rt">
            <select class="zrpeople">
              <option v-for="(item,tag) in empList" :value="item.Value" :key="tag">{{item.Text}}</option>
            </select>
            <img src="../assets/img/819.png">
          </div>
        </div>
        <div>
          <div id="tongNum">米数</div>
          <div class="rt">
            <input type="text" placeholder="请输入" class="pemilength">
          </div>
        </div>
        <div>
          <div id="tongNum">对头次数</div>
          <div class="rt">
            <input type="text" placeholder="请输入" class="duitou">
          </div>
        </div>
        <div>
          <div>班别</div>
          <div class="rt">
            <select class="classBan">
              <option v-for="(item,flag) in drpList" :value="item.Value" :key="flag">{{item.Text}}</option>
            </select>
            <img src="../assets/img/819.png">
          </div>
        </div>
        <div>
          <div>开始时间</div>
          <div class="rt">
            <el-date-picker
              v-model="startRadios[index+fuzeersLists.length]"
              type="datetime"
              size="small"
              placeholder="选择日期"
              class="startTime"
              value-format="yyyy-MM-dd HH:mm"
              :editable="false"
            ></el-date-picker>
          </div>
        </div>
        <div style="border-bottom: none;">
          <div>结束时间</div>
          <div class="rt">
            <el-date-picker
              v-model="endRadios[index+fuzeersLists.length]"
              type="datetime"
              size="small"
              placeholder="选择日期"
              class="startTime"
              value-format="yyyy-MM-dd HH:mm"
              :editable="false"
            ></el-date-picker>
          </div>
        </div>
      </div>
    </div>

    <div class="addNew" @click="aa">
      <img src="../assets/img/3893.png">
    </div>
    <div class="posit" @click="confirms">
      <p>确定</p>
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
        title: this.$route.query.handle == 'add'?"整经-新增轴":"整经-编辑",
        img: "",
        text: "addZhou"
      },
      shaDetails:[],
      fuzeersLists:[],
      num: this.$route.query.handle == 'add'?[1]:[],
      empList: [],
      drpList: [],
      startRadios: [],
      endRadios: []
    };
  },
  methods: {
    aa: function() {
       var i =1;
      this.num.push(i);
    },
    // 删除责任人
    delateitem:function(index,tag){
      this.$confirm('确定删除此责任人吗？', '',{
          // confirmButtonText: '确定',
          // cancelButtonText: '取消',
          // type: 'warning'
        }).then(() => {
          this.$message({
            showClose: true,
            type: 'success',
            message: '删除成功!'
          });
          if(tag == '1'){
            this.startRadios.splice(index, 1);
            this.endRadios.splice(index, 1);
            this.fuzeersLists.splice(index,1);
          }else if(tag == '2'){
             this.startRadios[index+this.fuzeersLists.length] == ""? "": this.startRadios.splice(index+this.fuzeersLists.length, 1);
             this.endRadios[index+this.fuzeersLists.length] == "" ? "" : this.endRadios.splice(index+this.fuzeersLists.length, 1);
            this.num.splice(index,1);
          }
          
          console.log(this.startRadios[index]);
          // if(this.$route.query.handle == "add"){
          //    this.startRadios[index] == ""? "": this.startRadios.splice(index, 1);
          //    this.endRadios[index] == "" ? "" : this.endRadios.splice(index, 1);
          // }else{
          //   this.startRadios.splice(index, 1);
          //   this.endRadios.splice(index, 1);
          // }
    
        }).catch(() => {
          this.$message({
            showClose: true,
            type: 'info',
            message: '已取消删除'
          });          
        });
        
    },
    confirms: function() {
    //先移除点击日期控件创建的干扰元素
    $('.el-picker-panel').remove();
    //循环遍历判断这个日期控件使用的input值
		if($(".jzbianhao").val() == "" || $(".duansha").val() == "" || $(".maoyunum").val() == "" ){
				this.$message({
					showClose: true,
					message: "请完善信息",
					type: "error",
					center: true
				});
		}else{
			for(var i=0;i<$('.add-item').length;i++){
        console.log($('.classBan')[i].value,11);
				if($('.zrpeople')[i].value == "" || $('.pemilength')[i].value==""|| $('.duitou')[i].value=="" || $("input.el-input__inner[readonly='readonly']")[2*i].value == "" || $("input.el-input__inner[readonly='readonly']")[2*i+1].value == "" || $('.classBan')[i].value == '' || $('.classBan')[i].value == "--请选择--"){
					this.$message({
						showClose: true,
						message: "请完善信息",
						type: "error",
						center: true
					});
					return;
				}
			}
			//不为空之后
			let entity = {};
        entity.WarpOrderID = localStorage.getItem("zjID");
        entity.WarpOrderCode = localStorage.getItem("zjCODE");
        entity.BeamCode = $(".jzbianhao").val();
        entity.YarnBroken = $(".duansha").val();
        entity.BeamHairinessNum = $(".maoyunum").val();
        entity.Remark = $(".beizhu").val();
        this.$route.query.handle == "edit"
          ? (entity.ID = this.$route.query.id)
          : "";

        let emps = [];

        for (let i = 0; i < $(".add-item").length; i++) {
          let emplisters = {};

          emplisters.EmpID = $(".zrpeople")
            .eq(i)
            .val();
          emplisters.EmpName = $(".zrpeople option:selected")
            .eq(i)
            .text();
          emplisters.Lenght = $(".pemilength")
            .eq(i)
            .val();
          emplisters.DoNum = $(".duitou")
            .eq(i)
            .val();
          emplisters.BeginTime = this.startRadios[i];
          emplisters.EndTime = this.endRadios[i];
          emplisters.ClassBan = $(".classBan")
            .eq(i)
            .val();
          emplisters.ClassBanName = $(".classBan option:selected")
            .eq(i)
            .text();

          emps.push(emplisters);
        }
        this.$axios({
          method: "post",
          url:  "/WarpingOrder/SaveWarpingDetail",
          data: {
            entity: entity,
            empjson: emps
          }
        })
          .then(res => {
            console.log(res);
            this.$message({
              showClose: true,
              message:
                this.$route.query.handle == "add" ? "新增成功" : "修改成功",
              type: "success",
              center: true
            });
            this.$router.push("zhengJ");
          })
          .catch(error => {
            console.log(error);
          });
		}
		
    },
    tzConfirmerList: function() {
      //筒子确认者列表
      this.$axios({
        method: "post",
        url:  "/WarpingOrder/GetEmpDropDownList"
      })
        .then(res => {
          // console.log(res);
          this.empList = res.data.data;
        })
        .catch(error => {
          console.log(error);
        });
      //垫圈确认者列表
      this.$axios({
        method: "post",
        url:  "/WarpingOrder/GetBShiftDrpDownList"
      })
        .then(res => {
          // console.log(res);
          this.drpList = res.data.data;
        })
        .catch(error => {
          console.log(error);
        });
    }
  },
  created:function() {
    this.tzConfirmerList();
    if(this.$route.query.handle == 'edit'){
				this.$axios({
					method: 'post',
					url:  '/WarpingOrder/GetWarpingDetailByID',
					data:{
						id:this.$route.query.id
					}
				}).then((res) => {
					// console.log(res);
					this.shaDetails = res.data.data;
          this.fuzeersLists = res.data.emps;
        
          for(let i = 0; i < this.fuzeersLists.length; i++){
            this.startRadios[i] = this.fuzeersLists[i].BeginTime;
            this.endRadios[i] = this.fuzeersLists[i].EndTime
          }
				}).catch((error) => {
					console.log(error);
				});
			}
  },
  mounted(){
    $('#app').css('overflow-y','auto')
  },
  updated(){
    if($('.add-item').length == '1'){
      $('.delateitem:eq(0)').addClass("disdelate");
    }else{
      $('.delateitem:eq(0)').removeClass("disdelate");
    }
  }
};
</script>

<style scoped lang="less">
.disdelate{
  display: none;
}
.bg1 {
  // position: relative;
  font-size: 0.17rem;
  height: auto;
  min-height: 100%;
  padding-bottom: 0.7rem;
  .delateitem{
    position: relative;
    margin-top: -0.3rem;
    margin-left: 3rem;
    color: #666;
    font-size: 0.15rem;
    padding-bottom: 0.1rem
  }
  .basic {
    background-color: white;
    line-height: 0.5rem;
    padding-left: 0.15rem;
    > div {
      height: 0.5rem;
      border-bottom: 1px solid #d5d5d5;
    }
    > div div {
      display: inline-block;
      color: #474747;
    }
    > div div:first-child {
      width: 1rem;
      color: #474747;
    }
    input {
      width: 2.25rem;
      height: 0.35rem;
      line-height: 0.35rem;
      padding-left: 0.07rem;
      font-size: 0.17rem;
      border: none;
      outline: none;
      background-color: #f3f3f3;
      color: #999;
    }
    select {
      vertical-align: middle;
      outline: none;
      border: none;
      white-space: nowrap;
      appearance: none;
      -moz-appearance: none;
      -webkit-appearance: none;
      text-indent: 0.02rem;
      background-color: transparent;
      font-size: 0.17rem;
      color: #999;
    }
    img {
      margin-right: 0.15rem;
      width: 0.09rem;
      height: 0.17rem;
    }
    textarea {
      width: 2.25rem;
      height: 0.75rem;
      margin-top: 0.15rem;
      margin-bottom: 0.15rem;
      outline: none;
      padding-left: 0.05rem;
      padding-top: 0.05rem;
      resize: none;
      border: none;
      background-color: #f3f3f3;
      color: #999999;
    }
  }
}
.posit {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  height: 0.5rem;
  line-height: 0.5rem;
  text-align: center;
  background-color: #007eff;
  color: white;
}
.addNew {
  text-align: center;
  margin-top: 0.15rem;
  img {
    width: 1.65rem;
  }
}
.add-item {
  width: 3.4rem !important;
  margin: 0 auto;
  p {
    margin: 0.15rem 0 0.1rem 0.05rem;
    color: #999999;
    font-size: 0.15rem;
  }
  input {
    width: 2.1rem !important;
    margin-right: 0.15rem;
  }
  #tongNum {
    width: 0.9rem;
  }
}
#remark {
  height: 1.1rem;
  border: none;
  div {
    vertical-align: top;
    display: inline-block;
    height: 1.1rem;
  }
}
.startTime {
  width: 2.2rem !important;
  margin-left: -2.35rem;
}
</style>