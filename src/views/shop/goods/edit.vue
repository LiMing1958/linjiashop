<template>
  <div class="app-container">
    <div class="block">
      <el-row>
        <el-col :span="24">
          <el-button @click="save" size="mini" type="primary">{{active==3?"提交":"下一步"}}</el-button>
          <el-button @click="prev" size="mini" v-show="active>0">上一步</el-button>
        </el-col>
      </el-row>
    </div>
    <br>
    <div class="block">
      <el-steps :active="active" finish-status="success">
        <el-step title="基本信息"></el-step>
        <el-step title="商品相册"></el-step>
        <el-step title="商品详情"></el-step>
        <el-step title="上架信息"></el-step>
      </el-steps>
    </div>


    <el-form  :model="form"   label-width="150px" v-show="active==0">
      <el-row>
        <el-col :span="24">
          <el-form-item label="名称">
            <el-input minlength=1 v-model="form.name"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="24">
          <el-form-item label="产品简介">
            <el-input minlength=1 v-model="form.descript"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="24">
          <el-form-item label="类别">
            <el-select filterable placeholder="请选择" v-model="form.idCategory">
              <el-option
                :key="item.id"
                :label="item.name"
                :value="item.id"
                v-for="item in categories"

              >
              </el-option>
            </el-select>

          </el-form-item>
        </el-col>
        <el-col :span="24">
          <el-form-item label="是否新品">
            <el-radio-group v-model="form.isNew">
              <el-radio :label="true">是</el-radio>
              <el-radio :label="false">否</el-radio>
            </el-radio-group>
          </el-form-item>
        </el-col>
        <el-col :span="24">
          <el-form-item label="是否热卖">
            <el-radio-group v-model="form.isHot">
              <el-radio :label="true">是</el-radio>
              <el-radio :label="false">否</el-radio>
            </el-radio-group>
          </el-form-item>
        </el-col>


      </el-row>
    </el-form>

    <el-form  label-width="150px" v-show="active==1">
      <el-row>

        <el-col :span="24">
          <el-form-item label="商品缩略图">
            <el-upload
              :action="uploadUrl"
              :headers="uploadHeaders"
              :on-success="handleUploadPicSuccess"
              :show-file-list="false"
              accept=".jpg,.jpeg,.png,.gif"
              class="avatar-uploader">
              <img :src="apiUrl+ '/file/getImgStream?idFile=' +form.pic" class="avatar" v-if="form.pic">
              <i class="el-icon-plus avatar-uploader-icon" v-else/>
            </el-upload>

          </el-form-item>
        </el-col>
        <el-col :span="24">
          <el-form-item label="商品相册">
              <el-upload
                :action="uploadUrl"
                :file-list="galleryList"
                :headers="uploadHeaders"
                :on-remove="handleRemove"
                :on-success="handleUploadGallerySuccess"
                list-type="picture-card">
                <i class="el-icon-plus"></i>
              </el-upload>

          </el-form-item>
        </el-col>


      </el-row>
    </el-form>

    <div :class="{fullscreen:fullscreen}" class="tinymce-container editor-container" v-show="active==2">
      <textarea class="tinymce-textarea" id="tinymceId"/>
      <div class="editor-custom-btn-container">
        <editorImage @successCBK="imageSuccessCBK" class="editor-upload-btn" color="#1890ff"/>
      </div>
    </div>
    <el-form  label-width="150px" v-show="active==3">
      <el-row>



        <el-col :span="24">
          <el-form-item label="商品规格">
            <el-radio class="radio" label="one" v-model="spec">单规格</el-radio>
            <el-radio class="radio" label="more" v-model="spec" v-show="attrKeyList.length>0">多规格</el-radio>
          </el-form-item>
        </el-col>
        <el-col :span="24" v-if="spec == 'one'">
          <el-form-item label="库存">
            <el-input-number :max="100000" :min="0" v-model="form.stock"></el-input-number>
          </el-form-item>
          <el-form-item label="市场价(分)">
            <el-input-number :max="10000000" :min="0" v-model="form.marketingPrice"></el-input-number>
          </el-form-item>
          <el-form-item label="价格(分)">
            <el-input-number :max="10000000" :min="0" v-model="form.price"></el-input-number>
          </el-form-item>
        </el-col>
        <el-col  :offset="3" :span="18" style="overflow: auto; text-align: center;" v-else>
          <el-button @click="openAddSkuForm" style="margin-bottom: 10px;" type="primary">添加规格</el-button>
          <el-table
            :data="skuList"
            :row-class-name="tableRowClassName"
            style="margin-bottom: 20px;">
            <el-table-column
              label="规格"
              prop="codeName">
            </el-table-column>
            <el-table-column
              label="市场价"
              prop="marketingPrice">
            </el-table-column>

            <el-table-column
              label="价格(分)"
              prop="price">
            </el-table-column>

            <el-table-column
              label="库存"
              prop="stock">
            </el-table-column>

            <el-table-column label="操作">
              <template slot-scope="scope">
                <el-button
                  @click="removeSku(scope.$index)"
                  size="small"
                  type="danger">删除
                </el-button>
              </template>
            </el-table-column>

          </el-table>
        </el-col>
      </el-row>
    </el-form>
    <el-dialog :visible.sync="specDialogFormVisible" title="SKU配置">
      <el-form :model="skuForm">

            <el-form-item label="已选规格" label-width="100px" prop="specs">
            <el-tag
              :key="tag.id"
              :type="tag.id"
              @close="removeTag(tag)"
              closable
              v-for="tag in tags">
              {{tag.attrVal}}
            </el-tag>
            </el-form-item>

        <el-row>
          <el-col :span="8">
            <el-form-item label="属性名" label-width="100px"  >
              <el-select
                @change="changeAttrKey"
                allow-create
                default-first-option
                filterable
                placeholder="属性名"
                v-model="attrKeySel">
                <el-option
                  :key="item.id"
                  :label="item.attrName"
                  :value="item.id"
                  v-for="item in attrKeyList">
                </el-option>
              </el-select>

            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="属性值" label-width="100px"  >
              <el-select
                allow-create
                default-first-option
                filterable
                placeholder="属性值"
                v-model="attrValSel">
                <el-option
                  :key="item.id"
                  :label="item.attrVal"
                  :value="item.id"
                  v-for="item in attrValListSel">
                </el-option>
              </el-select>

            </el-form-item>
          </el-col>
          <el-col :offset="2" :span="6">
            <el-button @click="addToTag" type="success">添加规格</el-button>
          </el-col>
        </el-row>

        <el-form-item label="市场价(分)" label-width="100px">
          <el-input-number :max="100000" :min="0" v-model="skuForm.marketingPrice"></el-input-number>
        </el-form-item>
        <el-form-item label="价格(分)" label-width="100px">
          <el-input-number :max="1000000" :min="0" v-model="skuForm.price"></el-input-number>
        </el-form-item>
        <el-form-item label="库存" label-width="100px">
          <el-input-number :max="100000" :min="0" v-model="skuForm.stock"></el-input-number>
        </el-form-item>
      </el-form>
      <div class="dialog-footer" slot="footer">
        <el-button @click="closeAddSkuForm" type="default">取 消</el-button>
        <el-button @click="addSku" type="primary">确 定</el-button>
      </div>
    </el-dialog>
    <br>
    <div class="block">
      <el-row>
        <el-col :span="24">
          <el-button @click="save" size="mini" type="primary">{{active==3?"提交":"下一步"}}</el-button>
          <el-button @click="prev" size="mini" v-show="active>0">上一步</el-button>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script src="./goodsEdit.js"></script>


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

  .avatar-uploader .el-upload {
    border: 1px dashed #d9d9d9;
    border-radius: 6px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
  }
  .avatar-uploader .el-upload:hover {
    border-color: #20a0ff;
  }
  .avatar-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 120px;
    height: 120px;
    line-height: 120px;
    text-align: center;
  }
  .avatar {
    width: 145px;
    height: 145px;
    display: block;
  }

  .add_attr_key_row {
    height: 0;
    overflow: hidden;
    transition: all 400ms;
  }

  .showEdit {
    height: 185px;
  }

  .add_attr_key_button {
    text-align: center;
    line-height: 40px;
    transition: all 400ms;

  }
</style>

