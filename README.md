# Vue-Customize-Box
一个自己定义的自定义组件，方便以后查看

具体使用看博客链接 https://blog.csdn.net/qq_55637303/article/details/130837240?spm=1001.2014.3001.5501


# Vue自定义消息提示框



## 前言

本插件使用的是Vue3框架下原生的JS、CSS，不涉及TS以及SCSS。是一个仅供参考的自定义插件；本文的样式参考了bootstrap4的模态框以及PorkCanteen的代码，结合了复用性+美观性

[Modal · Bootstrap v4 中文文档 v4.6 | Bootstrap 中文网 (bootcss.com)](https://v4.bootcss.com/docs/components/modal/)

[(3条消息) Vue 可自定义弹框组件实现_vue自定义弹框按钮_PorkCanteen的博客-CSDN博客](https://blog.csdn.net/PorkCanteen/article/details/122058746)

## 组件特点

1. 可自定义弹框内容、大小、按钮类型、主题颜色、过渡效果等
2. 可通过插槽自定义弹框主体内容



## 组件设计



### 子组件（设计组件）

~~~vue
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
				<!-- 3. 底部(按钮)，这里修改了a为button，并且附加上了css，为了后期方便使用 -->
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
~~~


![在这里插入图片描述](https://img-blog.csdnimg.cn/2aeb9dde292b4ded8efc278ad4be52c0.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/953e99b883af4297b912a5249410a130.png)




### 父组件

调用方法

~~~vue
<template>
	<div>
        xxxxxx
        <message-register title="新增信息" :btn="2" :showModule="showModule"
							submitBtn="确定" cancelBtn="取消" @cancel="showModule = false" 
							@submit="submit">
			  <template v-slot:body>
			    <div>
			      I will not close if you click outside me. Don't even try to press escape key.
			    </div>
			  </template>
			</message-register>
    </div>
</template>
<script>
    import messageRegister from '组件路径'
    export default {
      name: 'xxx',
      components: {
        Popup,
      },
      data() {
        return {
          showModule: false,
        }
      },
      method: {
        submit() {
          点击确认按钮触发的事件...
          this.showModule = false;
        }
      }  

</script>
~~~





## 结语

学习用途！！！仅供参考，侵权必删
