<template>
    <div class="vue-uploader">
        <div class="file-list">
            <section v-for="(file, index) of files" class="file-item draggable-item file-box"  v-if="!isShow">
                <img :src="file.src" alt="" ondragstart="return false;">
                <!-- <p class="file-name">{{file.name}}</p> -->
                <!--&lt;!&ndash;<span class="file-remove" @click="remove(index)">+</span>&ndash;&gt;删除按钮-->
            </section>
            <img :src="this.imgSrc" alt="">
            <section v-if="status == 'ready'" class="file-item">
                <button @click="submit" class="add">点击上传</button>
            </section>

        </div>
         <div class="file-Explain">
            图片在3M以上，支持jpg,png等格式
        </div>
        <section v-if="files.length != 0" class="upload-func">
            <div class="progress-bar">
                <section v-if="uploading" :width="(percent * 100) + '%'">{{(percent * 100) + '%'}}</section>
            </div>
            <!-- <div class="operation-box">
                <button v-if="status == 'ready'" @click="submit">上传</button>
                <button v-if="status == 'finished'" @click="finished">完成</button>
            </div> -->
        </section>
        <input type="file" accept="image/*" @change="fileChanged" ref="file" multiple="multiple">
        <!--{{ this.userName }}-->
    </div>
</template>
<script>

    import qs from 'qs'
    import Vue from 'vue'
    import router from '@/router'
    export default {
        props: {
            src: String,
            userName:String
        },
        data() {
            return {
                status: 'ready',
                files: [],
                filesCur:[],
                point: {},
                uploading: false,
                percent: 0,
                fileIds:[],
                imgSrc:'',
                isShow:''
            }
        },
        methods: {
            // add() {
            //     this.$refs.file.click()
            // },
            setFileIds: function(){
                // this.$emit('getFileIds',this.fileIds);
                this.$emit('getFileIds',this.fileIds);
                // console.log(this.fileIds)
            },
            setClickDisabled :function(bool){
                this.isShow=bool
                this.$emit('getClickDisabled',bool);
            },
            submit() {

                this.$refs.file.click()
                 if (this.filesCur.length === 0) {
                    console.warn('no file!');
                    return
                }
                var formData = new FormData()
                // console.log( this.filesCur)
               // //上穿所有
                this.filesCur.forEach((item) => {
                    formData.append('name', item.name)
                    formData.append('file', item.file)
                    formData.append('userName',this.userName)
                    console.log(item.name, item.file)
                })


               // this.files.forEach(function(item){
               //        console.log(item.size)
               //  })
               //单个上传
                    // console.log(this.files[this.files.length-1])
                    // let lastFiles=this.files[this.files.length-1]
                    // formData.append('name', lastFiles.name)
                    // formData.append('file', lastFiles.file)

                // const xhr = new XMLHttpRequest()
                // xhr.upload.addEventListener('progress', this.uploadProgress, false)
                // xhr.open('POST', this.src, true)
                // this.uploading = true
                // xhr.send((formData))
                // let _self = this
                // xhr.onload = () => {
                //     this.uploading = false
                //     if (xhr.status === 200 || xhr.status === 304) {
                //         this.status = 'finished'
                //         console.log('upload success!')
                //         this.fileIds.push(fileId);

                //         this.setFileIds();

                //     } else {
                //         console.log(`error：error code ${xhr.status}`)
                //     }

                //     // this.finished()
                //     this.setClickDisabled(true)
                // }127.0.0.1:8083/ggstSys/cardMembersList.json
                //
                 var url = 'http://h20z908444.imwork.net:17523/ggstSys/updateFile.json'
                // //     // var data= qs.stringify(formData)
                     var that=this
                this.uploading = true
                //  //console.log(formData);
                //   //  console.log(formData);
                // // that.$http.post(url,{"userName":this.userName,"formData":formData},{emulateJSON:true}).then(
                //
                that.$http.post(url,formData,{emulateJSON:true}).then(
                    function (res){
                      //debugger reImgId
                        //that.files=[]
                        formData= new FormData()
                         if(res.data.ret==0){
                           let fileIds=res.data.reImgIds
                                that.fileIds.push(fileIds)
                                that.uploading = false
                                that.setFileIds()
                                that.$emit('newNodeEvent',fileIds)
                                // let imgSrc=res.data.myStrs
                                //      that.imgSrc=imgSrc
                                //      console.log(imgSrc)
                         }else{
                            ("上传失败")
                         }
                         // that.finished()
                          that.setClickDisabled(false)
                    }
                )

            },
            finished() {
                this.files = []
                this.status = 'ready'
            },
            // remove(index) {
            //    //加一个删除的接口；并且要删除fileIds中对应的值，并调用this.setFileIds();
            //    let deleteId=''
            //      for(let i=0;i<this.fileIds.length;i++){
            //         if(i==index){
            //             deleteId=this.fileIds[i]
            //         }
            //      }
            //      let url=this.$store.state.localHostUrl +'/delUploadImage.json'
            //      let data=qs.stringify({reImgId:deleteId})
            //      this.$http.post(url,data,{emulateJSON:true}).then(
            //         function (res){
            //              if(res.data.ret==0){
            //                 console.log('删除图片成功!')
            //              }
            //         }
            //     )
            //     this.setClickDisabled(true)
            //     this.files.splice(index, 1)
            //      // this.fileIds.splice(index,1)
            // },
            fileChanged() {
              //debugger
                this.filesCur = [];
                this.setClickDisabled(false)
                this.files = []//把它去了，就是连着有多张，不去。清空。只有一张
                const list = this.$refs.file.files
                for (let i = 0; i < list.length; i++) {
                    if (!this.isContain(list[i])) {
                        const item = {
                            name: list[i].name,
                            size: list[i].size,
                            file: list[i]
                        }
                        this.html5Reader(list[i], item)
                        this.files.push(item)
                        this.filesCur.push(item)
                    }
                }
                this.$refs.file.value = ''

                this.submit();
            },
            // 将图片文件转成BASE64格式
            html5Reader(file, item){
                const reader = new FileReader()
                reader.onload = (e) => {
                    this.$set(item, 'src', e.target.result)
                }
                reader.readAsDataURL(file)
            },
            isContain(file) {
                this.files.forEach((item) => {
                    if(item.name === file.name && item.size === file.size) {
                        return true
                    }
                })
                return false
            },
            uploadProgress(evt) {
                const component = this
                if (evt.lengthComputable) {
                    const percentComplete = Math.round((evt.loaded * 100) / evt.total)
                    component.percent = percentComplete / 100
                } else {
                    console.warn('upload progress unable to compute')
                }
            }
        }
    }
</script>
<style>
.vue-uploader {
    /*border: 1px solid #e5e5e5;*/
}
.vue-uploader .file-list {
    padding: 5px 0px;
}
.vue-uploader .file-list:after {
    content: '';
    display: block;
    clear: both;
    visibility: visible;
    line-height: 0;
    height: 0;
    font-size: 0;
}
.vue-uploader .file-list .file-item {
    float: left;
    position: relative;
    width: 33%;
    text-align: center;
}
.vue-uploader .file-list .file-item img{
    width: 90%;
    height: 100px;
}
.vue-uploader .file-list .file-item .file-remove {
      position: absolute;
    right: 6px;
    display: none;
    top: 4px;
    width: 22px;
    height: 22px;
    color: white;
    cursor: pointer;
    line-height: 22px;
    border-radius: 100%;
    -webkit-transform: rotate(45deg);
    transform: rotate(45deg);
    background: #F08632;
}
.vue-uploader .file-list .file-item:hover .file-remove {
    display: inline;
}
.vue-uploader .file-list .file-item .file-name {
    margin: 0;
    height: 40px;
    word-break: break-all;
    font-size: 14px;
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
}
.vue-uploader .file-list .file-box{
      width: 33%;
      height: 160px;
      margin-left: 74px;
      float: left;
      text-align: center;
      line-height: 80px;
      border: 2px dashed #ececec;
      font-size: 30px;
      cursor: pointer;
}
.vue-uploader .file-list .file-box img{
    width: 100%;
    height: 100%;
}
.vue-uploader .add {
    margin:50px 0 50px 74px;
    width: 80px;
    height:30px;
    float: left;
    text-align: center;
    cursor: pointer;
}
.vue-uploader .upload-func {
    display: flex;
    padding: 10px;
    margin: 0px;
    background: #f8f8f8;
    border-top: 1px solid #ececec;
}
.vue-uploader .upload-func .progress-bar {
    flex-grow: 1;
}
.vue-uploader .upload-func .progress-bar section {
    margin-top: 5px;
    background: #00b4aa;
    border-radius: 3px;
    text-align: center;
    color: #fff;
    font-size: 12px;
    transition: all .5s ease;
}
.vue-uploader .upload-func .operation-box {
    flex-grow: 0;
    padding-left: 10px;
}
.vue-uploader .upload-func .operation-box button {
    padding: 4px 12px;
    color: #fff;
    background: #007ACC;
    border: none;
    border-radius: 2px;
    cursor: pointer;
}
.vue-uploader > input[type="file"] {
    display: none;
}
.file-Explain{font-size: 0.65rem;padding:0 0 10px 0;width: 96%;margin:0 auto;}
</style>
