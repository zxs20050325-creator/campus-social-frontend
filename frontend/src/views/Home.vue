<template>
  <v-container>
    <v-row>
      <v-col cols="12" md="8">
        <v-card class="pa-4 mb-6">
          <h2 class="mb-4">发布动态</h2>
          <v-text-field
            v-model="newPost.title"
            label="标题"
            variant="outlined"
            clearable
          ></v-text-field>
          
          <v-textarea
            v-model="newPost.content"
            label="内容"
            variant="outlined"
            auto-grow
            rows="3"
            clearable
          ></v-textarea>
          
          <v-file-input
            v-model="newPost.images"
            label="上传图片"
            multiple
            prepend-icon="mdi-camera"
            variant="outlined"
          ></v-file-input>
          
          <v-btn 
            color="primary" 
            class="mt-2" 
            @click="submitPost"
            :loading="posting"
            :disabled="!newPost.title || !newPost.content"
          >
            发布
          </v-btn>
        </v-card>

        <div v-for="post in posts" :key="post.id">
          <PostCard :post="post" />
        </div>
      </v-col>

      <v-col cols="12" md="4">
        <v-card class="pa-4 mb-6">
          <h3 class="mb-4">热门话题</h3>
          <v-chip-group>
            <v-chip v-for="tag in popularTags" :key="tag" variant="outlined">
              #{{ tag }}
            </v-chip>
          </v-chip-group>
        </v-card>

        <v-card class="pa-4 mb-6">
          <h3 class="mb-4">推荐用户</h3>
          <v-list>
            <v-list-item v-for="user in recommendedUsers" :key="user.id" class="mb-2">
              <v-list-item-avatar>
                <v-img :src="user.avatar" alt="User"></v-img>
              </v-list-item-avatar>
              
              <v-list-item-content>
                <v-list-item-title>{{ user.name }}</v-list-item-title>
                <v-list-item-subtitle>{{ user.bio }}</v-list-item-subtitle>
              </v-list-item-content>
              
              <v-list-item-action>
                <v-btn variant="text" color="primary">关注</v-btn>
              </v-list-item-action>
            </v-list-item>
          </v-list>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import PostCard from '@/components/PostCard.vue';

export default {
  name: 'HomeView',
  components: {
    PostCard
  },
  data() {
    return {
      posting: false,
      newPost: {
        title: '',
        content: '',
        images: []
      },
      posts: [
        {
          id: 1,
          title: '新学期开始了！',
          content: '大家好，新学期开始了，希望大家都能够开心快乐地度过每一天！',
          author_name: '张三',
          avatar: 'https://cdn.vuetifyjs.com/images/john.jpg',
          created_at: '2023-09-01T10:30:00Z',
          likes_count: 12,
          comments_count: 5,
          liked: false,
          image_urls: 'https://images.unsplash.com/photo-1506744038136-46273834b3fb?ixlib=rb-4.0.3&auto=format&fit=crop&w=600&q=80'
        },
        {
          id: 2,
          title: '寻找室友',
          content: '有没有同学想要找室友？本人性格开朗，爱干净，想找一个合得来的室友。',
          author_name: '李四',
          avatar: 'https://cdn.vuetifyjs.com/images/john.jpg',
          created_at: '2023-09-02T14:20:00Z',
          likes_count: 8,
          comments_count: 3,
          liked: true,
          image_urls: 'https://images.unsplash.com/photo-1493663284031-b7e3aefcae8e?ixlib=rb-4.0.3&auto=format&fit=crop&w=600&q=80'
        }
      ],
      popularTags: ['新生入学', '宿舍生活', '学习技巧', '社团活动', '校园美食'],
      recommendedUsers: [
        {
          id: 1,
          name: '王五',
          bio: '计算机专业，热爱编程',
          avatar: 'https://cdn.vuetifyjs.com/images/john.jpg'
        },
        {
          id: 2,
          name: '赵六',
          bio: '设计专业，喜欢画画',
          avatar: 'https://cdn.vuetifyjs.com/images/john.jpg'
        },
        {
          id: 3,
          name: '孙七',
          bio: '体育特长生，爱好运动',
          avatar: 'https://cdn.vuetifyjs.com/images/john.jpg'
        }
      ]
    };
  },
  methods: {
    async submitPost() {
      this.posting = true;
      
      // 模拟API请求
      await new Promise(resolve => setTimeout(resolve, 1000));
      
      const post = {
        id: Date.now(),
        title: this.newPost.title,
        content: this.newPost.content,
        author_name: '当前用户',
        avatar: 'https://cdn.vuetifyjs.com/images/john.jpg',
        created_at: new Date().toISOString(),
        likes_count: 0,
        comments_count: 0,
        liked: false
      };
      
      this.posts.unshift(post);
      
      this.newPost = {
        title: '',
        content: '',
        images: []
      };
      
      this.posting = false;
    }
  }
};
</script>