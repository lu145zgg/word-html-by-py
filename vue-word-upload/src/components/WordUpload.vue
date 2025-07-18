<template>
  <div>
    <h2>Upload a Word file</h2>
    <input type="file" @change="onFileChange" />
    <button @click="uploadFile">Upload</button>

    <h3>Converted HTML:</h3>
    <div v-if="htmlContent" v-html="htmlContent"></div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      file: null,
      htmlContent: null,
    };
  },
  methods: {
    // 处理文件选择
    onFileChange(event) {
      this.file = event.target.files[0];
    },
    // 上传文件并接收 HTML 内容
    uploadFile() {
      if (!this.file) {
        alert("Please select a file first.");
        return;
      }

      const formData = new FormData();
      formData.append('file', this.file);

      // 发送 POST 请求到后端，上传文件并接收转换后的 HTML 内容
      axios
        .post('http://localhost:3000/upload', formData, {
          headers: {
            'Content-Type': 'multipart/form-data',
          },
        })
        .then((response) => {
          this.htmlContent = response.data;  // 显示转换后的 HTML
          console.log('Converted HTML:', response.data);
        })
        .catch((error) => {
          console.error('Error uploading file:', error.response || error.message);
        });
    },
  },
};
</script>

<style scoped>
/* 添加一些样式来美化界面 */
button {
  margin-top: 10px;
  padding: 8px 16px;
  font-size: 14px;
  background-color: #4CAF50;
  color: white;
  border: none;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}

input[type="file"] {
  margin-top: 10px;
  padding: 5px;
}

h2 {
  color: #333;
}

h3 {
  color: #4CAF50;
}
</style>
