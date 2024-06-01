<template>
	<view>
		<u-navbar :is-back="false" title="OAweb办公系统" title-color="#ffffff" :background="background">
		</u-navbar>
		<view class="wrap">
			<scroll-view scroll-y='true' style="height: 100%;width: 100vw;">
				<view class="container">
					<u-form label-width="80px" label-position="top" :model="form" ref="uForm">

						<u-form-item label="任务内容" prop="taskintro">
							<u-input v-model="form.taskintro" />
						</u-form-item>
						<u-form-item label="优先级" prop="priority">
							<u-input v-model="form.priority" type="select" @click="show = true" />
							<u-action-sheet :list="actionSheetList" v-model="show" @click="actionSheetCallback">
							</u-action-sheet>
						</u-form-item>
						<u-form-item label="参与者" prop="friendInvite">
							<!-- <u-checkbox-group>
								<u-checkbox @change="checkboxChange(item.friendId)" v-model="item.checked"
									v-for="(item, index) in checkboxList" :key="index" :name="item.friendRealName">
									{{ item.friendRealName }}
								</u-checkbox>
							</u-checkbox-group> -->
							<u-input type='select' placeholder="选择参与者" v-model='form.friendInvite' border="true"
								@click='show_p1=true' />
							<u-popup v-model="show_p1" mode="center" width="500rpx" height="300px" border-radius="20"
								@close="close1">
								<view class="pop_2">
									<text class="pop_2_text">选择参与者</text>
									<u-checkbox-group>
										<view v-for="(item, index) in checkboxList" :key="index">
											<u-checkbox @change="checkboxChange1(item.friendId)" v-model="item.checked"
												:name="item.friendRealName">

												{{ item.friendRealName }}


											</u-checkbox>
										</view>
									</u-checkbox-group>

								</view>
								
							</u-popup>
						</u-form-item>
						<u-form-item label="参与群组" prop="friendInvite">
							<!-- <u-checkbox-group>
								<u-checkbox @change="checkboxChange(item.friendId)" v-model="item.checked"
									v-for="(item, index) in checkboxList" :key="index" :name="item.friendRealName">
									{{ item.friendRealName }}
								</u-checkbox>
							</u-checkbox-group> -->
							<u-input type='select' placeholder="选择群组" v-model='form.groupInvite' border="true"
								@click='show_p2=true' />
							<u-popup v-model="show_p2" mode="center" width="500rpx" height="300px" border-radius="20"
								@close="close2">
								
								<view class="pop_2">
									<text class="pop_2_text">选择参与群组</text>
									<u-checkbox-group>
										<view v-for="(item, index) in groupList" :key="index">
											<u-checkbox @change="checkboxChange2(item.id)" v-model="item.checked"
												:name="item.name">
						
												{{ item.name }}
						
						
											</u-checkbox>
										</view>
									</u-checkbox-group>
								</view>
							</u-popup>
						</u-form-item>
						<u-form-item label="开始时间">
							<u-input v-model="value_in1" :type="type_in" :border="border_in" @click='click_in1'
								placeholder="请输入日期" />
							<u-calendar v-model="show_c1" :mode="mode_c" @change="change_c1" max-date="2050-12-31">
								<view slot="tooltip">
									<view class="title">
										请选择开始日期
									</view>
								</view>
							</u-calendar>
							<u-input v-model="value_s1" :type="type_in" :border="border_in" @click='click1'
								placeholder="请输入时间" />
							<u-select v-model="show_s1" mode="mutil-column" :list="list_s" @confirm="confirm_s1">
							</u-select>
						</u-form-item>
						<u-form-item label="结束时间">
							<u-input v-model="value_in2" :type="type_in" :border="border_in" @click='click_in2'
								placeholder="请输入日期" />
							<u-calendar v-model="show_c2" :mode="mode_c" @change="change_c2" max-date="2050-12-31">
								<view slot="tooltip">
									<view class="title">
										请选择结束日期
									</view>
								</view>
							</u-calendar>
							<u-input v-model="value_s2" :type="type_in" :border="border_in" @click='click2'
								placeholder="请输入时间" />
							<u-select v-model="show_s2" mode="mutil-column" :list="list_s" @confirm="confirm_s2">
							</u-select>
						</u-form-item>
					</u-form>

					<u-popup v-model="show_p" mode="center" width="500rpx" height="300px" border-radius="20">
						<view class="pop">
							<text class="pop_text">一经提交无法撤回，确认提交？</text>
							<u-button type="warning" @click="click" :ripple="true" ripple-bg-color="#fcbd71">确认提交
							</u-button>
						</view>
					</u-popup>

					<u-button @click="click_p" type="primary" :ripple="true" ripple-bg-color="#a0cfff">发布</u-button>
				</view>

			</scroll-view>
		</view>
		<u-tabbar v-model="current" :list="tabbar" :mid-button="true"></u-tabbar>
	</view>
</template>

<script>
	import helper from '../../common/base.js';
	var app = getApp();
	export default {
		data() {
			return {
				show_p1: false,
				show_p2: false,
				friendInviteList: [],
				groupInviteList: [],
				


				taskid: 0,
				//Date
				date: '',
				//popup
				show_p: false,
				//calender
				show_c1: false,
				show_c2: false,
				mode_c: 'date',
				//input
				value_in1: '',
				value_in2: '',
				type_in: 'text',
				border_in: true,

				current: 0,
				//postdata
				begintime: '',
				endtime: '',
				priority: '',

				//select
				value_s1: '',
				value_s2: '',
				show_s1: false,
				show_s2: false,
				list_s: [
					[{
							value: '1',
							label: '00'
						},
						{
							value: '2',
							label: '01'
						},
						{
							value: '3',
							label: '02'
						},
						{
							value: '4',
							label: '03'
						},
						{
							value: '5',
							label: '04'
						},
						{
							value: '6',
							label: '05'
						},
						{
							value: '7',
							label: '06'
						},
						{
							value: '8',
							label: '07'
						},
						{
							value: '9',
							label: '08'
						},
						{
							value: '10',
							label: '09'
						},
						{
							value: '11',
							label: '10'
						},
						{
							value: '12',
							label: '11'
						},
						{
							value: '13',
							label: '12'
						},
						{
							value: '14',
							label: '13'
						},
						{
							value: '15',
							label: '14'
						},
						{
							value: '16',
							label: '15'
						},
						{
							value: '17',
							label: '16'
						},
						{
							value: '18',
							label: '17'
						},
						{
							value: '19',
							label: '18'
						},
						{
							value: '20',
							label: '19'
						},
						{
							value: '21',
							label: '20'
						},
						{
							value: '22',
							label: '21'
						},
						{
							value: '23',
							label: '22'
						},
						{
							value: '24',
							label: '23'
						},
					],
					[{
							value: '1',
							label: '00'
						},
						{
							value: '2',
							label: '10'
						},
						{
							value: '3',
							label: '20'
						},
						{
							value: '4',
							label: '30'
						},
						{
							value: '5',
							label: '40'
						},
						{
							value: '6',
							label: '50'
						},
					],

				],

				//tabbar
				tabbar: '',
				background: {
					// backgroundColor: '#001f3f',

					// 导航栏背景图
					background: 'url(https://cdn.uviewui.com/uview/swiper/1.jpg) no-repeat',
					// 还可以设置背景图size属性
					backgroundSize: 'cover',

					// 渐变色
					backgroundImage: 'linear-gradient(45deg, rgb(28, 187, 180), rgb(141, 198, 63))'
				},
				show: false,

				//form data
				form: {
					taskintro: '',
					priority: '',
					friendInvite: '',
					groupInvite:''
				},
				rules: {
					friendInvite: [{
							required: true,
							message: '请至少选择一位参与者',
							trigger: ['change', 'blur'],
						},
						{
							min: 1,
							message: '请至少邀请一位好友',
							trigger: ['change', 'blur'],
						}
					]

				},
				// checkelist: [],
				checkboxnameList: [],
				checkboxList: [{
					img: 1,
					friendName: 'fafa',
					friendRealName: 'fafafa',
					tem: '37.3'
				}, {
					img: 4,
					friendName: 'huahua',
					friendRealName: 'huahuahua',
					tem: '36.3'
				}],
				groupList: [{
					img: 1,
					id:2,
					name: 'fengshengshou',
					intro: 'welcome to fengshengshou',
				}, {
					img: 1,
					id:3,
					name: 'laowangzhe',
					intro: 'welcome to laowangzhe',
				}],
				actionSheetList: [{
						text: '高',
						num: 1
					},
					{
						text: '中',
						num: 2
					},
					{
						text: '低',
						num: 3
					}
				],
				indexList: [],
				// radio: '',
				// switchVal: false
			}
		},
		onShow() {
			this.getList();
			this.getGroup();
		},
		onReady() {
			this.$refs.uForm.setRules(this.rules);
		},
		onLoad() {

			this.date = new Date().getFullYear() + '-' + (new Date().getMonth() + 1) + '-' + new Date().getDate();
			console.log(this.date);
			this.tabbar = [{
					iconPath: "info-circle",
					selectedIconPath: "info-circle-fill",
					text: '资讯',
					count: 0,
					isDot: false,
					customIcon: false,
					pagePath: '/pages/news/news',
				},
				{
					iconPath: "chat",
					selectedIconPath: "chat-fill",
					text: '联系人',
					customIcon: false,
					pagePath: '/pages/friend/friend'
				},
				{
					iconPath: "https://cdn.uviewui.com/uview/common/min_button.png",
					selectedIconPath: "https://cdn.uviewui.com/uview/common/min_button_select.png",
					text: '发布',
					midButton: true,
					customIcon: false,
					pagePath: '/pages/submit/submit'
				},
				{
					iconPath: "bookmark",
					selectedIconPath: "bookmark-fill",
					text: '任务',
					customIcon: false,
					pagePath: '/pages/mytask/mytask'
				},
				{
					iconPath: "account",
					selectedIconPath: "account-fill",
					text: '我的',
					count: 0,
					isDot: false,
					customIcon: false,
					pagePath: '/pages/my/my'
				}
			];
			this.getList();
			this.getGroup();
		},
		// updated() {
		// 	this.updateindexlist();
		// },
		onPullDownRefresh() {
			this.getList();
			this.getGroup();
			setTimeout(e => {
				uni.stopPullDownRefresh();
			}, 2000);
		},
		methods: {

			close1() {
				this.form.friendInvite = '';
				this.friendInviteList = [];
				
				for (var i = 0; this.checkboxList[i] != undefined; i++) {
					if (this.checkboxList[i].checked == true) {

						this.form.friendInvite += this.checkboxList[i].friendRealName + '; ';
						this.friendInviteList.push(this.checkboxList[i].friendName);
					}
				}
				
				
			},
			close2() {
				
				this.form.groupInvite = '';
				this.groupInviteList = [];
				
				for (var i = 0; this.groupList[i] != undefined; i++) {
					if (this.groupList[i].checked == true) {
				
						this.form.groupInvite += this.groupList[i].name + '; ';
						this.groupInviteList.push(this.groupList[i].id);
					}
				}
			},
			//popup
			click_p() {
				this.show_p = true;
				this.begintime = this.value_in1 + 'T' + this.value_s1 + ':00.000+00:00';
				this.endtime = this.value_in2 + 'T' + this.value_s2 + ':00.000+00:00';
				console.log(this.begintime);
				console.log(this.endtime);
			},
			//calender
			click_in1() {
				setTimeout(e => {
					uni.hideKeyboard(); //隐藏软键盘  
				}, 0);

				this.show_c1 = true;
			},
			click_in2() {
				setTimeout(e => {
					uni.hideKeyboard(); //隐藏软键盘  
				}, 0);

				this.show_c2 = true;
			},
			change_c1(e) {
				this.value_in1 = e.result;
				console.log(e);
			},
			change_c2(e) {
				this.value_in2 = e.result;
				console.log(e);
			},

			//select
			click1() {
				setTimeout(e => {
					uni.hideKeyboard(); //隐藏软键盘  
				}, 0);
				this.show_s1 = true;
			},
			click2() {
				setTimeout(e => {
					uni.hideKeyboard(); //隐藏软键盘  
				}, 0);
				this.show_s2 = true;
			},
			confirm_s1(e) {

				this.value_s1 = e[0].label + ':' + e[1].label;
				console.log(e);
			},

			confirm_s2(e) {
				// setTimeout(function() {
				// 	uni.hideKeyboard(); //隐藏软键盘  

				// }, 0);
				this.value_s2 = e[0].label + ':' + e[1].label;
				console.log(e);
			},

			actionSheetCallback(index) {
				this.form.priority = this.actionSheetList[index].text;
				this.priority = this.actionSheetList[index].num;
			},
			getList() {
				uni.request({
					url: 'http://1i682186u9.qicp.vip/friend/findFriend/' + helper.id,
					method: 'GET',
					data: {},
					header: {
						Authorization: helper.tokenHead + '' + helper.token
					},
					success: res => {
						if (res.data.code == 200) {
							console.log(res.data);
							// console.log(helper.id);
							this.checkboxList = res.data.data;
							this.checkboxList.splice(0, 1);
							// this.getnameList();
							// console.log(this.checkboxList);
						}

					},
					fail: () => {},
					complete: () => {

					}
				});

			},
			getGroup() {
				uni.request({
					url: 'http://1i682186u9.qicp.vip/group/findByUserId/' + helper.id,
					method: 'GET',
					data: {},
					header: {
						Authorization: helper.tokenHead + '' + helper.token
					},
					success: res => {
						console.log(res.data);
						if (res.data.code == 200) {
							this.groupList = res.data.data;

						}
					},
					fail: () => {},
					complete: () => {}
				});
			},

			checkboxChange: function(id) {
				this.indexList.push(id);
				this.delete(id);
			},
			delete(id) {
				var index = this.indexList.findIndex(item => {
					if (item == id) {
						return true;
					}
				});
				this.indexList.splice(index, 1)
			},
			click() {
				uni.request({

					url: 'http://1i682186u9.qicp.vip/task/saveTask',
					method: 'POST',
					data: {
						task: {
							tasktext: this.form.taskintro,
							priority: this.priority,
							begintime: this.begintime,
							endtime: this.endtime,
							initiatorid: helper.id,
						},
						recipientids: this.friendInviteList,
						grouplist:this.groupInviteList
					},
					header: {
						Authorization: helper.tokenHead + '' + helper.token
					},
					success: res => {
						console.log(res.data);
						if (res.data.code == 200) {
							uni.showToast({
								title: '上传成功',
								icon: 'success'
							});
						} else {
							uni.showToast({
								title: '创建失败'
							});
						}


						// console.log(this.taskid);
					},
					fail: () => {

					},
					complete: () => {

						this.show_p = false;
						this.form.tasktitle = '';
						this.form.taskintro = '';
						this.form.priority = '';
						this.value_in1 = '';
						this.value_in2 = '';
						this.value_s1 = '';
						this.value_s2 = '';
					}
				});



			}
		}
	}
</script>

<style lang="scss" scoped>
	.wrap {
		height: 100%;
	}

	.container {
		margin: 10px 10px 60px 10px;

	}

	.title {
		color: $u-type-primary;
		text-align: center;
		padding: 20rpx 0 0 0;
	}

	.pop {
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		margin: 10px;
		height: 270px;
	}

	.pop_text {
		color: #fa3534;
	}

	.pop_2 {
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		margin: 10px;
		// height: 270px;
	}

	.pop_2_text {
		margin-bottom: 10px;
	}
</style>
