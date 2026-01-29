<template>
  <v-card class="mb-4">
    <v-card-title>
      <v-avatar size="40" class="mr-3">
        <v-img :src="post.avatar || 'https://cdn.vuetifyjs.com/images/john.jpg'" alt="Author"></v-img>
      </v-avatar>
      <div>
        <div class="font-weight-bold">{{ post.author_name }}</div>
        <div class="text-caption">{{ formatDate(post.created_at) }}</div>
      </div>
    </v-card-title>

    <v-card-text>
      <h3 class="headline mb-2">{{ post.title }}</h3>
      <p>{{ post.content }}</p>
      
      <!-- 显示图片 -->
      <div v-if="post.image_urls && post.image_urls.length > 0" class="mt-4">
        <v-carousel height="300" hide-delimiters>
          <v-carousel-item
            v-for="(image, index) in post.image_urls.split(',')"
            :key="index"
            :src="image.trim()"
            cover
          ></v-carousel-item>
        </v-carousel>
      </div>
    </v-card-text>

    <v-card-actions>
      <v-btn variant="text" @click="toggleLike(post)">
        <v-icon :color="post.liked ? 'red' : ''">
          {{ post.liked ? 'mdi-heart' : 'mdi-heart-outline' }}
        </v-icon>
        <span class="ml-1">{{ post.likes_count }}</span>
      </v-btn>
      
      <v-btn variant="text" @click="toggleComment">
        <v-icon>mdi-comment-outline</v-icon>
        <span class="ml-1">{{ post.comments_count }}</span>
      </v-btn>
      
      <v-spacer></v-spacer>
      
      <v-btn variant="text" @click="sharePost">
        <v-icon>mdi-share</v-icon>
      </v-btn>
    </v-card-actions>
    
    <!-- 评论区域 -->
    <v-expand-transition>
      <v-card-text v-show="showComments">
        <v-textarea
          v-model="newComment"
          label="写下你的评论..."
          auto-grow
          rows="1"
          variant="outlined"
          clearable
        ></v-textarea>
        <v-btn 
          class="mt-2" 
          color="primary" 
          @click="addComment"
          :disabled="!newComment.trim()"
        >
          发表评论
        </v-btn>
        
        <v-list class="mt-4">
          <v-list-item
            v-for="(comment, index) in post.comments || []"
            :key="index"
            class="mb-2"
          >
            <v-list-item-avatar>
              <v-img :src="comment.avatar || 'https://cdn.vuetifyjs.com/images/john.jpg'" alt="Commenter"></v-img>
            </v-list-item-avatar>
            
            <v-list-item-content>
              <v-list-item-title>{{ comment.author_name }}</v-list-item-title>
              <v-list-item-subtitle>{{ comment.content }}</v-list-item-subtitle>
            </v-list-item-content>
            
            <v-list-item-action>
              <v-btn icon x-small>
                <v-icon color="grey lighten-1">mdi-thumb-up-outline</v-icon>
              </v-btn>
            </v-list-item-action>
          </v-list-item>
        </v-list>
      </v-card-text>
    </v-expand-transition>
  </v-card>
</template>

<script>
export default {
  name: 'PostCard',
  props: {
    post: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      showComments: false,
      newComment: ''
    };
  },
  methods: {
    formatDate(dateString) {
      const date = new Date(dateString);
      return date.toLocaleString();
    },
    toggleLike(post) {
      post.liked = !post.liked;
      post.likes_count += post.liked ? 1 : -1;
    },
    toggleComment() {
      this.showComments = !this.showComments;
    },
    addComment() {
      if (!this.newComment.trim()) return;
      
      // 这里应该调用API添加评论
      const comment = {
        id: Date.now(),
        author_name: '当前用户',
        content: this.newComment,
        avatar: 'https://cdn.vuetifyjs.com/images/john.jpg'
      };
      
      if (!this.post.comments) {
        this.post.comments = [];
      }
      
      this.post.comments.push(comment);
      this.post.comments_count += 1;
      this.newComment = '';
    },
    sharePost() {
      // 分享功能
      console.log('分享帖子:', this.post.id);
    }
  }
};
</script>