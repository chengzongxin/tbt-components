<script setup lang="ts">
/*
		空页面类型：
			1. 支持使用组件自带样式
			2. 支持自定义图标，文字样式
			3. 支持插槽，直接添加所需元素

		Usage：使用组件内指定类型
			方法一：
			使用 type 指定无页面类型，后续在./props扩展
			nodata：无数据
			nonet：无网络

			方法二：
			自定义icon，text，desc
			传入自定义数据即可。例如：
			<c-empty type='nodata' icon="xxx" text="xxx" desc="xxx">
			</c-empty>
		 */

const typeData = {
  nodata: {
    icon: "https://pic.to8to.com/te/osf/32b84eb0fded4a71a568acde94f978bb.png",
    text: "这里空空如也",
    desc: "",
  },
  nonet: {
    icon: "https://pic.to8to.com/te/osf/447edeada0b14022ba0b17a2ab77256e.png",
    text: "网络正在开小差",
    desc: "点击屏幕刷新一下吧",
  },
  noservice: {
    icon: "https://pic.to8to.com/te/osf/32b84eb0fded4a71a568acde94f978bb.png",
    text: "服务器开小差了",
    desc: "请稍后再重试",
  },
};

const props = defineProps({
  type: {
    type: String,
    default: 'nodata'
  },
  icon: {
    type: String,
  },
  text: {
    type: String,
  },
  desc: {
    type: String,
  },
});

const emits = defineEmits(["click"]);

const onClick = () => {
  emits("click");
};


function getIcon() {
  return props.icon ? props.icon : getValueInTypeData("icon");
}

function getText() {
  return props.text ? props.text : getValueInTypeData("text");
}

function getDesc() {
  return props.desc ? props.desc : getValueInTypeData("desc");
}

function getValueInTypeData(key: string) {
  const dict = typeData[props.type];
  return dict[key];
}

function clickPage() {
  emits("click");
}
</script>

<template>
  <view class="c-empty" @click="onClick">
    <image class="c-empty-image" :src="getIcon()" mode="widthFix"></image>
    <text class="c-empty-text">{{ getText() }}</text>
    <text class="c-empty-desc">{{ getDesc() }}</text>
    <view class="c-empty-wrap" v-if="$slots.default">
      <slot />
    </view>
  </view>
</template>

<style scoped lang="less">

.c-empty {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  min-height: 100vh;

  &-image {
    margin-top: -300rpx;
    width: 200rpx;
    height: 200rpx;
  }

  &-text,
  &-desc {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 48rpx;
  }

  &-text {
    font-size: 28rpx;
    color: #1a1a1a;
  }

  &-desc {
    margin-top: 16rpx;
    font-size: 28rpx;
    color: #aaafbe;
  }

  &-wrap {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 48rpx;
  }
}
</style>