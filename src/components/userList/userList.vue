<template>
	<div>
		<el-form :inline="true" :model="tableList" class="demo-form-inline query-form">
		
			<el-form-item label="部门名称:">
				<el-select v-model="tableList.bumenname" placeholder=""> 
				    <el-option
				      v-for="(item,index) in bumenList"
				      :key="index"
				      :label="item[index]"
				      :value="item[index]" >
				     </el-option>
				</el-select>
			</el-form-item> 
			<el-form-item label="状态">
			     <el-select v-model="tableList.zhuangtai" placeholder="请选择" >
				    <!-- <el-option
				      v-for="item in selectList"
				      :key="item.key"
				      :label="item.label"
				      :value="item.value" >
				    </el-option> -->
					 <el-option
				      v-for="(item,index) in selectList"
				      :key="index"
				      :label="item[index]"
				      :value="item[index]" >
				     </el-option>

				</el-select>
		    </el-form-item>
		  <el-form-item label="卡号:">
				<el-input v-model="tableList.kahao" placeholder=""></el-input>
			</el-form-item>
              <el-form-item label="姓名:">
				<el-input v-model="tableList.xingming" placeholder=""></el-input>
			</el-form-item>

	     	<div class="query_button">
	      		<el-form-item>
	                <el-button type="primary" @click="getSarch(tableList.bumenname,tableList.kahao,tableList.zhuangtai,tableList.xingming)" >查询</el-button>
	       			<el-button  @click="setReset">重置</el-button>
		       </el-form-item>


	      	</div>
		</el-form>
		<div class="table"> 
				<el-row class="container">
					<el-col :span='24' >
						<el-row class="all-classify">
							<el-col :span='20' >
							</el-col>
							<el-col :span='2'>
								<el-button type="info" size="mini" @click="exportFn()">导出</el-button>
							</el-col>
							<el-col :span='2'>
								<el-button type="success" size="mini" @click="addFn">新建</el-button>
							</el-col>
						</el-row>
					</el-col>
				</el-row>
	          <!--  <el-table :data="tableDatas"  border style="width: 100%;" v-loading="loading" @row-click="changeFn(scope.row.bianhao,'2',scope.row.cardFlag)">
	           -->
			    <el-table :data="tableDatas"  border style="width: 100%;" v-loading="loading" @row-click="changeFn">
				 
				    <el-table-column  align="center"   prop="xuhao" label="序号">  </el-table-column>
				    <el-table-column   align="center"  prop="zhenghao" label="卡号">  </el-table-column>
				    <el-table-column   align="center"  prop="xingming" label="姓名" >  </el-table-column>
				    <el-table-column  align="center"   prop="xingbie" label="性别">  </el-table-column>
				    <el-table-column  align="center"   prop="bumen" label="部门">  </el-table-column>
				    <el-table-column  align="center"   prop="zhiwu" label="职务">  </el-table-column>
				    <el-table-column  align="center"   prop="zhicheng" label="职称">  </el-table-column>
				    <el-table-column  align="center"   prop="zhuangtai" label="状态">  </el-table-column>
				</el-table>
				<div class="TotalRecords RedColor">
					共{{count}}记录
				</div>
				 <el-pagination class="pagination"  @current-change="handleCurrentChange" background layout="prev, pager, next , jumper"  :current-page.sync="tableList.pageIndex" :total="totalCount" > 
			</el-pagination> 
		</div>
		
	</div>
</template>
<script>
	import qs from 'qs'
	import Vue from 'vue'
	import router from '@/router'
	import {localHostUrl,getUserList,BumenZhuangtaiSelect} from '../../api/api.js'
	import {setCookie,getCookie,delCookie} from '../../cookie/cookie.js'
	export default{
		data(){
			return{
				oldStatus:'',
				bumenStatus:'',
				selectList:[],
				bumenList:[],
				totalCount:1,
				loading: true,
				tableList:{
					bumenname:'',
					zhuangtai:'',
                    xingming:'',
                    kahao:'',
                    pageIndex:1,
                    pageNum:10
				},
				tableDatas:[],//列表的值
				cardFlag:'',
				params:"",
				count:''
				//  zhuangtaiSelect:[],
       			//  bumenSelect:[]
			}
		},
		methods:{
            getSarch(bumenStatus,hakao,zhuangtai,xingming){
				this.getTablelist()
			},
			getUserList(realName,status,userName){
				let key='';
				this.selectList.forEach(function(items){
					if(items.value == status){
						key=items.key
					}
				})

				this.tableList.realName=realName
				this.tableList.userName=userName
				this.tableList.status=key
				this.getTablelist()
			},
			//点击哪页触发的时间
			handleCurrentChange(val) {
				this.tableList.pageIndex=val
				this.getTablelist()
	   			// console.log(`当前页: ${val}`);
	        },
			getTablelist(){
				getUserList(this.tableList).then((data) => {
					if(data.code==1){
						this.count=data.pageSum.count
						this.tableDatas=data.data
						this.totalCount=data.pageSum.pageNum
						// setCookie('userListData',this.tableDatas,1000*60)
						this.params=data.params

						
					}else{
						this.tableDatas=[]
						alert("没有数据")
					}
					this.loading=false
				}).catch(message => {
					this.$message.error("请求失败，请联系客服，失败码"+message);
					 this.loading=false
				})
			},
			 getSelectList(){
				BumenZhuangtaiSelect({method:"bumen"}).then((data) =>{
					if(data.code===1){
						this.bumenList=data.data
					}else{
						this.bumenList=[]
					}
				})
				BumenZhuangtaiSelect({method:"zhuangtai"}).then((data) =>{
					if(data.code===1){
						this.selectList=data.data
					}else{
						this.selectList=[]
					}
				})
			},
			//（1:添加 2:修改 9:删除）
			changeFn(row){
				let bianhao = row.bianhao
				let cardFlag = row.cardFlag
				if(cardFlag==1){
					this.$router.push({path:'/userListChange',query:{bianhao:bianhao,AddOrChange:2,params:this.params}})
				}else{
					alert("暂时不支持临时卡")
				}

			},
			addFn(row){
				this.$router.push({path:'/userListChange',query:{AddOrChange:1}})
			},
			setReset(){
				this.tableList.bumenname=''
				this.tableList.zhuangtai=''
				this.tableList.kahao=''
				this.tableList.xingming=''
				
			},
			exportFn(settId,pageIndex){
				var url = `${localHostUrl}exportBillBySettId.json?settId=${settId}`
				window.location.href=url
	        }
		},
		mounted(){
			this.getTablelist();
			this.getSelectList();
		}
	}
</script>