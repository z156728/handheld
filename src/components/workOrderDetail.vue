<template>
	<div class="bg1">
		<HeaderSame :headerObj="headerObj"></HeaderSame>
		<div class="tablist">
            <div :class="{active:0==isactive}" @click="details(0)">工单信息</div>
            <div :class="{active:1==isactive}" @click="details(1)">整经条件</div>
            <div :class="{active:2==isactive}" @click="details(2)">上浆条件</div>
        </div>
        
        <!--工单信息-->
        <div v-if="gongdan">
	    	<div class="zhengjing">
	    		<p>整经信息</p>
	    	</div>
	        <div class="list-detail">
	        	<div>
	        		<span>发行日期</span>
	        		<span class="rt">{{gdList.Codedate}}</span>
	        	</div>
	        	<div>
	        		<span>发行者</span>
	        		<span class="rt">{{gdList.PublisheUser}}</span>
	        	</div>
	        	<div>
	        		<span>整经编号</span>
	        		<span class="rt">{{gdList.Code}}</span>
	        	</div>
	        	<div>
	        		<span>品名</span>
	        		<span class="rt">{{gdList.PNo}}</span>
	        	</div>
	        	<!-- <div>
	        		<span>筒子个数</span>
	        		<span class="rt">{{gdList.CheesNum}} 个</span>
	        	</div> -->
	        	<div>
	        		<span>卷取长度</span>
	        		<span class="rt">{{gdList.RollLength}} 米</span>
	        	</div>
	        	<div>
	        		<span>经纱总根数</span>
	        		<span class="rt">{{gdList.YarnNum}} 根</span>
	        	</div>
	        	<div style="border: 0;">
	        		<span>并轴个数</span>
	        		<span class="rt">{{gdList.SpoolNum}} 个</span>
	        	</div>
	        </div>
        	<div class="zhengjing workers">
	    		<p>对头作业者</p>
	    	</div>
	        <div class="list-detail">
	        	<div style="border: 0;">
	        		<span>东侧一排   <span> {{gdList.EFCheckThreaderName}}</span></span>
	        		<span class="rt colors">西侧一排 <span> {{gdList.WFCheckThreaderName}}</span></span>
	        	</div>
	        </div>
	        <div class="zhengjing workers">
	    		<p>筒子确认者</p>
	    	</div>
	        <div class="list-detail">
	        	<div style="border: 0;">
	        		<span>东侧一排   <span> {{gdList.EFCheeseCheckerName}}</span></span>
	        		<span class="rt colors">西侧一排 <span> {{gdList.WFCheeseCheckerName}}</span></span>
	        	</div>
	        </div>
        </div>
        
        <!--整经条件-->
        <div v-if="zhengjing">
	    	<div class="zhengjing">
	    		<p>整经条件</p>
	    	</div>
	        <div class="list-detail">
	        	<div>
	        		<span>条件设定者</span>
	        		<span class="rt">{{zjList.ConditionSetUserName}}</span>
	        	</div>
	        	<div>
	        		<span>条件确认者</span>
	        		<span class="rt">{{zjList.ConditionCheckUserName}}</span>
	        	</div>
	        	<div>
	        		<span>白色垫圈</span>
	        		<span class="rt">{{zjList.CheeseNum}} 个</span>
	        	</div>
	        	<div>
	        		<span>筒子张力器位置</span>
	        		<span class="rt">{{zjList.TensionerPosition}}</span>
	        	</div>
	        	<div>
	        		<span>卷取速度</span>
	        		<span class="rt">{{zjList.RollingSpeed}} m/min</span>
	        	</div>
	        	<div>
	        		<span>总卷取张力</span>
	        		<span class="rt">{{zjList.ToatalRollingtension}} kg</span>
	        	</div>
	        	<div>
	        		<span>单纱卷取张力</span>
	        		<span class="rt">{{zjList.YarnRollingtension}} g</span>
	        	</div>
	        	<div>
	        		<span>单纱筒子卷取张力</span>
	        		<span class="rt">{{zjList.BobbinFrameTension}} g</span>
	        	</div>
	        	<!-- <div>
	        		<span>总长度</span>
	        		<span class="rt">{{zjList.TotalLength}} m</span>
	        	</div>
	        	<div>
	        		<span>总羽毛数</span>
	        		<span class="rt">{{zjList.TotalHairiness}} 个</span>
	        	</div> -->
	        	<div>
	        		<span>卷取张力按钮刻度</span>
	        		<span class="rt">{{zjList.RollTButScale}}</span>
	        	</div>
	        	<div style="border: 0;">
	        		<span>经轴压纱辊压力</span>
	        		<span class="rt">{{zjList.GauzerollerPress}} kg</span>
	        	</div>
	        </div>
        </div>
        
        <!--上浆单-->
        <div v-if="shangjiang">
	    	<div class="zhengjing">
	    		<p>上浆并轴条件</p>
	    	</div>
	        <div class="list-detail">
	        	<section class="conditions-list">
	        		<div v-for="(item,index) in SizingCodes" @click="sjbztj(item.ID,index)" :key="index" class="selectdiv">{{item.Code}}</div>
	        	</section>
	        	<div>
	        		<span>条件设定者</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.SettingUserName}}</span>
	        	</div>
	        	<div>
	        		<span>条件确认者</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.CheckUserName}}</span>
	        	</div>
	        	<div class="halfs">
	        		<div>
	        			<span>卷取速度</span>
	        			<span class="rt">{{Sizingcondition == null?'':Sizingcondition.RollingSpeed}} m/min</span>
	        		</div>
	        		<div style="border-right: none;">
	        			<span style="margin-left: .15rem;">第一锡林</span>
	        			<span class="rt">{{Sizingcondition == null?'':Sizingcondition.FirstCylinder}} ℃</span>
	        		</div>
	        	</div>
	        	<div class="halfs">
	        		<div>
	        			<span>第一烘箱</span>
	        			<span class="rt">{{Sizingcondition == null?'':Sizingcondition.Firstoven}} ℃</span>
	        		</div>
	        		<div style="border-right: none;">
	        			<span style="margin-left: .15rem;">第二锡林</span>
	        			<span class="rt">{{Sizingcondition == null?'':Sizingcondition.SecondCylinder}} ℃</span>
	        		</div>
	        	</div>
	        	<div class="halfs">
	        		<div>
	        			<span>第二烘箱</span>
	        			<span class="rt">{{Sizingcondition == null?'':Sizingcondition.Secondoven}} ℃</span>
	        		</div>
	        		<div style="border-right: none;">
	        			<span style="margin-left: .15rem;">第三锡林</span>
	        			<span class="rt">{{Sizingcondition == null?'':Sizingcondition.ThirdCylinder}} ℃</span>
	        		</div>
	        	</div>
	        	<div class="halfs">
	        		<div>
	        			<span>浆料温度</span>
	        			<span class="rt">{{Sizingcondition == null?'':Sizingcondition.Slurrytemper}} ℃</span>
	        		</div>
	        		<div style="border-right: none;">
	        			<span style="margin-left: .15rem;">第四锡林</span>
	        			<span class="rt">{{Sizingcondition == null?'':Sizingcondition.ForthCylinder}} ℃</span>
	        		</div>
	        	</div>
	        	<div class="halfs last-halfs">
	        		<div>
	        			<span>温水温度</span>
	        			<span class="rt">{{Sizingcondition == null?'':Sizingcondition.Watertemper}} ℃</span>
	        		</div>
	        		<div style="border-right: none;">
	        			<span style="margin-left: .15rem;">第五锡林</span>
	        			<span class="rt">{{Sizingcondition == null?'':Sizingcondition.FifthCylinder}} ℃</span>
	        		</div>
	        	</div>
	        </div>
	        <div class="zhengjing">
	    		<p>后上油</p>
	    	</div>
	        <div class="list-detail">
	        	<div>
	        		<span>使用油剂</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.OilType}}</span>
	        	</div>
	        	<div>
	        		<span>油温</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.Oiltemper}}</span>
	        	</div>
	        	<div style="border-bottom: 0;">
	        		<span>后上油蜡辊转速</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.Rollerspeed}} rpm</span>
	        	</div>
	        </div>
	        <div class="zhengjing">
	    		<p>后上蜡</p>
	    	</div>
	        <div class="list-detail">
	        	<div>
	        		<span>使用蜡名</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.WaxType}}</span>
	        	</div>
	        	<div>
	        		<span>蜡温</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.Waxtemper}}</span>
	        	</div>
	        	<div>
	        		<span>压浆辊压力</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.PRollerpress}} kg</span>
	        	</div>
	        	<div>
	        		<span>SD部分压力</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.SDPartTension}} kg</span>
	        	</div>
	        	<div>
	        		<span>浸没辊压力</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.IRollerpress}} kg</span>
	        	</div>
	        	<div>
	        		<span>卷取张力</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.RollingTension}} kg</span>
	        	</div>
	        	<div>
	        		<span>上浆部分拉伸</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.SizingLift}}</span>
	        	</div>
	        	<div>
	        		<span>经轴最后卷径</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.WarpLastRadius}} cm</span>
	        	</div>
	        	<div>
	        		<span>烘箱部分拉升</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.OvenLift}}</span>
	        	</div>
	        	<div>
	        		<span>梳子上浆料附着</span>
	        		<span class="rt" v-if="Sizingcondition == null?'':Sizingcondition.IsSizeAdhere == 0">是</span>
					<span class="rt" v-if="Sizingcondition == null?'':Sizingcondition.IsSizeAdhere == 1">否</span>
	        		<span class="rt" v-if="Sizingcondition== null || Sizingcondition.IsSizeAdhere == null || Sizingcondition.IsSizeAdhere == ''">是/否</span>
	        	</div>
	        	<div>
	        		<span>上浆率</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.SizingRate}} %</span>
	        	</div>
	        	<div>
	        		<span>烘箱出口经纱张力</span>
	        		<span class="rt">
	        			<span><span id="reds">左:</span> {{Sizingcondition == null?'':Sizingcondition.OYTL}}</span>&nbsp;
	        			<span><span id="reds">中:</span> {{Sizingcondition == null?'':Sizingcondition.OYTC}}</span>&nbsp;
	        			<span><span id="reds">右:</span> {{Sizingcondition == null?'':Sizingcondition.OYTR}}</span>
	        		</span>
	        	</div>
	        	<div style="border-bottom: 0;">
	        		<span>浆料配方</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.Sizinginstruction}}</span>
	        	</div>
	        </div>
	        
	        <div class="zhengjing">
	    		<p>并轴条件</p>
	    	</div>
	        <div class="list-detail">
	        	<div>
	        		<span>条件设定者</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.ConSetUserName}}</span>
	        	</div>
	        	<div>
	        		<span>条件确认者</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.ConCheckUserName}}</span>
	        	</div>
	        	<div>
	        		<span>卷取速度</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.RollSpeed}} m/min</span>
	        	</div>
	        	<div>
	        		<span>经轴幅宽</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.BeamWidth}}</span>
	        	</div>
	        	<div>
	        		<span>匹长</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.Length}} m</span>
	        	</div>
	        </div>
	        <div class="zhengjing">
	    		<p>并轴经轴架</p>
	    	</div>
	        <div class="list-detail">
	        	<div>
	        		<span>经轴个数</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.BeamNum}} 个</span>
	        	</div>
	        	<div>
	        		<span>经轴最初卷径</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.BeamBRadius}} cm</span>
	        	</div>
	        	<div>
	        		<span>维度(电器粗刻度)</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.Dimension}}</span>
	        	</div>
	        </div>
	        <div class="zhengjing">
	    		<p>卷曲部分</p>
	    	</div>
	        <div class="list-detail">
	        	<div>
	        		<span>卷取张力</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.BeamRollingTension}} kg</span>
	        	</div>
	        	<div>
	        		<span>单个经轴张力</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.BeamTension}} kg</span>
	        	</div>
	        	<div>
	        		<span>经轴单纱张力</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.YarnTension}} g</span>
	        	</div>
	        	<div>
	        		<span>其他备注</span>
	        		<span class="rt">{{Sizingcondition == null?'':Sizingcondition.Remark}}</span>
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
				title: "工单详情",
				img: "",
				text: ""
			},
            isactive:0,
            gongdan:true,
            zhengjing:false,
            shangjiang:false,
            gdList:{},
            zjList:{},
            SizingCodes:[],
            Sizingcondition:{}
          }
		},
		mounted(){
		   $('#app').css('overflow-y','auto')
		},
        methods:{
        	details:function(index){
        		this.isactive=index
        		if(this.isactive == 0){
        			this.gongdan = true;
        			this.zhengjing = false;
        			this.shangjiang = false;
        		}else if(this.isactive == 1){
        			this.gongdan = false;
        			this.zhengjing = true;
        			this.shangjiang = false;
        		}else if(this.isactive == 2){
        			this.gongdan = false;
        			this.zhengjing = false;
        			this.shangjiang = true;
        		}
        		
			},
			// 上浆并轴条件跳转
			sjbztj:function(tag,index){
				$('.selectdiv:eq(0)').css('background-color','#fff');
				$('.selectdiv:eq(0)').css('color','#007EFF');
				$('.selectdiv').removeClass('selectspan');
				$('.selectdiv:eq('+index+')').addClass('selectspan');
				// console.log(tag);
                	this.$axios({
		      	    method: 'post',
		      	    url:  '/WarpingOrder/GetWarpSizingConditionByID',
		      	    data:{
		      	    	id:tag,
		      	    }
		      	}).then((res)=> {
		      		if(res.data.code == "0"){
						//   console.log(res.data.data);
						  this.Sizingcondition = res.data.data;
						  console.log(this.Sizingcondition)
		      		}
		      	}).catch((error)=> {
		      	    console.log(error);
		      	});
			},
        	listData:function(){
        		this.$axios({
		      	    method: 'post',
		      	    url:  '/WarpingOrder/GetWarpOrderByID',
		      	    data:{
		      	    	id:localStorage.getItem("zjID"),
						type:"1"
		      	    }
		      	}).then((res)=> {
		      		if(res.data.code == "0"){
		      			// console.log(res.data.data.WarpOrder);
		      			this.gdList = res.data.data.WarpOrder;
		      			this.zjList = res.data.data.WarpCondition;
		      		}
		      	}).catch((error)=> {
		      	    console.log(error);
		      	});
		      	
		      	this.$axios({
		      	    method: 'post',
		      	    url:  '/WarpingOrder/GetWarpSizingConditionByOrderid',
		      	    data:{
		      	    	orderid:localStorage.getItem("zjID")
		      	    }
		      	}).then((res)=> {
		      		if(res.data.code == "0"){
		      			console.log(res.data.data);
		      			this.SizingCodes = res.data.data.SizingCodes;
		      			this.Sizingcondition = res.data.data.Sizingcondition;
		      			
		      		}
		      	}).catch((error)=> {
		      	    console.log(error);
		      	});
		      	
		      	
        	}
        },
        created() {
			this.listData();
		},
    }  
</script>

<style scoped="scoped" lang="less">
	.selectspan{
		background-color: #007EFF!important;
		color: #fff!important;
	}
	.conditions-list>div:first-child{
		background-color: #007EFF;
		color: #fff;
	}
  .bg1{
	font-size: .17rem;
	.tablist{
    	border-bottom: 1px solid #f2f2f2;
        font-size: 0.17rem;
        color: #999999;
        background: #ffffff;
        display: flex;
		height: 0.45rem;
        justify-content: space-around;
      div{
        box-sizing: border-box;
        height: 0.45rem;
        line-height:0.45rem;
      }
	    .active{
	      border-bottom:2px solid #007EFF;
	      color: #007EFF;
	    }
    }
	.zhengjing{
		height: .35rem;
		line-height: .35rem;
		background-color: #F3F3F3;
		p{
			margin-left:.17rem;
			color: #999999;
			font-size: 0.15rem;
		}
	}
	.list-detail{
		background-color: white;
		padding-left: .17rem;
		>div{
			height: .5rem;
			line-height: .5rem;	
			border-bottom: 1px solid #D5D5D5;
			span:first-child{
				color: #999999;
			}
			span:last-child{
				color: #333;
			}
			.rt{
				margin-right: .17rem;
				span{
					color: #333!important;
				}
			}
		}
	}
}
.conditions-list{
	min-height:.6rem ;
	font-size: 0.15rem;
	border-bottom:1px solid #D5D5D5 ;
	div{
		margin-top: .17rem;
		width: 1rem;
		height: .29rem;
		line-height: .29rem;
		border-radius:1.36rem;
		text-align: center;
		color: #007EFF;
		display: inline-block;
		margin-right: .09rem;
		border: 1px solid #007EFF;
	}
	div:last-child{
		margin-bottom: .15rem;
	}
}
.halfs{
	display: flex;
	div{
		display: inline-block;
		flex: 1;
		border-right: 1px solid #D5D5D5;
		span:first-child{
			color: #999;
		}
		span:last-child{
			color: #333;
		}
	}
}
.last-halfs{
	border-bottom:0!important;
}
.colors{
	color: #999!important;
}
#reds{
	color: red!important;
}
</style>