<template>
  <view
    :class="['header', headerH5]"
    :style="
      backgroundHeadFullImg
        ? `background-image: url(${backgroundHeadFullImg})`
        : `background: ${color ? color : '--color(--qui-FC-FFF)'}`
    "
  >
    <view class="logoBox" @click="logoClick">
      <image
        class="logo"
        :src="headImg != '' && headImg != null ? headImg : '/static/admin-logo-x2.png'"
        mode="aspectFit"
        lazy-load
      ></image>
    </view>
    <view class="circleDet">
      <text>
        <text class="circleDet-txt">{{ t.theme }}</text>
        <text class="circleDet-num">{{ themeNum }}</text>
      </text>
      <text>
        <text class="circleDet-txt">{{ t.homecontent }}</text>
        <text class="circleDet-num">{{ postNum }}</text>
      </text>

      <view class="circleDet-share" @click="open">
        <qui-icon class="qui-icon" name="icon-share1" size="26" :color="iconcolor"></qui-icon>
        {{ t.share }}
      </view>
      <view class="mask" v-if="shareShow" @click="closeShare">
        <view class="wxShareTip">
          <img src="/static/sharePoint.png" alt class="sharePoint" />
          <img src="/static/shareKnow.png" alt class="shareKnow" />
        </view>
      </view>
    </view>
  </view>
</template>
<script>
let headerH5 = 'header-h5';
/* #ifdef MP-WEIXIN */
headerH5 = '';
/* #endif */
export default {
  name: 'QuiHeader',
  props: {
    headImg: {
      type: String,
      default: '',
    },
    backgroundHeadFullImg: {
      type: String,
      default: '',
    },
    themeNum: {
      type: [Number, String],
      default: 0,
    },
    homecontent: {
      type: String,
      default: '',
    },
    iconcolor: {
      type: String,
      default: '#fff',
    },
    postNum: {
      type: [Number, String],
      default: 0,
    },
    share: {
      type: String,
      default: '',
    },
    shareBtn: {
      type: String,
      default: '',
    },
    shareShow: {
      type: Boolean,
      default: false,
    },
    title: {
      type: String,
      default: '',
    },
    isShowBack: {
      type: Boolean,
      default: true,
    },
    isShowHome: {
      type: Boolean,
      default: true,
    },
    isShowMore: {
      type: Boolean,
      default: true,
    },
    color: {
      type: String,
      default: '',
    },
  },
  data: () => {
    return {
      headerH5,
    };
  },
  computed: {
    // 语言包
    t() {
      return this.i18n.t('home');
    },
  },
  onLond() {},
  methods: {
    open(evt) {
      this.$emit('click', evt);
    },
    closeShare(evt) {
      this.$emit('closeShare', evt);
    },
    logoClick(evt) {
      this.$emit('logoClick', evt);
    },
  },
};
</script>
<style lang="scss">
@import '@/styles/base/variable/global.scss';
@import '@/styles/base/theme/fn.scss';
.header {
  position: relative;
  width: 100%;
  height: 400rpx;
  // background: #1878f3;
  background-size: cover;
  .logo {
    display: block;
    // width: 100%;
    max-height: 88rpx;
    padding-top: 159rpx;
    margin: 0 auto;
  }
  /deep/ .qui-back {
    background: transparent;
  }
  .circleDet {
    display: flex;
    justify-content: space-between;
    padding: 69rpx 30rpx 47rpx;
    line-height: 37rpx;
    text-align: center;
    text {
      // padding: 0 15rpx;
      // font-size: $fg-f3;
      line-height: 37rpx;
      text-align: center;
    }
  }
  .circleDet-txt {
    font-size: $fg-f3;
    color: --color(--qui-FC-FFF);
    opacity: 0.5;
  }
  .circleDet-num {
    padding-left: 15rpx;
    font-size: $fg-f4;
    color: --color(--qui-FC-FFF);
  }
  .circleDet-share {
    font-size: $fg-f3;
    color: --color(--qui-FC-FFF);
  }
  .qui-icon {
    padding-right: 18rpx;
  }
  .bar-sticky {
    position: sticky;
    // position: -webkit-sticky;
    top: 0;
    z-index: 101;
  }
  .mask {
    position: fixed;
    top: 0;
    left: 0;
    z-index: 17;
    width: 100%;
    height: 100%;
    background: rgba(#000, 0.6);
  }
  .wxShareTip {
    position: fixed;
    top: 0;
    left: 0;
    z-index: 2222222222222;
    width: 100%;
    height: 100%;
    text-align: right;
    .sharePoint {
      display: inline-block;
      width: 70%;
      margin-top: 10rpx;
      margin-right: 30rpx;
    }
    .shareKnow {
      display: block;
      width: 35%;
      margin: 20vh auto 30rpx;
    }
  }
}
.header-h5 {
  height: 256rpx;
  background-size: cover;
  .logo {
    max-height: 74rpx;
    padding-top: 58rpx;
  }
  .circleDet {
    padding: 49rpx 20rpx 47rpx;
  }
}
</style>
