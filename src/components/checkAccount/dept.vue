<template>
	<div>
		<el-dialog :title="methodName" :visible.sync="dialogFormVisible" :modal="false" lock-scroll = 'true'>
		 	<el-form :model="formData">
		 		<el-form-item label="部门主键" :label-width="formLabelWidth" v-show ="showPinyin">
			     	<el-input v-model="formData.pinyin" auto-complete="off"></el-input>
			    </el-form-item>
			    <el-form-item label="部门名称" :label-width="formLabelWidth">
			     	<el-input v-model="formData.bumen" auto-complete="off"></el-input>
			    </el-form-item>
			    <el-form-item label="部门代码" :label-width="formLabelWidth">
			        <el-input v-model="formData.daima" auto-complete="off"></el-input>
			  	</el-form-item>
			</el-form>
			
			<el-card class="box-card" v-show ="!showPinyin">
				<div align="left">
					修改内容
				</div>
			 	<div class="text item">
			    	<el-table
			   			 :data="editDetials"  strpe style="width: 100%" v-loading="loading"  max-height="150">
						<el-table-column align='left' prop="shijian" label="时间">  </el-table-column>
			    		<el-table-column align='left' prop="neirong" label="内容">  </el-table-column>
			   		</el-table>
			  	</div>
			</el-card>


			<div slot="footer" class="dialog-footer">
			    <el-button @click="dialogFormVisible = false">取 消</el-button>
			    <el-button type="primary" @click="handleEditSave()">确 定</el-button>
			</div>
		</el-dialog>



	  	<el-form :inline="true" :model="tableList" class="demo-form-inline query-form ">
	 		<el-form-item label="部门编号">
                   <el-input v-model="tableList.daima" placeholder="部门编号"></el-input>
            </el-form-item>
            <el-form-item label="部门名称">
                   <el-input v-model="tableList.bumen" placeholder="部门名称"></el-input>
            </el-form-item>
              
            <div class="query_button">
             		<el-form-item>
		            	<el-button type="primary" @click="getSarch(tableList.daima,tableList.bumen)">查询</el-button>
		                <el-button  @click="setReset">重置</el-button>
		           	</el-form-item>
             </div>
       </el-form>
        <div class="table"> 
            <el-table
			    :data="tableDatas"  border style="width: 100%" v-loading="loading">
			    <el-table-column   align='left' prop="daima" label="部门代码">  </el-table-column>
			    <el-table-column   align='left' prop="bumen" label="部门名称">  </el-table-column>
			    <el-table-column   align='left' prop="maxid" label="新编号">  </el-table-column>
			    <el-table-column   align='left' label="操作" width="200">
	                <template slot-scope="scope">
	                    <el-button size="small" @click="handleEdit(scope.row.pinyin )">修改</el-button>
	                    <el-button size="small" @click="handleDel(scope.$index, scope.row,scope.row.pinyin)">删除</el-button>
	                </template>
	            </el-table-column>
			  </el-table>
			 <!--  <el-pagination class="pagination"  @current-change="handleCurrentChange" background layout="prev, pager, next,jumper"  :current-page.sync="tableList.pageIndex" :total="totalCount">
			 			 </el-pagination> -->
			<el-row :gutter="20" style="padding:10px">
				<el-col :span="2" ><el-button size="small" @click="handleAdd()">添加</el-button></el-col>
			</el-row>
        </div>
	</div>
</template>
<script >
	import qs from 'qs'
	import Vue from 'vue'
	import router from '@/router'
	import { getSelectList,getMallSettInfo,deptList ,searchBuMenInfo ,deptEditSave} from "../../api/api";
	export default{
		name: 'HelloWorld',
		data () {
			return {
				 tableList:{
		        	//mallId:this.$store.state.mallId,
		         	pageIndex:1
		         	/*daima:'',
		         	bumen:''*/
		        	//settId:'',
		         	//status:'',
		         	//startTime:'',
		         	//endTime:''
		         },
		        editDetials : [],

		        tableDatas:[],
		        //列表的值
		        //totalCount:0,//页数
		       // time:'',
		        loading: false,
		        //type:'settStatus',
		      //  selectList:'',
		      	dialogFormVisible: false,
		      	formData: {
		          bumen: '',
		          daima: '',
		          type : '',
		          pinyin : ''
		        },
		        showPinyin : '',
		        methodName : '',
				//formInline: {
				//	user: '',
				//	region: '',
				//	name:''
				//},
				//oldStatus:''
				formLabelWidth: '100px',
				code : ''
			}
		},
		methods:{
			//获取table列表
			getTablelist(){
				// var url = this.$store.state.localHostUrl +'/mallSettInfo.json'
		  //   	let that= this
				// that.$http.post(url,data,{emulateJSON:true}).then(
				// 	function(res){
				// 		if(res.data.code===1){
				// 			that.tableDatas=res.data.data
				// 			that.totalCount=res.data.pageInfo.totalCount
				// 			that.loading=false
				// 		}else{
				// 			that.$message.error("请求失败，请联系客服，失败码"+res.data.descript);
				// 			that.loading=false
				// 		}
				// 	}
				// )
		    	// var data= qs.stringify(this.tableList)
				deptList(this.tableList).then(data => {
					console.info(data.data);
					this.tableDatas=data.data
					
					//this.totalCount=data.pageInfo.totalCount
					this.loading=false
				}).catch(message => {
					this.$message.error("请求失败，请联系客服，失败码"+message);
				    this.loading=false
				});
			},

			handleAdd(){
				this.dialogFormVisible =true
            	this.formData.type = 1
            	this.showPinyin = true
            	
            	this.bumen = ''
            	this.daima = ''
            	this.methodName = '添加'
            },
			setReset(){
				
			},

            handleEdit(pinyin ){
            	/*var url = this.$store.state.localHostUrl +'searchBuMenInfo.json'
            	let that= this
				that.$http.post(url,{pinyin : pinyin},{emulateJSON:true}).then(
				function(res){
					if(res.data.code===1){
						console.info(data.data);
				 			
				 		}else{
				 			that.$message.error("请求失败，请联系客服，失败码"+res.data.descript);
				 			that.loading=false
				 		}
				 	}
				)*/

				searchBuMenInfo({pinyin : pinyin}).then(data => {
					console.info(data);
					this.dialogFormVisible =true
	            	this.formData.pinyin = data.data.pinyin	
	            	this.formData.bumen = data.data.bumen 
	            	this.formData.daima = data.data.daima
	            	this.formData.type = 2
	            	this.showPinyin = false;
	            	this.methodName = '修改'

	            	this.editDetials = data.data_ls
					//this.tableDatas=data.data
					
					//this.totalCount=data.pageInfo.totalCount
					//this.loading=false
				}).catch(message => {
					this.$message.error("请求失败，请联系客服，失败码"+message);
				    this.loading=false
				});
			


            	/*this.dialogFormVisible =true
            	this.formData.pinyin = pinyin	
            	this.formData.name = bumen 
            	this.formData.daima = daima
            	this.formData.type = 2
            	this.showPinyin = false;
            	this.methodName = '修改'*/

            	/*deptEditSave(this.formData).then(data => {
            		if(data.code == 1){
            			this.dialogFormVisible =false
            			getTablelist()
            		}else{
            			this.$message.error("请求失败，请联系客服，失败码"+data.descript);
            		}
            	}).catch(message =>{
            		this.$message.error("请求失败，请联系客服，失败码"+message);
				    this.loading=false
            	})*/
            },

            handleEditSave(){
            	if(this.formData.pinyin == ''){
            		alert("请填写部门主键");
            		return
            	}

            	deptEditSave(this.formData).then(data => {
					if(data.code == 1){
            			this.dialogFormVisible =false
            			deptList(this.tableList).then(data => {
							console.info(data.data);
							this.tableDatas=data.data
							
							//this.totalCount=data.pageInfo.totalCount
							this.loading=false
						}).catch(message => {
							this.$message.error("请求失败，请联系客服，失败码"+message);
						    this.loading=false
						});
            		}else{
            			this.$message.error("请求失败，请联系客服，失败码" + data.descript);
            		}
					this.loading=false
				}).catch(message => {
					this.$message.error("请求失败，请联系客服，失败码" + message);
				    this.loading=false
				});
            },
            handleDel(index , row , pinyin){
				deptEditSave({pinyin : pinyin, type : 9}).then(data => {
					if(data.code == 1){
            			deptList(this.tableList).then(data => {
							console.info(data.data);
							this.tableDatas=data.data
						}).catch(message => {
							this.$message.error("请求失败，请联系客服，失败码"+message);
						    this.loading=false
						});
            		}else{
            			this.$message.error("请求失败，请联系客服，失败码" + data.descript);
            		}
					this.loading=false
				}).catch(message => {
					this.$message.error("请求失败，请联系客服，失败码" + message);
				    this.loading=false
				});
            }

		},
		mounted(){
			this.getTablelist();

			//this.getSelectList()
		}
	}
</script>
<style scoped>
	.main_left{
		text-align: left;
	}
	.dialogHeight{
		height:  400px;
	}
	.rowData{
		float: left;
	}

</style>
