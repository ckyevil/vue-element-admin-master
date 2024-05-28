<template>
  <div class="comment-container">
    <el-input
      type="textarea"
      :rows="4"
      v-model="commentText"
      placeholder="Please input your comment"
    ></el-input>
    <el-button class="sub" type="primary" @click="submitComment">提交评论</el-button>
    <div class="comments-list">
      <div class="comment" v-for="(comment, index) in displayedComments" :key="index">
        <div class="comment-header">
          
          <div class="comment-author">{{ comment.author }}</div>
          <div class="comment-time">{{ comment.time | timeFormat }}</div>
        </div>
        <div class="comment-content">{{ comment.content }}</div>
        <div class="comment-actions">
          <el-button @click="likeComment(index)" type="text">
            <i class="el-icon-thumb"></i> Like {{ comment.likes }}
          </el-button>
          <el-button @click="dislikeComment(index)" type="text">
            <i class="el-icon-thumb-down"></i> Dislike {{ comment.dislikes }}
          </el-button>
        </div>
      </div>
    </div>
    <el-pagination
      @current-change="handleCurrentChange"
      :current-page="currentPage"
      :page-size="pageSize"
      layout="prev, pager, next"
      :total="comments.length">
    </el-pagination>
  </div>
</template>

<script>
export default {
  name: 'Comment',
  data() {
    return {
      commentText: '',
      comments: [
        // 示例评论列表，包含作者、内容、头像URL和时间戳
        { author: 'User1', content: '安全星盟平台的技术支持太棒了，很多问题都能得到及时响应和解决，给团队点赞！', time: '2024/5/26 15:22:22',likes: 3, dislikes: 0},
        { author: 'User2', content: '平台的社区氛围非常好，大家都乐于分享和交流，让我感受到了团队的力量！!', time: '2024/5/26 16:10:10',likes: 2, dislikes: 0 },
        { author: 'User2', content: '安全星盟平台真是个宝藏，大家都来使用~', time: '2024/5/26 16:22:01',likes: 5, dislikes: 0 },
        { author: 'User3', content: '这个平台设计的简单易上手，我太喜欢使用啦', time: '2024/5/26 18:12:32',likes: 3, dislikes: 0  },
        { author: 'User1', content: '大家觉得怎么样，有没有真实反馈？', time: '2024/5/27 13:32:14',likes: 99, dislikes: 0 },
        { author: 'Admin', content: '111我是平台的忠实粉丝 我为安全星盟打CALL', time:'2024/5/27 13:42:24',likes: 99, dislikes: 0  },
        { author: 'User1', content: '评论发晚了只能在第二页了', time: '2024/5/27 15:52:02',likes: 10, dislikes: 0 },
        { author: 'User1', content: '安全星盟加油  越来越好！！！', time: '2024/5/27 15:56:03',likes: 99, dislikes: 0  },
       
        // ...其他评论
      ],
      currentPage: 1,
      pageSize: 6,
    };
  },
  computed: {
    displayedComments() {
      let start = (this.currentPage - 1) * this.pageSize;
      let end = start + this.pageSize;
      return this.comments.slice(start, end);
    }
  },
  filters: {
    timeFormat(time) {
      // 格式化时间戳为易读的格式
      return new Date(time).toLocaleString();
    }
  },
  methods: {
    submitComment() {
      if (this.commentText.trim()) {
        const newComment = {
          author: 'Admin', // 假设提交评论的用户是Admin
          content: this.commentText,
          avatar: 'path/to/default-avatar.png', // 默认头像URL
          time: new Date().toLocaleString() // 当前时间
        };
        this.comments.push(newComment);
        this.commentText = ''; // 清空输入框
        this.currentPage = 1; // 新增评论后回到第一页
      }
    },
    handleCurrentChange(val) {
      this.currentPage = val; // 更新当前页码
    },
    likeComment(commentIndex) {
      this.comments[commentIndex].likes++;
    },
    dislikeComment(commentIndex) {
      this.comments[commentIndex].dislikes++;
    }
  }
};
</script>

<style scoped>
.sub {
  margin-top: 20px;
}
.comment-container {
  margin: 50px;
}
.comments-list {
  margin-top: 20px;
}
.comment {
  position: relative;
  border-bottom: 1px solid #ccc;
  padding: 10px;
}
.comment-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.comment-author {
  font-weight: bold;
}
.comment-time {
  font-size: 12px;
  color: #888;
}
.comment-content {
  margin-top: 10px;
}
.comment-actions {
  margin-top: 3px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.comment-actions button {
  margin-right: 0px;
}
</style>