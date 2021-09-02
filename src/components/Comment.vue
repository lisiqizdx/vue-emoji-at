<template>
  <div class="comment-wrap">
    <div class="comments-list">
      <div
        class="comments-list-item"
        v-for="(item, index) in comments"
        v-bind:key="index"
      >
        <div class="comments-list-item-heading">
          <img src="../assets/img/heading.jpg" />
          <span class="comments-list-item-username">pppercywang</span>
        </div>
        <div class="comments-list-item-content" v-html="item"></div>
      </div>
    </div>
    <!-- <textarea class="comment-input" placeholder="请输入内容" id="textpanel" v-model="content"></textarea> -->
     <!-- @keyup.shift.50="change" -->
  <at :members="members" name-key="name"  @at="handleAt">
    <template slot="item" slot-scope="s">
      <span v-text="s.item.name" ></span>
    </template>
      <div
      
        class="comment-input editor"
        contenteditable="true"
        id="textpanel"
        v-html="content"
       
      ></div>
  </at>
    <div class="opration">
      <div class="emoji-panel-btn" @click="showEmojiPanel">
        <img src="../assets/img/face_logo.png" />
      </div>
      <div
        @click="saveComment"
        class="comment-send-btn comment-send-btn-bounce"
      >
        发表评论
      </div>
      <emoji-panel
        @emojiClick="appendEmoji"
        v-if="isShowEmojiPanel"
      ></emoji-panel>
    </div>
  </div>
</template>
<script>
import EmojiPanel from "./children/EmojiPanel.vue";
import At from "vue-at";
export default {
  data() {
    return {
      content: "",
      flag: false,
      isShowEmojiPanel: false,
      comments: [],
      innerText: "",
      content2: "",
      members: [
        {
          name: "啊哲",
        },
        {
          name: "椿木",
        },
      ],
    };
  },
  components: {
    EmojiPanel,
    At,
  },
  watch:{
    "el.textContent":{
      handler(newval,oldval){
        console.log(newval,oldval)
      },
      deep: true,
    }
  },
  methods: {
    handleAt(chunk){  
      console.log(chunk)
    },
    saveComment() {
      const el = document.getElementById("textpanel");
      console.log(el.innerHTML);
      
      this.comments.push(el.innerHTML); // 替换":"符号包含的字符串,通过emoji方法生成表情<span></span>
      console.log(this.comments);

      this.content = "";
      this.isShowEmojiPanel = false;
    },
    showEmojiPanel() {
      this.isShowEmojiPanel = !this.isShowEmojiPanel;
      const el = document.getElementById("textpanel");
      console.log(el.innerHTML);
    },
    emoji(word) {
      console.log(word)
      const img = require('../assets/img/opcity.jpeg')
      // 生成html
      const type = word.substring(1, word.length - 1);
      return `<img src='${img}' class="emoji-item-common emoji-${type} emoji-size-small" ></img>`;
    },
    appendEmoji(text) {
      const el = document.getElementById("textpanel");
      this.content2 = el.innerHTML + ":" + text + ":";
      this.content = this.content2.replace(/:.*?:/g, this.emoji);

      console.log(el.innerHTML);
    },
  },
};
</script>
<style  lang="scss">
// 注意 这里因为v-html的原因 不能使用scoped 不然样式不能失效
.comment-wrap {
  width: 522px;
  margin-bottom: 180px;
  .emoji-item-common {
    background: url("../assets/img/emoji_sprite.png");
    display: inline-block;
    &:hover {
      cursor: pointer;
    }
  }
  .color{
    color: #1da1f2;
    margin: 0;
    min-width: 10px;
  }
  .editor {
    white-space: pre-wrap; // 保留空白符
  }
  .emoji-size-small {
    // 表情大小
    zoom: 0.3;
  }
  .emoji-size-large {
    zoom: 0.5; // emojipanel表情大小
    margin: 4px;
  }
  .comments-list {
    margin-top: 20px;
    .comments-list-item {
      margin-bottom: 20px;
      .comments-list-item-heading {
        display: inline-block;
        img {
          height: 32px;
          width: 32px;
          border-radius: 50%;
          vertical-align: middle;
        }
        .comments-list-item-username {
          margin-left: 5px;
          font-weight: bold;
        }
      }
      .comments-list-item-content {
        margin: 10px 0px;
        border-bottom: 1px solid #cccccc;
        &:last-child {
          border-bottom: 0;
        }
        span {
          vertical-align: top;
        }
      }
    }
  }
  .comment-input {
    height: 100px;
    width: 500px;
    border: 1px solid #cccccc;
    border-radius: 5px;
    padding: 10px;
    resize: none;
    &:focus {
      outline: none;
    }
  }
  .opration {
    width: 522px;
    display: flex;
    justify-content: space-between;
    position: relative;
    .emoji-panel-btn {
      &:hover {
        cursor: pointer;
        opacity: 0.8;
      }
      img {
        height: 24px;
        width: 24px;
        border: 0;
      }
    }
    .comment-send-btn {
      width: 80px;
      height: 30px;
      line-height: 30px;
      text-align: center;
      border: 1px solid #1da1f2;
      border-radius: 100px;
      box-sizing: border-box;
      font-weight: bold;
      font-size: 13px;
      color: #ffffff;
      background-color: #4ab3f4;
      &:hover {
        cursor: pointer;
      }
    }
    .comment-send-btn-bounce {
      position: relative;
    }
    .comment-send-btn-bounce:focus {
      outline: none;
    }
    .comment-send-btn-bounce:after {
      content: "";
      display: block;
      position: absolute;
      top: 0px;
      left: 0px;
      right: 0px;
      bottom: 0px;
      border-radius: 100px;
      border: 0px solid #1da1f2;
      opacity: 0.7;
      transition: all 0.1s;
    }
    .comment-send-btn-bounce:active:after {
      //.bounce active时 伪元素:after的样式
      opacity: 1;
      top: -5px;
      left: -5px;
      right: -5px;
      bottom: -5px;
      border-radius: 100px;
      border: 2px solid #1da1f2;
      transition: all 0.2s;
    }
  }
}
@import "../assets/css/emoji.css"; // 导入精灵图样式
</style>