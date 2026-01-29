<template>
  <v-container>
    <v-row justify="center">
      <v-col cols="12" md="8">
        <v-card class="pa-6">
          <h2 class="mb-6 text-center">发布新动态</h2>
          
          <v-form @submit.prevent="submitPost">
            <v-text-field
              v-model="postForm.title"
              label="标题"
              variant="outlined"
              required
              class="mb-4"
            ></v-text-field>
            
            <v-textarea
              v-model="postForm.content"
              label="内容"
              variant="outlined"
              auto-grow
              rows="5"
              required
              class="mb-4"
            ></v-textarea>
            
            <v-file-input
              v-model="postForm.images"
              label="上传图片"
              multiple
              prepend-icon="mdi-camera"
              variant="outlined"
              class="mb-4"
            ></v-file-input>
            
            <v-select
              v-model="postForm.visibility"
              :items="visibilityOptions"
              label="可见性"
              variant="outlined"
              class="mb-4"
            ></v-select>
            
            <div class="d-flex justify-end">
              <v-btn 
                color="secondary" 
                @click="$router.go(-1)" 
                class="mr-4"
              >
                取消
              </v-btn>
              <v-btn 
                type="submit"
                color="primary" 
                :loading="posting"
                :disabled="!postForm.title || !postForm.content"
              >
                发布动态
              </v-btn>
            </div>
          </v-form>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'PostView',
  data() {
    return {
      posting: false,
      postForm: {
        title: '',
        content: '',
        images: [],
        visibility: 'public'
      },
      visibilityOptions: [
        { text: '公开', value: 'public' },
        { text: '仅关注的人', value: 'followers' },
        { text: '仅自己', value: 'private' }
      ]
    };
  },
  methods: {
    async submitPost() {
      if (!this.postForm.title || !this.postForm.content) {
        alert('请填写标题和内容');
        return;
      }

      this.posting = true;

      // 模拟API请求
      await new Promise(resolve => setTimeout(resolve, 1500));

      // 这里应该是实际的API调用
      console.log('提交动态:', this.postForm);

      // 提交成功后返回首页
      this.$router.push('/');
      
      this.posting = false;
    }
  }
};
</script>