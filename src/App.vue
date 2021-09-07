<template>
  <div id="app">
    <!-- 发表 -->
    <div class="publish_comment_container">
      <textarea class="comment" v-model="commentValue"></textarea>
      <button @click="handleCommit">add comment</button>
    </div>

    <!-- 展示 -->
    <div class="show_comment_container">
      <ul>
        <li v-for="(item, index) in commentLists" :key="index">
          <div class="main">
            <comment-item :comment-item="item" @open="handleOpen">
              <div>
                <comment-item
                  :comment-item="subItem"
                  class="sub"
                  v-for="(subItem, subIndex) in item.reply"
                  :key="subIndex"
                ></comment-item>
              </div>
            </comment-item>
          </div>
        </li>
      </ul>
    </div>

    <!-- 回复输入框 -->
    <div class="reply_comment_container" :class="[reply ? 'show' : 'hide']">
      <textarea class="replay" v-model="replyComment"></textarea>
      <div class="btn_container">
        <button class="reply_btn" @click="handleReply">回复</button>
        <button class="close_btn" @click="handleClose">关闭</button>
      </div>
    </div>
  </div>
</template>

<script>
import CommentItem from "@/components/commentItem.vue";
export default {
  name: "App",
  components: {
    CommentItem,
  },
  data() {
    return {
      commentValue: "",
      replyComment: "",
      commentLists: [],
      subCommentLists: [],
      reply: false,
      index: null,
    };
  },
  created() {
    this.getCommentLists();
  },

  methods: {
    handleOpen(i) {
      this.index = this.commentLists.indexOf(i);
      this.reply = true;
    },
    handleClose() {
      this.reply = false;
    },
    getCommentLists() {
      this.commentLists = JSON.parse(localStorage.getItem("commentLists"));
    },
    handleReply() {
      if (!this.replyComment) {
        alert("内容不可为空");
        return;
      }
      const subCommentLists = this.commentLists[this.index].reply;
      let subCommentObject = {
        name: "subname",
        time: new Date().getTime(),
        comment: this.replyComment,
      };

      subCommentLists.push(subCommentObject);
      localStorage.setItem("commentLists", JSON.stringify(this.commentLists));

      this.replyComment = "";
      this.reply = false;
      this.getCommentLists();
    },

    handleCommit() {
      const { commentValue } = this;
      if (!commentValue) {
        alert("内容不可为空");
        return;
      }
      const commentLists =
        JSON.parse(localStorage.getItem("commentLists")) || [];
      let commentObject = {
        id: commentLists.length + 1,
        name: "username",
        time: new Date().getTime(),
        comment: commentValue,
        reply: [],
      };
      commentLists.push(commentObject);
      localStorage.setItem("commentLists", JSON.stringify(commentLists));

      this.getCommentLists();
      this.commentValue = "";
    },
  },
};
</script>

<style>
body {
  padding: 0;
  margin: 0;
}
#app {
  min-height: 100vh;
  background: #f7f1e3;
  padding: 0 20px;
}

.comment {
  width: 600px;
  height: 150px;
  border: 1px solid #ccc;
  outline: none;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 14px;
}

button {
  display: block;
  background-color: #fff;
  border: 1px solid #ccc;
  border-radius: 3px;
}

.show_comment_container {
  margin-top: 50px;
}

ul {
  margin: 0;
  padding: 0;
}

li {
  list-style: none;
}

.reply_comment_container {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  width: 600px;
  box-sizing: border-box;
  padding: 16px 10px 0;
  border: 1px solid #ccc;
  background-color: #f7f1e3;
  border-radius: 6px;
}

.sub {
  margin-left: 30px;
}

.replay {
  width: 100%;
  height: 150px;
  box-sizing: border-box;
  border: 1px solid #ccc;
  outline: none;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 14px;
}
.btn_container {
  width: 100%;
  display: flex;
  margin: 10px 0;
  justify-content: flex-end;
}

.close_btn {
  margin-left: 10px;
}

.hide {
  display: none;
}
</style>