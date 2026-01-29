<template>
  <v-container>
    <v-row>
      <v-col cols="12" md="8">
        <v-card class="pa-4 mb-6">
          <h2 class="mb-4">发现朋友</h2>
          
          <v-text-field
            v-model="searchQuery"
            label="搜索用户"
            prepend-inner-icon="mdi-magnify"
            variant="outlined"
            clearable
            @input="searchUsers"
          ></v-text-field>
          
          <v-list three-line>
            <v-list-item
              v-for="user in filteredUsers"
              :key="user.id"
              class="mb-4"
            >
              <v-list-item-avatar size="60">
                <v-img :src="user.avatar" alt="User"></v-img>
              </v-list-item-avatar>
              
              <v-list-item-content>
                <v-list-item-title class="text-h6">
                  {{ user.name }}
                </v-list-item-title>
                <v-list-item-subtitle class="mb-2">
                  {{ user.bio }}
                </v-list-item-subtitle>
                
                <v-chip-group column>
                  <v-chip
                    v-for="interest in user.interests"
                    :key="interest"
                    variant="outlined"
                    size="small"
                  >
                    {{ interest }}
                  </v-chip>
                </v-chip-group>
              </v-list-item-content>
              
              <v-list-item-action>
                <v-btn 
                  :color="user.following ? 'grey' : 'primary'" 
                  @click="toggleFollow(user)"
                >
                  {{ user.following ? '已关注' : '关注' }}
                </v-btn>
              </v-list-item-action>
            </v-list-item>
          </v-list>
        </v-card>
      </v-col>

      <v-col cols="12" md="4">
        <v-card class="pa-4 mb-6">
          <h3 class="mb-4">好友列表</h3>
          <v-list>
            <v-list-item
              v-for="friend in friends"
              :key="friend.id"
              class="mb-2"
            >
              <v-list-item-avatar>
                <v-img :src="friend.avatar" alt="Friend"></v-img>
              </v-list-item-avatar>
              
              <v-list-item-content>
                <v-list-item-title>{{ friend.name }}</v-list-item-title>
                <v-list-item-subtitle>{{ friend.status }}</v-list-item-subtitle>
              </v-list-item-content>
              
              <v-list-item-action>
                <v-btn icon>
                  <v-icon>mdi-message-text-outline</v-icon>
                </v-btn>
              </v-list-item-action>
            </v-list-item>
          </v-list>
        </v-card>

        <v-card class="pa-4">
          <h3 class="mb-4">附近的人</h3>
          <v-list>
            <v-list-item
              v-for="nearby in nearbyUsers"
              :key="nearby.id"
              class="mb-2"
            >
              <v-list-item-avatar>
                <v-img :src="nearby.avatar" alt="Nearby User"></v-img>
              </v-list-item-avatar>
              
              <v-list-item-content>
                <v-list-item-title>{{ nearby.name }}</v-list-item-title>
                <v-list-item-subtitle>{{ nearby.distance }} km away</v-list-item-subtitle>
              </v-list-item-content>
              
              <v-list-item-action>
                <v-btn icon>
                  <v-icon>mdi-plus-circle-outline</v-icon>
                </v-btn>
              </v-list-item-action>
            </v-list-item>
          </v-list>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'SocialView',
  data() {
    return {
      searchQuery: '',
      users: [
        {
          id: 1,
          name: '张三',
          bio: '计算机科学专业学生，热爱编程和开源项目',
          avatar: 'https://cdn.vuetifyjs.com/images/john.jpg',
          interests: ['编程', '游戏', '科技'],
          following: false
        },
        {
          id: 2,
          name: '李四',
          bio: '艺术设计专业，擅长绘画和平面设计',
          avatar: 'https://cdn.vuetifyjs.com/images/john.jpg',
          interests: ['绘画', '摄影', '旅行'],
          following: true
        },
        {
          id: 3,
          name: '王五',
          bio: '商学院学生，对市场营销感兴趣',
          avatar: 'https://cdn.vuetifyjs.com/images/john.jpg',
          interests: ['商业', '营销', '创业'],
          following: false
        },
        {
          id: 4,
          name: '赵六',
          bio: '体育学院学生，国家二级运动员',
          avatar: 'https://cdn.vuetifyjs.com/images/john.jpg',
          interests: ['篮球', '健身', '户外'],
          following: false
        }
      ],
      friends: [
        {
          id: 1,
          name: '孙七',
          status: '在线',
          avatar: 'https://cdn.vuetifyjs.com/images/john.jpg'
        },
        {
          id: 2,
          name: '周八',
          status: '离线',
          avatar: 'https://cdn.vuetifyjs.com/images/john.jpg'
        },
        {
          id: 3,
          name: '吴九',
          status: '在线',
          avatar: 'https://cdn.vuetifyjs.com/images/john.jpg'
        }
      ],
      nearbyUsers: [
        {
          id: 1,
          name: '陈十',
          distance: 0.5,
          avatar: 'https://cdn.vuetifyjs.com/images/john.jpg'
        },
        {
          id: 2,
          name: '林十一',
          distance: 1.2,
          avatar: 'https://cdn.vuetifyjs.com/images/john.jpg'
        },
        {
          id: 3,
          name: '郑十二',
          distance: 2.3,
          avatar: 'https://cdn.vuetifyjs.com/images/john.jpg'
        }
      ]
    };
  },
  computed: {
    filteredUsers() {
      if (!this.searchQuery) return this.users;
      const query = this.searchQuery.toLowerCase();
      return this.users.filter(user => 
        user.name.toLowerCase().includes(query) || 
        user.bio.toLowerCase().includes(query) ||
        user.interests.some(interest => interest.toLowerCase().includes(query))
      );
    }
  },
  methods: {
    toggleFollow(user) {
      user.following = !user.following;
    },
    searchUsers() {
      // 计算属性会自动更新结果
    }
  }
};
</script>