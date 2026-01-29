<template>
  <v-container>
    <v-row>
      <v-col cols="12" md="4">
        <v-card>
          <v-card-title class="headline">个人信息</v-card-title>
          
          <v-card-text>
            <v-avatar size="100" class="mb-4">
              <v-img :src="user.avatar" alt="Avatar"></v-img>
            </v-avatar>
            
            <div class="text-h6">{{ user.name }}</div>
            <div class="text-subtitle-1 grey--text">{{ user.email }}</div>
            
            <v-divider class="my-4"></v-divider>
            
            <div class="mb-2">
              <strong>用户名:</strong> {{ user.username }}
            </div>
            <div class="mb-2">
              <strong>简介:</strong> {{ user.bio || '暂无简介' }}
            </div>
            <div class="mb-2">
              <strong>注册时间:</strong> {{ formatDate(user.created_at) }}
            </div>
          </v-card-text>
          
          <v-card-actions>
            <v-btn color="primary" block @click="editProfileDialog = true">编辑资料</v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
      
      <v-col cols="12" md="8">
        <v-card>
          <v-card-title class="headline">我的动态</v-card-title>
          
          <v-card-text>
            <div v-if="userPosts.length === 0" class="text-center py-8">
              <p class="text-h6">还没有发布任何动态</p>
              <v-btn color="primary" to="/post" class="mt-4">发布第一篇动态</v-btn>
            </div>
            
            <div v-else>
              <div v-for="post in userPosts" :key="post.id" class="mb-4">
                <v-card outlined>
                  <v-card-title>{{ post.title }}</v-card-title>
                  <v-card-subtitle>{{ formatDate(post.created_at) }}</v-card-subtitle>
                  <v-card-text>{{ post.content }}</v-card-text>
                  
                  <v-card-actions>
                    <v-btn variant="text" @click="toggleLike(post)">
                      <v-icon :color="post.liked ? 'red' : ''">
                        {{ post.liked ? 'mdi-heart' : 'mdi-heart-outline' }}
                      </v-icon>
                      <span class="ml-1">{{ post.likes_count }}</span>
                    </v-btn>
                    
                    <v-btn variant="text">
                      <v-icon>mdi-comment-outline</v-icon>
                      <span class="ml-1">{{ post.comments_count }}</span>
                    </v-btn>
                    
                    <v-spacer></v-spacer>
                    
                    <v-btn variant="text" @click="deletePost(post.id)">
                      <v-icon color="error">mdi-delete</v-icon>
                    </v-btn>
                  </v-card-actions>
                </v-card>
              </div>
            </div>
          </v-card-text>
        </v-card>
        
        <v-card class="mt-6">
          <v-card-title class="headline">我的置换物品</v-card-title>
          
          <v-card-text>
            <div v-if="userExchanges.length === 0" class="text-center py-8">
              <p class="text-h6">还没有发布任何置换物品</p>
              <v-btn color="primary" to="/exchange" class="mt-4">发布置换物品</v-btn>
            </div>
            
            <div v-else>
              <v-data-table
                :headers="exchangeHeaders"
                :items="userExchanges"
                class="elevation-1"
              >
                <template v-slot:item.image_urls="{ item }">
                  <v-img 
                    v-if="item.image_urls" 
                    :src="item.image_urls.split(',')[0]" 
                    max-height="60" 
                    max-width="60" 
                    class="rounded"
                  ></v-img>
                </template>
                <template v-slot:item.actions="{ item }">
                  <v-btn icon @click="editExchange(item)">
                    <v-icon>mdi-pencil</v-icon>
                  </v-btn>
                  <v-btn icon @click="deleteExchange(item.id)">
                    <v-icon color="error">mdi-delete</v-icon>
                  </v-btn>
                </template>
              </v-data-table>
            </div>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
    
    <!-- 编辑资料对话框 -->
    <v-dialog v-model="editProfileDialog" max-width="500">
      <v-card>
        <v-card-title>编辑资料</v-card-title>
        
        <v-card-text>
          <v-form ref="profileForm">
            <v-text-field
              v-model="editedUser.username"
              label="用户名"
              variant="outlined"
              required
            ></v-text-field>
            
            <v-text-field
              v-model="editedUser.name"
              label="姓名"
              variant="outlined"
              required
            ></v-text-field>
            
            <v-textarea
              v-model="editedUser.bio"
              label="个人简介"
              variant="outlined"
              auto-grow
              rows="2"
            ></v-textarea>
            
            <v-file-input
              v-model="editedUser.avatarFile"
              label="头像"
              prepend-icon="mdi-camera"
              variant="outlined"
            ></v-file-input>
          </v-form>
        </v-card-text>
        
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="secondary" @click="editProfileDialog = false">取消</v-btn>
          <v-btn color="primary" @click="saveProfile">保存</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
export default {
  name: 'ProfileView',
  data() {
    return {
      editProfileDialog: false,
      user: {
        id: 1,
        name: '当前用户',
        username: 'current_user',
        email: 'user@example.com',
        bio: '这是一个个人简介示例',
        avatar: 'https://cdn.vuetifyjs.com/images/john.jpg',
        created_at: '2023-01-15T10:30:00Z'
      },
      editedUser: {
        username: '',
        name: '',
        bio: '',
        avatarFile: null
      },
      userPosts: [
        {
          id: 1,
          title: '我的大学生活',
          content: '今天是开学第一天，认识了很多新朋友，期待这学期的精彩生活！',
          created_at: '2023-09-01T10:30:00Z',
          likes_count: 12,
          comments_count: 5,
          liked: false
        },
        {
          id: 2,
          title: '推荐一本好书',
          content: '最近读了《百年孤独》，马尔克斯的魔幻现实主义真的很棒，值得推荐给大家。',
          created_at: '2023-09-10T15:45:00Z',
          likes_count: 8,
          comments_count: 3,
          liked: true
        }
      ],
      userExchanges: [
        {
          id: 1,
          title: '旧教材出售',
          description: '大一使用的高数、英语等教材，几乎全新',
          category: '图书教材',
          condition: '几乎全新',
          image_urls: 'https://images.unsplash.com/photo-1544947950-fa07a98d237f?ixlib=rb-4.0.3&auto=format&fit=crop&w=600&q=80',
          created_at: '2023-09-15T10:30:00Z',
          is_available: true
        },
        {
          id: 2,
          title: '二手自行车',
          description: '仅使用了一个学期的山地车，性能良好',
          category: '交通工具',
          condition: '良好',
          image_urls: 'https://images.unsplash.com/photo-1532298229144-0ec0c57515c7?ixlib=rb-4.0.3&auto=format&fit=crop&w=600&q=80',
          created_at: '2023-09-18T14:20:00Z',
          is_available: false
        }
      ],
      exchangeHeaders: [
        { title: '图片', key: 'image_urls', sortable: false },
        { title: '物品名称', key: 'title' },
        { title: '类别', key: 'category' },
        { title: '状态', key: 'condition' },
        { title: '操作', key: 'actions', sortable: false }
      ]
    };
  },
  mounted() {
    this.resetEditUser();
  },
  methods: {
    resetEditUser() {
      this.editedUser = {
        username: this.user.username,
        name: this.user.name,
        bio: this.user.bio,
        avatarFile: null
      };
    },
    formatDate(dateString) {
      const date = new Date(dateString);
      return date.toLocaleDateString();
    },
    toggleLike(post) {
      post.liked = !post.liked;
      post.likes_count += post.liked ? 1 : -1;
    },
    deletePost(postId) {
      if (confirm('确定要删除这个动态吗？')) {
        this.userPosts = this.userPosts.filter(post => post.id !== postId);
      }
    },
    deleteExchange(exchangeId) {
      if (confirm('确定要删除这个置换物品吗？')) {
        this.userExchanges = this.userExchanges.filter(ex => ex.id !== exchangeId);
      }
    },
    editExchange(exchange) {
      console.log('编辑物品:', exchange);
      // 这里可以打开编辑对话框
    },
    saveProfile() {
      // 更新用户信息
      this.user.username = this.editedUser.username;
      this.user.name = this.editedUser.name;
      this.user.bio = this.editedUser.bio;
      
      if (this.editedUser.avatarFile) {
        // 这里应该是上传头像的逻辑
        console.log('上传新头像:', this.editedUser.avatarFile);
      }
      
      this.editProfileDialog = false;
      // 这里应该是实际的API调用
      console.log('更新用户资料:', this.user);
    }
  }
};
</script>