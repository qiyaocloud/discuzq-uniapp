<template>
  <view class="like-page">
    <qui-thread-item
      v-for="(item, index) in data"
      :key="index"
      :currentindex="index"
      :thread="item"
      @toTopic="toTopic"
      @greatCallBack="greatCallBack"
      @handleClickShare="handleClickShare"
    ></qui-thread-item>
    <qui-load-more :status="loadingType" :show-icon="false" v-if="loadingType"></qui-load-more>
  </view>
</template>

<script>
import { status } from '@/library/jsonapi-vuex/index';

export default {
  props: {
    userId: {
      type: String,
      default: '',
    },
  },
  data() {
    return {
      loadingType: '',
      data: [],
      pageSize: 20,
      pageNum: 1, // 当前页数
      editThreadId: '',
      currentLoginId: this.$store.getters['session/get']('userId'),
    };
  },
  mounted() {
    this.loadlikes();
  },
  methods: {
    toTopic(id) {
      this.editThreadId = id;
    },
    handleClickShare(id) {
      this.$emit('handleClickShare', id);
    },
    pullDownRefresh() {
      this.pageNum = 1;
      this.data = [];
      this.loadlikes('pullDownRefresh');
    },
    // 加载当前点赞数据
    loadlikes(type) {
      this.loadingType = 'loading';
      const params = {
        include: [
          'user',
          'user.groups',
          'firstPost',
          'firstPost.images',
          'firstPost.postGoods',
          'category',
          'threadVideo',
          'threadAudio',
          'question',
          'question.beUser',
          'question.beUser.groups',
        ],
        'page[number]': this.pageNum,
        'page[limit]': this.pageSize,
        'filter[isApproved]': 1,
        'filter[user_id]': this.userId,
      };
      status
        .run(() => this.$store.dispatch('jv/get', ['threads/likes', { params }]))
        .then(res => {
          if (res._jv) {
            delete res._jv;
          }
          this.loadingType = res.length === this.pageSize ? 'more' : 'nomore';
          this.data = [...this.data, ...res];
          if (type && type === 'pullDownRefresh') {
            uni.stopPullDownRefresh();
          }
        });
    },
    greatCallBack(isLiked, index) {
      if (isLiked && this.currentLoginId === this.userId) {
        const data = JSON.parse(JSON.stringify(this.data));
        data.splice(index, 1);
        this.data = data;
        this.$emit('changeFollow', { userId: this.userId });
      }
    },
    // 下拉加载
    pullDown() {
      if (this.loadingType !== 'more') {
        return;
      }
      this.pageNum += 1;
      this.loadlikes();
    },
    uploadItem() {
      if (!this.editThreadId) {
        return;
      }
      const item = this.$store.getters['jv/get'](`threads/${this.editThreadId}`);
      this.data.forEach((data, index) => {
        if (data._jv.id === this.editThreadId) {
          this.editThreadId = '';
          this.$set(this.data, index, item);
        }
      });
    },
  },
};
</script>
<style lang="scss">
.like-page {
  margin-top: -20rpx;
}
</style>
