<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <v-card class="pa-4 mb-6">
          <h2 class="mb-4">发布物品置换</h2>
          
          <v-form @submit.prevent="submitExchange">
            <v-row>
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="newExchange.title"
                  label="物品名称"
                  variant="outlined"
                  required
                ></v-text-field>
              </v-col>
              
              <v-col cols="12" md="6">
                <v-select
                  v-model="newExchange.category"
                  :items="categories"
                  label="物品类别"
                  variant="outlined"
                ></v-select>
              </v-col>
            </v-row>
            
            <v-textarea
              v-model="newExchange.description"
              label="物品描述"
              variant="outlined"
              auto-grow
              rows="3"
              required
            ></v-textarea>
            
            <v-row>
              <v-col cols="12" md="6">
                <v-select
                  v-model="newExchange.condition"
                  :items="conditions"
                  label="物品状况"
                  variant="outlined"
                ></v-select>
              </v-col>
              
              <v-col cols="12" md="6">
                <v-switch
                  v-model="newExchange.is_available"
                  label="是否可用"
                  inset
                ></v-switch>
              </v-col>
            </v-row>
            
            <v-file-input
              v-model="newExchange.images"
              label="上传物品图片"
              multiple
              prepend-icon="mdi-camera"
              variant="outlined"
            ></v-file-input>
            
            <v-btn 
              type="submit"
              color="primary" 
              class="mt-2" 
              :loading="posting"
              :disabled="!newExchange.title || !newExchange.description"
            >
              发布置换
            </v-btn>
          </v-form>
        </v-card>
      </v-col>
    </v-row>
    
    <v-row>
      <v-col cols="12">
        <v-card class="pa-4">
          <v-toolbar flat>
            <v-toolbar-title>待置换物品</v-toolbar-title>
            
            <v-spacer></v-spacer>
            
            <v-select
              v-model="filterCategory"
              :items="['全部', ...categories]"
              label="筛选类别"
              variant="outlined"
              density="compact"
              style="max-width: 200px;"
              @change="filterExchanges"
            ></v-select>
            
            <v-text-field
              v-model="searchQuery"
              label="搜索物品"
              prepend-inner-icon="mdi-magnify"
              variant="outlined"
              density="compact"
              style="max-width: 200px; margin-left: 16px;"
              @input="filterExchanges"
            ></v-text-field>
          </v-toolbar>
          
          <v-container>
            <v-row>
              <v-col 
                v-for="item in displayedExchanges" 
                :key="item.id" 
                cols="12" 
                sm="6" 
                md="4" 
                lg="3"
              >
                <v-card hover>
                  <v-img
                    v-if="item.image_urls"
                    :src="item.image_urls.split(',')[0]"
                    height="200px"
                    cover
                  ></v-img>
                  
                  <v-card-title class="pb-1">{{ item.title }}</v-card-title>
                  
                  <v-card-subtitle class="pb-0">
                    <v-chip 
                      :color="getCategoryColor(item.category)" 
                      label 
                      size="small"
                    >
                      {{ item.category }}
                    </v-chip>
                  </v-card-subtitle>
                  
                  <v-card-text class="pt-2">
                    <div class="text-subtitle-2 grey--text">状态：{{ item.condition }}</div>
                    <div class="text-body-2 mt-2">{{ truncateText(item.description, 80) }}</div>
                  </v-card-text>
                  
                  <v-card-actions>
                    <v-avatar size="32" class="mr-2">
                      <v-img :src="item.owner_avatar || 'https://cdn.vuetifyjs.com/images/john.jpg'" alt="Owner"></v-img>
                    </v-avatar>
                    
                    <div class="flex-grow-1 text-caption">
                      {{ item.owner_name || '未知' }} · {{ formatDate(item.created_at) }}
                    </div>
                    
                    <v-btn
                      icon
                      @click="contactOwner(item)"
                    >
                      <v-icon>mdi-message</v-icon>
                    </v-btn>
                  </v-card-actions>
                </v-card>
              </v-col>
            </v-row>
            
            <v-row justify="center" v-if="filteredExchanges.length === 0">
              <v-col cols="12" class="text-center">
                <p class="text-h6">没有找到匹配的置换物品</p>
              </v-col>
            </v-row>
          </v-container>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'ExchangeView',
  data() {
    return {
      posting: false,
      searchQuery: '',
      filterCategory: '全部',
      newExchange: {
        title: '',
        description: '',
        category: '电子产品',
        condition: '良好',
        images: [],
        is_available: true
      },
      categories: [
        '电子产品', 
        '图书教材', 
        '服装鞋帽', 
        '运动器材', 
        '生活用品', 
        '其他'
      ],
      conditions: [
        '全新', 
        '几乎全新', 
        '良好', 
        '一般', 
        '较差'
      ],
      exchanges: [
        {
          id: 1,
          title: 'MacBook Pro 2021',
          description: '9成新MacBook Pro，M1芯片，适合学生编程使用，性能强劲，电池健康度95%',
          category: '电子产品',
          condition: '良好',
          image_urls: 'https://images.unsplash.com/photo-1496181133206-80ce9b88a853?ixlib=rb-4.0.3&auto=format&fit=crop&w=600&q=80',
          owner_name: '张三',
          owner_avatar: 'https://cdn.vuetifyjs.com/images/john.jpg',
          created_at: '2023-09-15T10:30:00Z',
          is_available: true
        },
        {
          id: 2,
          title: '高等数学教材',
          description: '同济版高等数学上下册，购买时价格不菲，保存良好，有少量笔记',
          category: '图书教材',
          condition: '一般',
          image_urls: 'https://images.unsplash.com/photo-1544947950-fa07a98d237f?ixlib=rb-4.0.3&auto=format&fit=crop&w=600&q=80',
          owner_name: '李四',
          owner_avatar: 'https://cdn.vuetifyjs.com/images/john.jpg',
          created_at: '2023-09-10T14:20:00Z',
          is_available: true
        },
        {
          id: 3,
          title: '篮球',
          description: '斯伯丁官方比赛用球，仅使用过几次，保养得很好，表面无磨损',
          category: '运动器材',
          condition: '几乎全新',
          image_urls: 'https://images.unsplash.com/photo-1546519638-68e109498ffc?ixlib=rb-4.0.3&auto=format&fit=crop&w=600&q=80',
          owner_name: '王五',
          owner_avatar: 'https://cdn.vuetifyjs.com/images/john.jpg',
          created_at: '2023-09-18T09:15:00Z',
          is_available: true
        },
        {
          id: 4,
          title: '小米充电宝',
          description: '小米10000mAh充电宝，支持快充，有轻微划痕但不影响使用',
          category: '电子产品',
          condition: '良好',
          image_urls: 'https://images.unsplash.com/photo-1590972656981-bd72fdcb4bbd?ixlib=rb-4.0.3&auto=format&fit=crop&w=600&q=80',
          owner_name: '赵六',
          owner_avatar: 'https://cdn.vuetifyjs.com/images/john.jpg',
          created_at: '2023-09-20T16:45:00Z',
          is_available: false
        }
      ]
    };
  },
  computed: {
    filteredExchanges() {
      let result = this.exchanges;
      
      // 按类别筛选
      if (this.filterCategory !== '全部') {
        result = result.filter(item => item.category === this.filterCategory);
      }
      
      // 按关键词搜索
      if (this.searchQuery) {
        const query = this.searchQuery.toLowerCase();
        result = result.filter(item => 
          item.title.toLowerCase().includes(query) || 
          item.description.toLowerCase().includes(query)
        );
      }
      
      return result;
    },
    displayedExchanges() {
      // 返回前12个匹配的物品（用于演示）
      return this.filteredExchanges.slice(0, 12);
    }
  },
  methods: {
    async submitExchange() {
      this.posting = true;
      
      // 模拟API请求
      await new Promise(resolve => setTimeout(resolve, 1000));
      
      const exchange = {
        id: Date.now(),
        title: this.newExchange.title,
        description: this.newExchange.description,
        category: this.newExchange.category,
        condition: this.newExchange.condition,
        image_urls: 'https://images.unsplash.com/photo-1505740420928-5e560c06d30e?ixlib=rb-4.0.3&auto=format&fit=crop&w=600&q=80', // 示例图片
        owner_name: '当前用户',
        owner_avatar: 'https://cdn.vuetifyjs.com/images/john.jpg',
        created_at: new Date().toISOString(),
        is_available: this.newExchange.is_available
      };
      
      this.exchanges.unshift(exchange);
      
      this.newExchange = {
        title: '',
        description: '',
        category: '电子产品',
        condition: '良好',
        images: [],
        is_available: true
      };
      
      this.posting = false;
    },
    getCategoryColor(category) {
      const colors = {
        '电子产品': 'blue',
        '图书教材': 'green',
        '服装鞋帽': 'orange',
        '运动器材': 'red',
        '生活用品': 'purple',
        '其他': 'gray'
      };
      return colors[category] || 'gray';
    },
    truncateText(text, length) {
      if (!text) return '';
      return text.length > length ? text.substring(0, length) + '...' : text;
    },
    formatDate(dateString) {
      const date = new Date(dateString);
      return date.toLocaleDateString();
    },
    contactOwner(item) {
      console.log(`联系物品 ${item.title} 的拥有者 ${item.owner_name}`);
      // 这里可以实现与物主联系的功能
    },
    filterExchanges() {
      // 通过计算属性自动过滤
    }
  }
};
</script>