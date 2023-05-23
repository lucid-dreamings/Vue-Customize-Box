<template>
	<transition name="slide">
		<div class="popup" :class="transition" v-show="showModule">
			<!-- 遮罩层 -->
			<div class="mask" v-if="mask"></div>
			<!-- 弹框 -->
			<div class="p-dialog" :style="{ '--theme-color': themeColor }">
				<!-- 1. 头部 -->
				<div class="p-header">
					<h4>{{ title }}</h4>
					<a href="javascript:;" class="iconfont icon-cuocha_kuai" @click="$emit('cancel')"></a>
				</div>
				<!-- 2. 内容(插槽) -->
				<div class="p-body">
					<slot name="body">content</slot>
				</div>
				<!-- 3. 底部(按钮) -->
				<div :class="{p_footer:true}">
					<button href="javascript:;" class="btn blueBG" v-if="btn === 1" @click="$emit('submit')">{{ submitBtn }}</button>
					<button href="javascript:;" class="btn blueBG" v-if="btn === 2" @click="$emit('cancel')">{{ cancelBtn }}</button>
					<div class="btn-group" v-if="btn === 3">
						<button href="javascript:;" class="btn sure_button" @click="$emit('submit')">{{ submitBtn }}</button>
						<button href="javascript:;" class="btn btn-cancel cancel_button" @click="$emit('cancel')">{{ cancelBtn }}</button>
					</div>
				</div>
			</div>
		</div>
	</transition>
</template>

<script>
	export default {
		name: 'messageRegister',
		props: {
			// 1. 弹框大小：包括 小 small 中 middle 大 large 表单 form
			size: {
				type: String,
				default: 'form',
			},
			// 2. 弹框标题
			title: {
				type: String,
				default: 'warning',
			},
			// 3. 按钮类型： 1 确定 2 取消 3 确定&取消
			btn: {
				type: Number,
				default: 3,
			},
			// 4. 按钮内容
			submitBtn: {
				type: String,
				default: 'submit',
			},
			cancelBtn: {
				type: String,
				default: 'cancel',
			},
			// 5. 是否显示遮罩层
			mask: {
				type: Boolean,
				default: true,
			},
			// 6. 选择弹框显示/隐藏的动画效果：top 从上方渐入渐出 fade 淡入淡出
			transition: {
				type: String,
				default: 'top',
			},
			// 7. 设置主题色			
			showModule: Boolean,
		},
	}
</script>

<style scoped>
	.popup {
		z-index: 11;
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		transition: all 0.5s;
	}

	.mask {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background-color: #000000;
		opacity: 0.5;
	}

	.p-dialog {
		position: absolute;
		top: 40%;
		left: 50%;
		transform: translate(-50%, -50%);
		width: 560px;
		background-color: #fff;
		overflow: hidden;
		border: 1px solid rgba(0,0,0,.2);
		border-radius: .3rem;
		outline: 0;
		
	}

	.p-header {
		height: 60px;
		line-height: 60px;
		padding: 0 25px;
		font-size: 16px;
		border-bottom: 1px solid #dee2e6;
		border-top-left-radius: calc(.3rem - 1px);
		border-top-right-radius: calc(.3rem - 1px);
		
	}
	.p-header h4{
		margin: 0;
		font-size: 1.25rem;
		font-weight: 500;
	}
	a {
		display: inline-block;		
		font-size: 20px;
		transition: all 0.3s;

		&:hover {
			transform: scale(1.4);
		}
	}


	.p-body {
		padding: 42px 40px 54px;
		border-bottom: 1px solid #dee2e6;
		outline: 0;
	}

	.p_footer {
		height: 72px;
		line-height: 72px;
		text-align: center;
		border-top: 1px solid #dee2e6;
	}

	.btn {
		display: inline-block;
		font-weight: 400;
		color: #212529;
		text-align: center;
		vertical-align: middle;
		user-select: none;
		background-color: transparent;
		border: 1px solid transparent;
		padding: .375rem .75rem;
		font-size: 1rem;
		line-height: 1.5;
		border-radius: .25rem;

		&:hover {
			transform: scale(1.1);
		}
	}

	.btn-cancel {
		background-color: #999999;
	}

	.btn-group{
		height: 100px;
		position: relative;
	}
	.sure_button{
		position: absolute;
		right: 100px;
		top: 20px;
		color: #fff;
		background-color: #007bff;
		border-color: #007bff;
		cursor: pointer;
		text-align: center;
	}
	.cancel_button{
		position: absolute;
		right: 20px;
		top: 20px;
		color: #fff;
		background-color: #6c757d;
		border-color: #6c757d;
		text-align: center;
	}
	.blueBG{
		color:black;
		font-size: 20px;
		font-weight: 500;
	}
	.blueBG:hover{
		color: #40C5F1;
	}
	.top {
		&.slide-enter-active {
			top: 0;
		}

		&.slide-leave-to {
			top: -100%;
		}

		&.slide-enter {
			top: -100%;
		}
	}


	.fade {
		&.slide-enter-active {
			opacity: 1;
		}

		&.slide-leave-to {
			opacity: 0;
		}

		&.slide-enter {
			opacity: 0;
		}
	}
</style>