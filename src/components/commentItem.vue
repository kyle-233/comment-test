<template>
  <div class="comment-item">
    <div class="comment_info">
      <span class="icon-1" :class="[collapse ? 'down' : '']"></span>
      <p class="comment_user_name">{{ commentItem.name }}</p>
      <p class="comment_publish_time">{{ commentItem.time | formatTime }}</p>
      <span
        class="icon_2"
        :title="collapse ? '展开' : '收起'"
        @click="handleCollapse"
        >[<span class="icon_line" :class="[collapse ? '' : 'row']"></span
        >]</span
      >
    </div>
    <div class="comment_detail" v-show="!collapse">
      <p class="comment_text">
        {{ commentItem.comment }}
      </p>
      <a href="javascript:;" @click="mainReply" v-if="commentItem.id">回复</a>
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "commentItem",
  props: {
    commentItem: {
      type: Object,
      default() {
        return {};
      },
    },
  },
  data() {
    return {
      collapse: false,
    };
  },
  filters: {
    formatTime(time) {
      let now = new Date().getTime();
      let differ = Math.floor((now - time) / 1000);
      let day = Math.floor(differ / (3600 * 24));
      if (day >= 1) return `${day}天前`;
      let hours_differ = differ % (3600 * 24);
      let hours = Math.floor(hours_differ / 3600);
      if (hours >= 1) return `${hours}小时前`;
      let minu_differ = hours_differ % 3600;
      let minu = Math.floor(minu_differ / 60);
      if (minu >= 1) return `${minu}分钟前`;
      let second_differ = minu_differ % 60;
      let seconds = Math.floor(second_differ / 60);
      return `${seconds + 1}秒前`;
    },
  },
  methods: {
    handleCollapse() {
      this.collapse = !this.collapse;
    },

    mainReply() {
      this.$emit("open", this.commentItem);
    },
  },
};
</script>

<style scoped>
.comment_info {
  display: flex;
  font-size: 12px;
  color: #555;
  /* align-items: center; */
}

.icon-1 {
  position: relative;
  display: block;
  width: 0;
  height: 0;
  border-left: 3px solid transparent;
  border-right: 3px solid transparent;
  border-bottom: 5px solid #555;
  top: 7px;
}

.down {
  transform: rotate(180deg);
}

.icon_2 {
  margin-left: 5px;
  cursor: pointer;
}

p {
  margin: 0;
}

.comment_user_name {
  margin-left: 3px;
}

.comment_publish_time {
  margin-left: 8px;
}

.icon_line {
  display: inline-block;
  background-color: #555;
  width: 1px;
  height: 8px;
  margin: 0 4px;
}

.row {
  transform: rotate(90deg);
}

.comment_detail {
  margin: 4px 0 0 10px;
}

.comment_text {
  font-size: 14px;
  line-height: 21px;
}

.comment_detail a {
  font-size: 12px;
  color: #555;
}
</style>