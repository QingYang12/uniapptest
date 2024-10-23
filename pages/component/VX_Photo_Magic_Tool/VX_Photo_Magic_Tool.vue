<template>
    <view>
        <page-head :title="title"></page-head>
        <view class="uni-padding-wrap uni-common-mt">
			<view class="title" style="display: flex; justify-content: center;color:#00FF00">状态：等待上传</view>
			<view class="uni-title">
				请上传 <text>\n本地图片</text>
			</view>
			<view class="uni-center" style="background:#FFFFFF; font-size:0;">
				<image class="image" ref="myImage"     mode="widthFix" src="../../../static/upload.png" />
			</view>
			<!-- --><uni-file-picker limit="1" title="最多选择1张图片" :source-type="sourceType" @select="handleFileSelect"></uni-file-picker>
            
			<ul>
			  <li v-for="(file, index) in selectedFiles" :key="index">{{ file.name }}</li>
			</ul>
			<button type="primary" @click="handleUploadClick"   >此处上传要修改的图片</button>
			<view class="title" style="display: flex; justify-content: center;color:#0000FF">图片中解析出的对话会填于下方</view>
			<view class="title" style="display: flex; justify-content: center;color:#0000FF">修改改后点击提交即可重新生成图片</view>
			<view class="title" style="display: flex; justify-content: center;color:#0000FF">生成后会出现图片预览，点击下载即可下载到本地</view>
			<view v-for="(item, index) in talkArr" :key="index">
			    <view class="title" style="color: #555555;margin: 10px;">{{ item.name }} 的所有对话内容：</view>
			    <input class="uni-input" :id="item.name+item.Num" v-model="item.value" />
			</view>
			<button type="primary" @click="createNewImgeClick" >提交</button>
			<view class="title" style="color: #00FF00;">生成预览：</view>
			<view class="uni-center" style="background:#FFFFFF; font-size:0;">
				<image class="image" mode="widthFix" src="../../../static/status.png" />
			</view>
			<button type="primary" style="backgroundColor:#1AAD19;"   @click="dowloadNewImg" >下载</button>
        </view>
    </view>
</template>
<script>
	import axios from 'axios';
	
	
    export default {
        data() {
            return {
				sourceType: ['album'],
				selectedFiles: [], // 已选文件列表
                title: 'VX修图神器',
                loading: false,
				file: null,
			    progress: null,
			    result: null,
				taskdir:"",
				talkArrOld:[],
				talkArr:[{"name":"A","value":"xxxx","Num":"1"},{"name":"B","value":"xxxx","Num":"2"},{"name":"A","value":"xxxx","Num":"3"}] 
            }
        },
        onLoad() {
            this._timer = null;
        },
        onShow() {
            this.clearTimer();
            this._timer = setTimeout(() => {
                this.loading = true;
            }, 300)
        },
        onUnload() {
            this.clearTimer();
            this.loading = false;
        },
        methods: {
            openTypeError(error) {
                console.error('open-type error:', error);
            },
            clearTimer() {
                if (this._timer != null) {
                    clearTimeout(this._timer);
                }
            },
			handleFileSelect (event) {
			      this.selectedFiles = event.tempFiles;  
			    },
			 handleUploadClick (){
				 //测试方法
				 /*axios.post('http://127.0.0.1:8088/vxapi/test', {"abc":"123"}, {
				 				headers: {
				 				  'Content-Type': 'application/json',
				 				},
				 }).then((res) => {
				 				alert(JSON.stringify(res));
				 				console.log("res:",res)
				 				console.log("res data:",res.data)
				 }).catch((err) => {
				 				console.error(err);
				 });*/
					/**/console.log("上传文件")
					this.selectedFiles.forEach((file) => {  
					        const uploadUrl = 'http://127.0.0.1:8088/vxapi/upload';  
					        uni.uploadFile({  
					          url: uploadUrl,  
					          filePath: file.path,  
					          name: 'file', // 后端接收文件的字段名  
					          formData: {  
					            // 可以在这里添加其他表单数据  
					          },  
					          success: (uploadFileRes) => {  
								alert("上传成功，请编辑并提交")
					            console.log("上传文件成功", uploadFileRes.data);  
								var jsondata=JSON.parse(uploadFileRes.data);
					            // 处理上传成功后的逻辑  
								
								console.log("jsondata.data ", jsondata.data);  
								 this.talkArr=jsondata.data;
								 console.log("talkArr ", this.talkArr); 
								 this.talkArrOld=JSON.parse(JSON.stringify(jsondata.data))  ;
								 this.taskdir=jsondata.taskdir;
								 console.log("taskdir ", this.taskdir); 
					          },  
					          fail: (error) => {  
					            console.error("上传文件失败", error);  
					            // 处理上传失败后的逻辑  
					          }  
					        });  
					      });  
					
					//直接上传 按钮的方式
					/*uni.chooseImage({
						count: 1,
					    sizeType: ['original', 'compressed'],
					    sourceType: ['album', 'camera'],
						success: (res) => {
							let files = res.tempFiles.map(tempFile => {
							      return {
							        name: tempFile.name,
							        uri: tempFile.path,
							        type: tempFile.type,
							        size: tempFile.size,
							        lastModified: tempFile.lastModified,
							      }
							    })
							uni.uploadFile({
								url: 'http://127.0.0.1:8088/vxapi/upload', 
								file: files,
								success: (uploadFileRes) => {
									console.log("上传文件成功", uploadFileRes.data);
								}
							});
						}
					});*/
				},
				createNewImgeClick(){
					console.log("createNewImgeClick:");
					console.log("talkArrNew:",JSON.stringify(this.talkArr));
					console.log("talkArrOld:",JSON.stringify(this.talkArrOld));
					var param={
						"talkArrNew":this.talkArr,
						"talkArrOld":this.talkArrOld,
						"taskdir":this.taskdir
					}
					
					axios.post('http://127.0.0.1:8088/vxapi/createNewImg', param, {
									headers: {
									  'Content-Type': 'application/json',
									},
					}).then((res) => {
									alert("编辑成功，请点击下载")
									console.log("res:",res)
									console.log("res data:",res.data)
					}).catch((err) => {
									console.error(err);
					});
				},
				
				dowloadNewImg(){
					console.log("dowloadNewImg:");
					console.log("talkArrNew:",JSON.stringify(this.talkArr));
					console.log("talkArrOld:",JSON.stringify(this.talkArrOld));
					var param={
						"talkArrNew":this.talkArr,
						"talkArrOld":this.talkArrOld,
						"taskdir":this.taskdir
					}
					
					axios.post('http://127.0.0.1:8088/vxapi/dowloadNewImg', param, {
									responseType: 'blob', // 指定响应数据的类型为 blob
					}).then((res) => {
							console.log("res:", res)
							  let url = window.URL.createObjectURL(new Blob([res.data]));
							  // 创建一个 a 标签，设置下载链接和文件名，模拟点击实现下载操作
							  let link = document.createElement('a');
							  link.href = url;
							  link.download = 'image.png';
							  document.body.appendChild(link);
							  link.click();
					}).catch((err) => {
									console.error(err);
					});
				}
        }
    }
</script>

<style>
    button {
        margin-top: 30rpx;
        margin-bottom: 30rpx;
    }

    .button-sp-area {
        margin: 0 auto;
        width: 60%;
    }

    .mini-btn {
        margin-right: 10rpx;
    }
</style>
