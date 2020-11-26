<template>
	<view class="my-search-container" :style="{'background-color': bgcolor}">
	    <!-- 使用 view 组件模拟 input 输入框的样式 -->
	    <view class="my-search-box" :style="{'border-radius': radius + 'px'}">
	      <uni-icons type="search" size="17"></uni-icons>
	      <text class="placeholder">搜索</text>
	    </view>
	  </view>
</template>

<script>
	export default {
		data() {
			return {
				
			};
		},
    
    // 设置props接收外界传来的数据
    // 如果使用这个组件时想要自己独特的搜索框样式，就可以传入自己的样式属性，否则就用默认的
    props: {
      // 背景颜色
      bgcolor: {
        type: String,
        default: '#C00000'
      },
      // 圆角尺寸
      radius: {
        type: Number,
        // 单位是 px
        default: 18
      }
    },
	}
</script>

<style lang="scss">
.my-search-container {
  // 移除背景颜色，改由 props 属性控制
  // background-color: #c00000;
  height: 50px;
  padding: 0 10px;
  display: flex;
  align-items: center;
}

.my-search-box {
  height: 36px;
  background-color: #ffffff;
  // 移除圆角尺寸，改由 props 属性控制
    // border-radius: 15px;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;

  .placeholder {
    font-size: 15px;
    margin-left: 5px;
  }
}
</style>
