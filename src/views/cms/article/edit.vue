<template>

  <div class="app-container">
    <div class="block">
      <el-form :model="form" ref="form">
        <el-input ref="content" type="hidden" v-model="form.content"></el-input>
        <el-row :gutter="20">
            <el-col :span="24">
              <el-button @click="save" icon="el-icon-plus" size="mini" type="primary">{{ $t('button.submit') }}</el-button>
              <el-button @click.native="back" icon="el-icon-back" size="mini">{{ $t('button.back') }}</el-button>
            </el-col>
          <el-col :span="12">
            <el-row :gutter="20">
              <el-col :span="24">
                <el-input minlength=1 placeholder="文章标题" style="font-size: 1.2rem;margin:.2rem 0rem;" v-model="form.title"></el-input>
              </el-col>
            </el-row>
            <el-row :gutter="20">
              <el-col :span="8">
                <el-select placeholder="选择栏目" v-model="form.idChannel">
                  <el-option
                    :key="item.id"
                    :label="item.name"
                    :value="item.id"
                    v-for="item in options">
                  </el-option>
                </el-select>
              </el-col>
              <el-col :span="16">
                <el-input minlength=1 placeholder="作者" style="margin-bottom:.2rem;" v-model="form.author"></el-input>
              </el-col>

            </el-row>
          </el-col>
          <el-col :span="12">
            <el-form-item label="题图"  v-if="ifUpload">
              <el-upload
                :action="uploadUrl"
                :before-upload="handleBeforeUpload"
                :headers="uploadHeaders"
                :on-success="handleUploadSuccess"
                class="upload-demo"
                drag
              >
                <i class="el-icon-upload"></i>
                <div class="el-upload__text">上传图片</div>
              </el-upload>
            </el-form-item>
            <img :src="articleImg" style="height:8rem;"  v-if="ifUpload!==true" >
            <el-button @click.native="uploadImg" icon="el-icon-edit" v-if="ifUpload!==true">修改题图</el-button>
          </el-col>
        </el-row>
        <br>
      </el-form>
      <div :class="{fullscreen:fullscreen}" class="tinymce-container editor-container">
        <textarea :id="tinymceId" class="tinymce-textarea"/>
        <div class="editor-custom-btn-container">
          <editorImage @successCBK="imageSuccessCBK" class="editor-upload-btn" color="#1890ff"/>
        </div>
      </div>
    </div>
  </div>
</template>

<script src="./edit.js"></script>

<style scoped>
  .tinymce-container {
    position: relative;
    line-height: normal;
  }

  .tinymce-container >>> .mce-fullscreen {
    z-index: 10000;
  }

  .tinymce-textarea {
    visibility: hidden;
    z-index: -1;
  }

  .editor-custom-btn-container {
    position: absolute;
    right: 4px;
    top: 4px;
    /*z-index: 2005;*/
  }

  .fullscreen .editor-custom-btn-container {
    z-index: 10000;
    position: fixed;
  }

  .editor-upload-btn {
    display: inline-block;
  }
</style>
