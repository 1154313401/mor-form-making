<template>
    <a-modal :destroyOnClose="true" :footer="null" :visible="visible" @cancel="handleCancel" style="top:20px;" title="JSON数据" width="850px" wrapClassName="code-modal-9136076486841527">
        <previewCode :editorJson="editorJson" />
    </a-modal>
</template>
<script>
/*
 * author kcz
 * date 2019-11-20
 * description 生成json Modal
 */
import previewCode from "../../PreviewCode/index";
export default {
  name: "JsonModal",
  data() {
    return {
      visible: false,
      editorJson: "",
      jsonData: {}
    };
  },
  watch: {
    visible(val) {
      if (val) {
        // const a = { b: 1, c: { d: 2, e: { f: 3, g: { h: 4 } } } };
        // this.traverseObj(this.jsonData);
        // this.traverseObj(a);
        // JSON.stringify(a);
        this.editorJson = JSON.stringify(this.jsonData, null, "\t");
        // this.editorJson = this.jsonData.toString();
      }
    }
  },
  components: {
    previewCode
  },
  methods: {
    traverseObj(obj) {
      Object.keys(obj).forEach(objKey => {
        if (typeof obj[objKey] == "object") {
          this.traverseObj(obj[objKey]);
        }
      });
      // return JSON.stringify(obj);
    },
    handleCancel() {
      this.visible = false;
    }
  }
};
</script>
