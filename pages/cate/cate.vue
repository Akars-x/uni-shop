<template>
  <view>
    <view class="cate-container">
      <!-- 左侧的滚动视图区域 -->
      <scroll-view class="left-scroll-view" scroll-y :style="{height:wh+'px'}">
        <!-- 这里用block只起到一个包裹作用，使用view会生成块级结构 -->
        <block v-for="(item, i) in cateList" :key="i">
          <view :class="['left-scroll-view-item', i === active ? 'active' : '']" @click="activeChanged(i)">{{item.cat_name}}</view>
          <!-- <view class="left-scroll-view-item" :class="[i === active ? 'active' : '']" @click="activeChanged(i)">{{item.cat_name}}</view> -->
          <!-- <view class="left-scroll-view-item" :class="['active':i === active]" @click="activeChanged(i)">{{item.cat_name}}</view> -->
        </block>
      </scroll-view>

      <!-- 右侧的滚动视图区域 -->
      <scroll-view class="right-scroll-view" scroll-y :style="{height:wh+'px'}" :scroll-top="scrollTop">
        <!-- 二级分类 -->
        <view class="cate-lv2" v-for="(item2, i2) in cateLevel2" :key="i2">
          <view class="cate-lv2-title">/ {{item2.cat_name}} /</view>
          <!-- 三级分类 -->
          <view class="cate-lv3-list">
            <!-- 三级分类 Item 项 -->
            <view class="cate-lv3-item" v-for="(item3, i3) in item2.children" :key="i3" @click="gotoGoodsList(item3)">
              <!-- 图片 -->
              <image :src="item3.cat_icon"></image>
              <!-- 文本 -->
              <text>{{item3.cat_name}}</text>
            </view>
          </view>
        </view>
      </scroll-view>

    </view>
  </view>
</template>

<script>
  export default {
    data() {
      return {
        wh: 0, //当前设备可用的高度
        cateList: [], //分类数据列表
        active: 0, // 当前选中项的索引，默认让第一项被选中
        cateLevel2: [], //二级分类数据列表
        scrollTop: 0, // 滚动条距离顶部的距离
      };
    },

    onLoad() {
      const sysInfo = uni.getSystemInfoSync() //获取当前系统的信息
      this.wh = sysInfo.windowHeight
      this.getCateList() //调用请求方法
    },

    methods: {
      async getCateList() {
        const {
          data: res
        } = await uni.$http.get('/api/public/v1/categories')
        if (res.meta.status !== 200) return uni.$showMsg()
        this.cateList = res.message
        // 初始化没点击一级分类时给二级分类赋值，否则开始页面不显示
        this.cateLevel2 = res.message[0].children   
      },

      // 选中项改变的事件处理函数
      activeChanged(i) {
        this.active = i
        this.cateLevel2 = this.cateList[i].children

        // scrollTop这个属性必须发生变化才能在切换页面时重新生效(返回顶部)
        // 让 scrollTop 的值在 0 与 1 之间切换(改变就不明显)
        // 这种写法表示如果值为0就变成1，值为1就变成0
        this.scrollTop = this.scrollTop ? 0 : 1
      },

      // 点击三级分类项跳转到商品列表页面
      gotoGoodsList(item3) {
        // uni.navigateTo	保留当前页面，跳转到应用内的某个页面
        // url 需要跳转的应用内非 tabBar 的页面的路径 , 路径后可以带参数。参数与路径之间使用?分隔，参数键与参数值用=相连，不同参数用&分隔；
        uni.navigateTo({
          url: '/subpkg/goods_list/goods_list?cid=' + item3.cat_id
        })
      }
    }
  }
</script>

<style lang="scss">
  .cate-container {
    display: flex;

    .left-scroll-view {
      width: 120px;

      .left-scroll-view-item {
        line-height: 60px;
        background-color: #f7f7f7;
        text-align: center;
        font-size: 12px;

        // 激活项的样式
        &.active {
          background-color: #ffffff;
          position: relative;

          // 渲染激活项左侧的红色指示边线
          &::before {
            content: ' ';
            display: block;
            width: 3px;
            height: 30px;
            background-color: #c00000;
            position: absolute;
            left: 0;
            top: 50%;
            transform: translateY(-50%);
          }
        }
      }
    }

    // 右侧二级分类
    .cate-lv2-title {
      font-size: 12px;
      font-weight: bold;
      text-align: center;
      padding: 15px 0;
    }

    .cate-lv3-list {
      display: flex;
      flex-wrap: wrap;

      .cate-lv3-item {
        width: 33.33%;
        margin-bottom: 10px;
        display: flex;
        flex-direction: column;
        align-items: center;

        image {
          width: 60px;
          height: 60px;
        }

        text {
          font-size: 12px;
        }
      }
    }
  }
</style>
