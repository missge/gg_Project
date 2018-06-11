<template>
	<div>
		<el-form :inline="true" :model="tableList" class="demo-form-inline query-form">
			<el-form-item label=" 姓名:">
				<el-input v-model="tableList.xingming" placeholder=""></el-input>
			</el-form-item>
		    <el-form-item label=" 部门:">
				<el-input v-model="tableList.bumen" placeholder=""></el-input>
			</el-form-item>
		    <el-form-item label="时间">
				<el-date-picker v-model="tableList.startTime" type="datetime" placeholder="选择开始时间"  value-format="yyyy-MM-dd HH:mm:ss"> </el-date-picker>
		    	<span class="demonstration">至</span>
			    <el-date-picker  v-model="tableList.endTime" type="datetime"  placeholder="选择结束时间"  value-format="yyyy-MM-dd HH:mm:ss"> </el-date-picker>
				</el-form-item>
	     	<div class="query_button">
	      		<el-form-item>
	                <el-button type="primary" @click="getSarch(tableList.xingming,tableList.bumen,tableList.startTime,tableList.endTime)" >查询</el-button>
	           </el-form-item>
                 <el-button @click="setReset" >重置</el-button>

	      	</div>
		</el-form>
		<div class="table"> 
	            <el-table :data="tableDatas"  border style="width: 100%;"  v-loading="loading">
				    <el-table-column   prop="num" label="序号">  </el-table-column>
				    <el-table-column   prop="shijian" label="操作时间">  </el-table-column>
				    <el-table-column   prop="bianhao" label="编号">  </el-table-column>
				    <el-table-column   prop="xingming" label="姓名">  </el-table-column>
				    <el-table-column   prop="bumen" label="部门">  </el-table-column>
				    <el-table-column   prop="neirong" label="内容">  </el-table-column>
			 		<el-table-column label="相关操作" >
		          		<template slot-scope="scope" >
	           				<el-button size="small" @click="changeFn(scope.row.id)">查看</el-button>
	           				
	           			</template>
		       		</el-table-column>
				</el-table>
				<el-pagination class="pagination"  @current-change="handleCurrentChange" background layout="prev, pager, next , jumper"  :current-page.sync="tableList.pageIndex" :total="totalCount" :page-size="tableList.pageSize" >
				</el-pagination>
		</div>
		
	</div>
</template>
<script>
	import qs from 'qs'
	import Vue from 'vue'
	import router from '@/router'
	import {LiShiInfoLog} from '../../api/api.js'
	export default{
		data(){
			return{
				totalCount:1,
				loading: true,
				tableList:{
					xingming:'',
					bumen:'',
					startTime:'',
					endTime:'',
					pageIndex:1,
					pageSize:50
				},
				tableDatas:[],//列表的值
			}
		},
		methods:{
			//搜索接口
			getSarch(xingming,bumen,startTime,endTime){
				this.tableList.xingming=xingming
				this.tableList.bumen=bumen
				this.tableList.startTime=startTime
				this.tableList.endTime=endTime
				this.getTablelist()
			},
			//点击哪页触发的时间
			handleCurrentChange(val) {
				this.tableList.pageIndex=val
				this.getTablelist()
				console.log("功能日志"+this.tableList);
	   			// console.log(`当前页: ${val}`);
	        },
			getTablelist(){
				LiShiInfoLog(this.tableList).then((data) => {
					this.tableDatas=data.data
					this.totalCount=data.pageInfo.totalCount
					 this.loading=false
				}).catch(message => {
					this.$message.error("请求失败，请联系客服，失败码"+message);
					 this.loading=false
				})
			},
			setReset(){
				this.tableList.xingming=''
				this.tableList.bumen=''
				this.tableList.startTime=''
				this.tableList.endTime=''
			},
			changeFn(flag){
				alert("暂无此功能")
			}

		},
		mounted(){
			this.getTablelist();
		}
	}
</script>