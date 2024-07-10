<template>
  <el-container style="height: 100vh;">
    <el-container>
      <el-main>
        <el-form :model="form" :rules="rules" ref="formRef" label-width="120px">
          <el-form-item label="SDK Name" prop="sdkName">
            <el-input v-model="form.sdkName" />
          </el-form-item>
          <el-form-item label="Category" prop="category">
            <el-select v-model="form.category" placeholder="Select Category">
              <el-option label="生活服务" value="生活服务" />
              <el-option label="安全" value="安全" />
              <!-- Add other categories as needed -->
            </el-select>
          </el-form-item>
          <el-form-item label="Icon File" prop="iconFile">
            <el-upload
              action=""
              :before-upload="handleIconUpload"
              :file-list="iconFileList"
              :on-remove="handleIconRemove"
              :limit="1"
              list-type="picture"
            >
              <el-button size="small" type="primary">点击上传</el-button>
              <div slot="tip" class="el-upload__tip">只允许上传一个文件</div>
            </el-upload>
          </el-form-item>
          <el-form-item label="HAR File" prop="harFile">
            <el-upload
              action=""
              :before-upload="handleHarUpload"
              :file-list="harFileList"
              :on-remove="handleHarRemove"
              :limit="1"
              list-type="picture"
            >
              <el-button size="small" type="primary">点击上传</el-button>
              <div slot="tip" class="el-upload__tip">只允许上传一个文件</div>
            </el-upload>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="submitForm">提交</el-button>
          </el-form-item>
        </el-form>
      </el-main>
    </el-container>
  </el-container>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import axios from 'axios';
import { ElForm, ElMessage } from 'element-plus';
import router from '@/router';

export default defineComponent({
  name: 'UploadSdk',
  setup() {
    const form = ref({
      sdkName: '',
      category: '',
      iconFile: null as File | null,
      harFile: null as File | null,
    });

    const formRef = ref<InstanceType<typeof ElForm> | null>(null);

    const rules = {
      sdkName: [{ required: true, message: 'Please input SDK Name', trigger: 'blur' }],
      category: [{ required: true, message: 'Please select a category', trigger: 'change' }],
      iconFile: [{ required: true, message: 'Please upload an icon file', trigger: 'change' }],
      harFile: [{ required: true, message: 'Please upload a HAR file', trigger: 'change' }],
    };

    const iconFileList = ref<any[]>([]);
    const harFileList = ref<any[]>([]);

    const handleIconUpload = (file: File) => {
      form.value.iconFile = file;
      return false;
    };

    const handleHarUpload = (file: File) => {
      form.value.harFile = file;
      return false;
    };

    const handleIconRemove = () => {
      form.value.iconFile = null;
    };

    const handleHarRemove = () => {
      form.value.harFile = null;
    };

    const submitForm = () => {
      if (formRef.value) {
        formRef.value.validate(async (valid: boolean) => {
          if (valid) {
            const formData = new FormData();
            formData.append('sdkName', form.value.sdkName);
            formData.append('category', form.value.category);
            formData.append('icon', form.value.iconFile as Blob);
            formData.append('har', form.value.harFile as Blob);

            try {
              const response = await axios.post('http://localhost:8080/api/upload', formData, {
                headers: {
                  'Content-Type': 'multipart/form-data',
                },
              });

              if (response.data.status === 200) {
                ElMessage.success('Upload successful');
              } else {
                ElMessage.error('Upload failed: ' + response.data.message);
              }
            } catch (error: any) {
              ElMessage.error('Upload failed: ' + error.message);
            }
          } else {
            ElMessage.error('Please fill in all fields and select files.');
          }
        });
      }
    };

    const handleMenuSelect = (index: string) => {
      if (index === 'home') {
        router.push('/');
      } else if (index === 'uploadSdk') {
        router.push('/upload-sdk');
      } else if (index === 'detectTask') {
        router.push('/detect-tasks');
      } else if (index === 'sdkList') {
        router.push('/sdk-list');
      } else if (index === 'permissionSettings') {
        router.push('/permission-settings');
      }
    };

    return {
      form,
      formRef,
      rules,
      iconFileList,
      harFileList,
      handleIconUpload,
      handleHarUpload,
      handleIconRemove,
      handleHarRemove,
      submitForm,
      handleMenuSelect,
      activeMenu: ref('uploadSdk'),
    };
  },
});
</script>

<style scoped>
.el-main {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 20px;
  width: 100%;
  overflow: auto;
}

.el-form {
  width: 100%;
  max-width: 800px;
}
</style>