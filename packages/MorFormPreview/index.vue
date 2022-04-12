<template>
    <a-modal
        :bodyStyle="bodyStyle"
        :centered="true"
        :destroyOnClose="true"
        :dialogStyle="dialogStyle"
        :visible="visible"
        :width="`${previewWidth}px`"
        @cancel="handleCancel"
        @ok="handleGetData"
        :zIndex="9999"
        cancelText="关闭"
        style="top:20px;"
        title="预览"
    >
        <mor-form-build :value="jsonData" @submit="handleSubmit" ref="MorFormBuild" />
        <jsonModel ref="jsonModel" />
    </a-modal>
</template>
<script>
/*
 * author kcz
 * date 2019-11-20
 */
import jsonModel from "../MorFormMaking/module/jsonModal";
export default {
  name: "MorFormPreview",
  data() {
    return {
      visible: false,
      previewWidth: 850,
      jsonData: {},
      dialogStyle: {
        position: "absolute",
        right: "150px",
        left: "150px",
        minWidth: "700px",
        width: "auto",
        top: "56px",
        bottom: "30px",
        minHeight: "500px"
      },
      bodyStyle: {
        height: "calc(100% - 108px)",
        overflow: "auto"
      }
    };
  },
  components: {
    jsonModel
  },
  methods: {
    handleSubmit(p) {
      p.then(res => {
        this.$refs.jsonModel.jsonData = res;
        this.$refs.jsonModel.visible = true;
      }).catch(err => {
        console.log(err);
      });
    },
    handleGetData() {
      // this.$refs.MorFormBuild.getAllData()
      // const data =
      this.$refs.MorFormBuild.getAllData(true)
        .then(res => {
          console.log(res);
        })
        .catch(err => {
          console.log(err);
        });
      this.visible = false;
    },
    handleCancel() {
      this.visible = false;
    }
  }
};
</script>
<style lang="less" scoped>
.ant-modal-root::v-deep .ant-modal-body::-webkit-scrollbar {
    width: 6px;
    height: 6px;
}
.ant-modal-root::v-deep .ant-btn-primary:hover,
.ant-btn-primary:focus {
    background-color: #6083f7;
    border-color: #6083f7;
    color: #fff;
}
.ant-modal-root::v-deep .ant-btn-primary {
    background-color: #7a99fc;
    border-color: #7a99fc;
    color: #fff;
}
.ant-modal-root::v-deep .ant-modal-body::-webkit-scrollbar-thumb {
    border-radius: 5px;
    -webkit-box-shadow: inset 0 0 5px rgb(0 0 0 / 20%);
    box-shadow: inset 0 0 5px rgb(0 0 0 / 20%);
    background: rgba(0, 0, 0, 0.2);
    scrollbar-arrow-color: red;
}
.ant-modal-root::v-deep .ant-modal-body::-webkit-scrollbar-track {
    -webkit-box-shadow: inset 0 0 5px rgb(0 0 0 / 20%);
    box-shadow: inset 0 0 5px rgb(0 0 0 / 20%);
    border-radius: 0;
    background: rgba(0, 0, 0, 0.1);
}
.ant-modal-root::v-deep .ant-modal-content {
    height: 100%;
}
</style>