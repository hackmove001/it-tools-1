<template>
  <n-card title="Base64 to file">
    <n-input v-model:value="base64Input" type="textarea" placeholder="Put your base64 file string here..." rows="5" />
    <n-space justify="center">
      <n-button secondary @click="download()"> Download file </n-button>
    </n-space>
  </n-card>

  <n-card title="File to base64">
    <n-upload v-model:file-list="fileList" :show-file-list="true" :on-before-upload="onUpload" list-type="image">
      <n-upload-dragger>
        <div style="margin-bottom: 12px">
          <n-icon size="35" :depth="3" :component="Upload" />
        </div>
        <n-text style="font-size: 14px"> Click or drag a file to this area to upload </n-text>
      </n-upload-dragger>
    </n-upload>

    <n-input :value="fileBase64" type="textarea" readonly placeholder="File in base64 will be here" />
    <n-space justify="center">
      <n-button secondary @click="copyFileBase64()"> Copy </n-button>
    </n-space>
  </n-card>
</template>

<script setup lang="ts">
import { useCopy } from '@/composable/copy';
import { useDownloadFileFromBase64 } from '@/composable/downloadBase64';
import { Upload } from '@vicons/tabler';
import { useBase64 } from '@vueuse/core';
import type { UploadFileInfo } from 'naive-ui';
import { ref, type Ref } from 'vue';

const base64Input = ref('');
const { download } = useDownloadFileFromBase64({ source: base64Input });

const fileList = ref();
const fileInput = ref() as Ref<File>;
const { base64: fileBase64 } = useBase64(fileInput);
const { copy: copyFileBase64 } = useCopy({ source: fileBase64, text: 'Base64 string copied to the clipboard' });

async function onUpload({ file: { file } }: { file: UploadFileInfo }) {
  if (file) {
    fileList.value = [];
    fileInput.value = file;
  }
}
</script>

<style lang="less" scoped>
.n-input,
.n-upload {
  margin-bottom: 15px;
}

::v-deep(.n-upload-trigger) {
  width: 100%;
}
</style>
