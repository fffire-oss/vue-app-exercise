<template>
	<view>
		<u-navbar :is-back="true" title="OAweb办公系统" title-color="#ffffff" :background="background">
		</u-navbar>
		<view class="wrap">
			<u-form :model="form" ref="uForm" label-position="top" :label-style="{'font-size':'32rpx'}">
				<u-form-item label="任务名">
					<text>{{tasktext}}</text>
				</u-form-item>
				<u-form-item label="提交内容" prop="uploadText">
					<u-input v-model="form.uploadText" border="true" height="420" type="textarea"
						:auto-height="false" />
				</u-form-item>
				<u-form-item label="上传图片">
					<u-toast ref="uToast"></u-toast>
					<view class="pre-box" v-if="!showUploadList">
						<view class="pre-item" v-for="(item, index) in lists" :key="index">
							<image class="pre-item-image" :src="item.url" mode="aspectFill"></image>
							<view class="u-delete-icon" @tap.stop="deleteItem(index)">
								<u-icon name="close" size="20" color="#ffffff"></u-icon>
							</view>
							<u-line-progress v-if="item.progress > 0 && !item.error" :show-percent="false" height="16" class="u-progress"
							 :percent="item.progress"></u-line-progress>
						</view>
					</view>
					<u-upload @on-success="mySuccess" @on-remove="myRemove" @on-choose-fail="onChooseFail" :before-remove="beforeRemove" ref="uUpload" :custom-btn="customBtn" :show-upload-list="showUploadList" :action="action" :auto-upload="autoUpload" :file-list="fileList"
					 :show-progress="showProgress" :deletable="deletable" :max-count="maxCount" @on-list-change="onListChange">
						<view v-if="customBtn" slot="addBtn" class="slot-btn" hover-class="slot-btn__hover" hover-stay-time="150">
							<u-icon name="photo" size="60" :color="$u.color['lightColor']"></u-icon>
						</view>
					</u-upload>
					

				</u-form-item>
			</u-form>


			<u-button @click="submit" type="primary" :ripple="true" ripple-bg-color="#a0cfff">提交</u-button>
		</view>
	</view>
</template>

<script>
	import helper from '../../common/base.js';
	export default {
		data() {
			return {
				//upload
				action: 'http://1i682186u9.qicp.vip/file/uploadimg',
				// 预置上传列表
				fileList: [],
				header:{Authorization:''},
				showUploadList: true,
				customBtn: false,
				autoUpload: true,
				showProgress: true,
				deletable: true,
				customStyle: false,
				maxCount: 4,
				lists: [], // 组件内部的文件列表
				pidlist:[],
				
				background: {
					// backgroundColor: '#001f3f',

					// 导航栏背景图
					background: 'url(https://cdn.uviewui.com/uview/swiper/1.jpg) no-repeat',
					// 还可以设置背景图size属性
					backgroundSize: 'cover',

					// 渐变色
					backgroundImage: 'linear-gradient(45deg, rgb(28, 187, 180), rgb(141, 198, 63))'
				},
				tasktext: '',
				taskid: 0,
				
				flag: 1,
				form: {
					uploadText: '',
				},
				rules: {
					uploadText: [{
						required: true,
						message: '请输入提交内容',
						trigger: ['blur', 'change']
					}]
				},
				filesArr: []
			}
		},
		onLoad() {
			console.log(helper.id);
			
				this.header.Authorization= helper.tokenHead + '' + helper.token;
			
			
		},
		methods: {
			mySuccess(data){
				console.log(data);
				this.pidlist.push(data.data.pid);
			},
			myRemove(data){
				console.log(data);
				this.pidlist.splice(data.index, 1);
			},
			// clear() {
			// 	this.$refs.uUpload.clear();
			// },
			onListChange(lists) {
				// console.log('onListChange', lists);
				this.lists = lists;
			},
			beforeRemove(index, lists) {
				return true;
			},
			deleteItem(index) {
				this.$refs.uUpload.remove(index);
			},
			reUpload() {
				this.$refs.uUpload.reUpload();
			},
			upload() {
				this.header.Authorization= helper.tokenHead; 
				console.log(helper.tokenHead);
				this.$refs.uUpload.upload();
			},
			submit() {
				this.$refs.uForm.validate(valid => {
					if (valid) {
						console.log('验证通过');
						uni.request({
							url: 'http://1i682186u9.qicp.vip/task/updateUtask',
							method: 'POST',
							data: {
								taskid: this.taskid,
								flag: 2,
								recipientid: helper.id,
								
							},
							header: {
								Authorization: helper.tokenHead + '' + helper.token
							},
							success: res => {
								console.log(res.data);
								// if (res.data.flag == 2) {
								// 	uni.showToast({
								// 		title: '提交成功',
								// 		icon: 'success'
								// 	});
								// } else {
								// 	uni.showToast({
								// 		title: '提交失败'
								// 	});
								// }
							},
							fail: () => {},
							complete: () => {}
						});
						uni.request({
							url: 'http://1i682186u9.qicp.vip/audit/taskSubmit',
							method: 'POST',
							data: {
								pidList: this.pidlist,
								taskid: this.taskid,
								text:this.form.uploadText
							},
							header: {
								Authorization: helper.tokenHead + '' + helper.token
							},
							success: res => {
								console.log(res.data);
								if (res.data.code == 200) {
									uni.showToast({
										title: '提交成功',
										icon: 'success'
									});
								} else {
									uni.showToast({
										title: '提交失败'
									});
								}
							},
							fail: () => {},
							complete: () => {}
						});
					} else {
						console.log('验证失败');
					}
				});
			}
		},
		
		onReady() {
			this.$refs.uForm.setRules(this.rules);
		},
		onLoad() {
			uni.$on("sendTaskDetail", (data) => {

				this.tasktext = data.tasktext;
				this.taskid = data.taskId;
				console.log(data.taskId);
			});
		},
		onUnload() {
			uni.$off('sendTaskDetail');
		}
	}
</script>

<style scoped lang="scss">
	.wrap {
		padding: 30rpx;
	}
</style>
