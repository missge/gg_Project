<template>
  <div>
    <div class="FormFile">
      <div>
        <span @click="page(lastbianhao,'last')">上一页</span>
        <span @click="page(nextbianhao,'next')">下一页</span>
        
      </div>
      <el-form :model="ruleForm" :rules="rules" ref="ruleForm" size="mini" label-width="100px" class="demo-ruleForm contr_time" >
          <el-row :gutter="24">
              <el-col :span="12">
                    <el-form-item label="证号" >
                      <el-input v-model="ruleForm.zhenghao" :disabled="true"></el-input>
                    </el-form-item>
                    <el-form-item label="姓名" prop="xingming">
                      <el-input v-model="ruleForm.xingming"></el-input>
                    </el-form-item>
                    <!--<el-form-item label="用户级别">
                      <el-radio-group v-model="ruleForm.userLevel">
                        <el-radio v-for="(items,key) in userLevel" :key='key' @change="getRadioFn"
                          :label="items.key"
                          >{{items.value}}
                      </el-radio>
                      </el-radio-group>
                    </el-form-item>-->
                  <el-form-item label="状态">
                      <el-select v-model="ruleForm.zhuangtai" placeholder="请选择" >
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
                    <el-form-item label="部门:" prop="bumen">
                      <el-select v-model="ruleForm.bumen" placeholder=""> 
                          <el-option
                            v-for="(item,index) in bumenList"
                            :key="index"
                            :label="item[index]"
                            :value="item[index]" >
                          </el-option>
                      </el-select>
                    </el-form-item> 
                    <el-form-item label="性别" prop="xingbie">
                      <el-radio-group v-model="ruleForm.xingbie">
                        <el-radio label="男"></el-radio>
                        <el-radio label="女"></el-radio>
                      </el-radio-group>
                    </el-form-item>
                    <el-form-item label="职务" >
                      <el-input v-model="ruleForm.zhiwu"></el-input>
                    </el-form-item>
                    <el-form-item label="职称" >
                      <el-input v-model="ruleForm.zhicheng"></el-input>
                    </el-form-item>
                  
                    <el-form-item label="拼音" >
                      <el-input v-model="ruleForm.pinyin"></el-input>
                    </el-form-item>
                    <el-form-item label="领卡人" >
                      <el-input v-model="ruleForm.lingkaren"></el-input>
                    </el-form-item>
                    <el-form-item label="退卡人" >
                      <el-input v-model="ruleForm.tuikaren"></el-input>
                    </el-form-item>
                    <el-form-item label="退卡时间" >
                      <el-col :span="11">
                        <el-form-item >
                          <el-date-picker type="date" placeholder="退卡时间" v-model="ruleForm.tuikashijian" style="width: 100%;"></el-date-picker>
                        </el-form-item>
                      </el-col>
                    </el-form-item>
                    <el-form-item label="备注" prop="desc">
                      <el-input type="textarea" v-model="ruleForm.beizhu"></el-input>
                    </el-form-item>
                    <el-form-item label="出院时间" >
                      <el-col :span="11">
                        <el-form-item >
                          <el-date-picker type="date" placeholder="出生日期" v-model="ruleForm.chushengriqi" style="width: 100%;"></el-date-picker>
                        </el-form-item>
                      </el-col>
                    </el-form-item>
                 
              </el-col>
              <el-col :span="12">
                  
                  <el-form-item label="民族" >
                    <el-input v-model="ruleForm.minzu"></el-input>
                  </el-form-item>
                  <el-form-item label="身份证号" >
                    <el-input v-model="ruleForm.shenfenzhenghao"></el-input>
                  </el-form-item>
                  <el-form-item label="籍贯" >
                    <el-input v-model="ruleForm.jiguan"></el-input>
                  </el-form-item>
                  <el-form-item label="政治面貌" >
                    <el-input v-model="ruleForm.zhengzhimianmao"></el-input>
                  </el-form-item>
                  <el-form-item label="文化程度" >
                    <el-input v-model="ruleForm.wenhuachengdu"></el-input>
                  </el-form-item>
                      
                  <el-form-item label="工作时间" >
                    <el-col :span="11">
                      <el-form-item >
                        <el-date-picker type="date" placeholder="选择日期" v-model="ruleForm.gongzuoshijian" style="width: 100%;"></el-date-picker>
                      </el-form-item>
                    </el-col>
                  </el-form-item>
                  <el-form-item label="来院时间" >
                    <el-input v-model="ruleForm.laiyuanshijian"></el-input>
                  </el-form-item>
                  <el-form-item label="派出所" >
                    <el-input v-model="ruleForm.paichusuo"></el-input>
                  </el-form-item>
                  <el-form-item label="离院时间" >
                    <el-col :span="11">
                      <el-form-item >
                        <el-date-picker type="date" placeholder="选择日期" v-model="ruleForm.liyuanshijian" style="width: 100%;"></el-date-picker>
                      </el-form-item>
                    </el-col>
                  </el-form-item>
                  <el-form-item label="家庭住址" >
                    <el-input v-model="ruleForm.jiatingzhuzhi"></el-input>
                  </el-form-item>
                  <el-form-item label="身份证住址" >
                    <el-input v-model="ruleForm.shenfenzhengzhuzhi"></el-input>
                  </el-form-item>
                   <el-form-item label="物理码" prop="desc">
                      <el-input type="textarea" v-model="ruleForm.wulima" :disabled="true" height="20px"></el-input>
                    </el-form-item>
                   <el-form-item label="领卡时间" >
                      <el-col :span="11">
                        <el-form-item >
                          <el-date-picker type="date" placeholder="领卡时间" v-model="ruleForm.lingkashijian" style="width: 100%;"></el-date-picker>
                        </el-form-item>
                      </el-col>
                    </el-form-item>
              </el-col>
          </el-row>
          <el-form-item>
            <el-button type="primary" @click="seveFn()">保存</el-button>
            <el-button  @click="returnFn()">返回</el-button>
          </el-form-item>
       </el-form>
        <div class="collapse_box">
          <div class="el-collapse">
            <div class="el-collapse-item" style="overflow: hidden; border-bottom: 1px solid #ebeef5;">
                <div class="el-collapse-item__header">
                    <el-button  type="text" style="float:left;padding-left:20px;" @click="returnHistory">返回上一步 </el-button>   
                  状态和操作 
                </div>
            </div>
            <div class="el-collapse-item__wrap">
                <div class="el-collapse-item__content">
                    <el-row >
                        <el-col :span="7">
                          <el-button  type="text" >{{buttonList.fakaStatus}}  </el-button>
                          <el-button  type="text">{{buttonList.guashiStatus}}  </el-button>
                          <el-button  type="text" :disabled="buttonList.guashi===0" @click="OperateFn('reportLoss')">挂失</el-button>
                          <el-button  type="text" :disabled="buttonList.jiegua===0" @click="OperateFn('cancelReportLoss')">解挂</el-button>
                          <el-button  type="text" :disabled="buttonList.buka===0" @click="OperateFn('repairCard')">补卡</el-button>
                        </el-col>
                        <el-col :span="10">
                          <el-button  type="text" :disabled="buttonList.zhongzhicaozuo===0" @click="OperateFn('stop')">终止操作  </el-button>
                          <el-button  type="text" :disabled="buttonList.tuixiu===0" @click="OperateFn('retire')">退休  </el-button>
                          <el-button  type="text" :disabled="buttonList.fanpin===0" @click="OperateFn('rehire')">返聘  </el-button>
                          <el-button  type="text">{{buttonList.shenfenzhengStatus}}  </el-button>
                          <el-button  type="text"> {{buttonList.zhiwenStatus}}    </el-button>
                        </el-col>
                        <el-col :span="7">
                          <el-button  type="text">读身份证</el-button>
                          <el-button  type="text">采集指纹</el-button>
                          <el-button  type="text" :disabled="buttonList.dayin===0" @click="OperateFn('print')">打印</el-button>
                          <el-button  type="text" :disabled="buttonList.faka===0" @click="OperateFn('writeCard')">发卡</el-button>
                          <el-button  type="text" :disabled="buttonList.tuika===0" @click="OperateFn('backCard')"> 退卡 </el-button>
                        </el-col>
                    </el-row>
                </div>
            </div>
          </div>
        </div>
        <div class="collapse_box">
          <el-collapse v-model="activeNames" >
            <el-collapse-item title="修改记录" name="1">
                <el-row :gutter="20" v-for="item in lishi" :key="item.shijian">
                  <el-col :span="6">
                    <div class="grid-content bg-purple">{{item.shijian}}</div>
                  </el-col>
                  <el-col :span="14">
                    <div class="grid-content bg-purple">{{item.neirong}}</div>
                  </el-col>
                </el-row>
            </el-collapse-item>
          </el-collapse>
        </div>
       
        <div id="subOutputRank-print" style="position: relative;display:none;">  
              <table style=" margin: 0px auto; width: 80%; transform: translateX(-50%) translateY(-50%); top: 30%; left: 50%; position: absolute;">
                  <tr>
                    <td style="width:50%;text-align:right;">
                        <p style="font-family:汉仪中宋简;font-size:6mm; margin-bottom: 20px;">
                          {{ruleForm.xingming}}
                        </p>
                        <p style="text-transform:capitalize;font-family:'Times New Roman'; margin-bottom: 20px;">
                          {{ruleForm.pinyin}}
                        </p>
                        <p style="font-size:4mm;font-family:宋体; line-height:14mm; margin-bottom: 20px;font-weight:bold">
                          {{ruleForm.kahao}}
                        </p>
                    </td>
                    <td style="width:50%;text-align:left;">
                      <ul  style="width:50%; border:0px;font-family:宋体;font-size:3mm;line-height:7mm;padding-left:13mm; padding-top:45mm">
                        <li>
                          <span style="color: orange; padding-right: 15px;">姓名</span>
                          <span style="  font-size:4.2mm">
                             {{ruleForm.xingming}}
                          </span>
                        </li>
                        <li>
                          <span style="color: orange;  padding-right: 15px;">职务</span>
                          <span style="font-size:4.2mm"> {{ruleForm.zhiwu}}</span>
                        </li>
                        <li>
                          <span style="color: orange; padding-right: 15px;">职称</span>
                          <span style="font-size:4.2mm">{{ruleForm.zhicheng}}</span>
                        </li>
                        <li>
                          <span style="color: orange; padding-right: 15px;">部门</span>
                          <span style="font-size:4.2mm">{{ruleForm.bumen}}</span>
                        </li>
                        <li>
                          <span style="color: orange; padding-right: 15px;">签发</span>
                          <span style="font-size:4.2mm">{{ruleForm.qianfashijian}}</span>
                        </li>
                      </ul>
                    </td>
                  
                  </tr>
              </table>
          
        </div> 


      </div>
    <object  id="testActive" classid="CLSID:420D42A0-3BB6-4A7C-AA2F-0EE06AC836D9" ></object>
      
  </div>
</template>
<script>
  import {userListChange,saveUserList,BumenZhuangtaiSelect,cardMembersOperate,getUserList,goHistory} from '../../api/api.js';
	import {setCookie,getCookie,delCookie} from '../../cookie/cookie.js'
  import qs from 'qs'
  import Vue from 'vue'
  import router from '@/router'
  export default {
      data() {
        return {
          rules: {
            xingming: [
                { required: true, message: '请输入姓名', trigger: 'blur' }
            ],
            xingbie: [
                { required: true, message: '请选择性别', trigger: 'blur' }
            ],
            bumen :[
              {
                required: true,message:'请选择部门',trigger:'blur'
              }
            ]
          },
          activeNames: ['1'],
          activeNames1: ['1'],
          
          bianhao:this.$route.query.bianhao,
          AddOrChange:this.$route.query.AddOrChange,
          ruleForm:{
            type:this.$route.query.AddOrChange,
            bianhao:'',
            xingming:'',
            zhiwu:'',
            zhuangtai:'',
            pinyin:'',
            bumen:'',
            xingbie:'',
            zhicheng:'',
            shengri:'',
            shenfenzheng:'',
            zhengzhimianmao:'',
            gongzuoshijian:'',
            paichusuo:'',
            minzu:'',
            jiguan:'',
            wenhuachengdu:'',
            laiyuanshijian:'',
            liyuanshijian:'',
            zhuzhi:'',
            shenfenzhengzhuzhi:'',
            lingkaren:'',
            lingkashijian:'',
            tuikaren:'',
            tuikashijian:'',
            beizhu:'',
            zhaopian:'',
            dayin:'',
            caiID:'',
            caizhiwen:''	
          },//form表单必须是一个对象，否则会报错
          checkboxList:[],
          checkedUser:[],
          userLevel:[],
          oldStatus:'',
          lishi:[],
          selectList:[],
          bumenList:[],
          buttonList:[],
          clickList:{
             bianhao:this.$route.query.bianhao,
             method:'',
             params:''
          },
          tableList:{
            bumenname:'',
            zhuangtai:'',
            xingming:'',
            kahao:'',
            pageIndex:1,
            pageNum:10
          },
          lastbianhao:'',
          nextbianhao:'',
          params:this.$route.query.params,
          isShow:false,
          fakaParams:{
	              errMsg:"00",
                //** 用户卡（复旦）参数 **/ 
                 p1:'',
                //** PSAM卡参数 **/
                p2:"20180420",
                p3:"20380420",
                p4:"00",
                info1:'',
                info2:'',
                info3:'',
                info4:'',
                info5:'',
                info6:'',
                info7:"",
                info8:"8888",
                info9:"9999",
                info10:"aaaa",
                fingerprint:"12345678901234567890",
                /** 用户卡（复旦）参数 **/
                reader:1,
                card:2,
                base:0,
                /** PSAM卡参数 **/
          },
          myValue:''
        }
      },
      methods: {
        getTablelist(){
          //标志是点击添加，还是修改按钮
          if(this.AddOrChange==2){
            userListChange({bianhao:this.bianhao,params:this.params}).then((data) => {
              this.ruleForm=data.data
              this.lishi=data.data.lishi
              this.buttonList=data.data.button
              this.lastbianhao=data.data.lastBianhao
              this.nextbianhao=data.data.nextBianhao
            }).catch(message => {
              this.$message.error("请求失败，请联系客服，失败码"+message);
              this.loading=false
            })
          }else{
            this.ruleForm.xingbie="男"
          }
          
        },
        page(bianhao,pageFlag){
          if(pageFlag=='last'){
            this.bianhao=bianhao
          }else{
            this.bianhao=bianhao
          }
            this.getTablelist()
        },
        seveFn(){
          // if(this.$route.query.AddOrChange==1 ){
          //   this.ruleForm.bumen=this.$route.query.bumenname
          // }

          if(this.ruleForm.xingming!==""&&this.ruleForm.bumen!=="" && this.ruleForm.xingbie!=="")
           {
              this.ruleForm.type = this.$route.query.AddOrChange
              saveUserList(this.ruleForm).then((data)=>{
                if(data.code==1){
                   if(this.$route.query.AddOrChange){
                      alert("修改成功")
                      this.$router.push({path:'/userList'})
                    }else{
                      alert("添加成功")
                    }
                }
               
              }).catch(message => {
                this.$message.error("请求失败，请联系客服，失败码"+message);
                this.loading=false
              })
           }
          else{
            alert("此字段不能为空")
          }
         
        },
        getRadioFn(val){
          this.ruleForm.userLevel=val
        },
        getStatus(val){

          let key='';
          this.selectList.forEach(function(items){
            if(items.value==val){
              key=items.key
            }
            
          })
          this.ruleForm.stutas=key
        },
        getCheckFn(val){
          //遍历角色所有的值，遍历选择的值。相等，输出items.key
          let checkArr=[]
          this.checkboxList.forEach(function(items){
            val.forEach(function(valItem){
              if(items.value==valItem){
                // console.log(items.key)
                checkArr.push(items.key)
              }
            })
          })
          this.ruleForm.roleIds=checkArr
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
        returnFn(){
          this.$router.push({path:'/userList'})
        },
        OperateFn(flag){
          this.clickList.method=flag
          if(flag="writeCard"){
            this.clickList.params=0
          }
           cardMembersOperate(this.clickList).then((data)=>{
                if(data.code==1){
                   this.getTablelist();
                    if(flag=="print"){
                      this.printContent()
                    }else if(flag=="writeCard"){
                      this.fakaParams.p1=data.fakaId
                      console.log( this.fakaParams.p1)
                      alert(testActive)
                        this.fakaParams.info1=this.ruleForm.xingming
                       this.fakaParams.info2=this.ruleForm.zhengjianhao
                       this.fakaParams.info3=this.ruleForm.xingbie
                       this.fakaParams.info4=this.ruleForm.bumen
                      this.fakaParams.info5=this.ruleForm.zhiwu
                      this.fakaParams.info6=this.ruleForm.zhicheng
                      this.myValue = testActive.CreateUserCard(this.fakaParams.p1,this.fakaParams.p2,this.fakaParams.p3,this.fakaParams.p4,this.fakaParams.info1,this.fakaParams.info2,this.fakaParams.info3,this.fakaParams.info4,this.fakaParams.info5,this.fakaParams.info6,this.fakaParams.info7,this.fakaParams.info8,this.fakaParams.info9,this.fakaParams.info10,this.fakaParams.fingerprint,this.fakaParams.reader,this.fakaParams.card,this.fakaParams.base);
                      alert( this.myValue)
                
                    }
                }
               
              }).catch(message => {
                this.$message.error("请求失败，请联系客服，失败码"+message);
                this.loading=false
              })
        },
        returnHistory(){
          goHistory({bianhao:this.bianhao}).then((data)=>{
            if(data.code==1){
              this.getTablelist()
            }else{
               this.$message('操作失败'+data.descript);
            }
          })
        },
        printContent(e){  
              let subOutputRankPrint = document.getElementById('subOutputRank-print');  
              console.log(subOutputRankPrint.innerHTML);  
              let newContent =subOutputRankPrint.innerHTML;  
              let oldContent = document.body.innerHTML;  
              document.body.innerHTML = newContent;  
              window.print();  
              window.location.reload();  
              document.body.innerHTML = oldContent;  
              return false;  
          },  

      },
      mounted(){
         this.getTablelist();
			this.getSelectList();
        
      }
   }
</script>
<style>
  .FormFile{
    margin:0 auto;
    background:#fff;
  }	
  .FormFile>.el-form{
    width:90%;
    padding: 24px;
    margin:10px 0;
  }
  .FormFile .el-form-item__content{text-align:left;}
  .collapse_box{    border: 1px solid #ebeef5;width:90%;margin:10px auto;}
  .collapse_box .el-collapse-item.is-active .el-collapse-item__header{
      border-bottom: 1px solid #ebeef5;
  }
  clearfix:before,
  .clearfix:after {
    display: table;
    content: "";
  }
  .clearfix:after {
    clear: both
  }
   .buttonList{width:90%;margin:0 auto;}
  .buttonList span{
    cursor: pointer
  }
 /* .contr_time  .el-form-item--mini.el-form-item{margin-bottom:0px;} */

</style>