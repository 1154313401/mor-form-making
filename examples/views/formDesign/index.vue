<template>
    <div>
        <mor-form-making @ @save="save" hideResetHint ref="MFM" />
        <!-- <mor-form-preview ref="MFV" /> -->
        <mor-form-build :value="jsonData" ref="MFB" />

        <!-- <a-button @click="getData" type="primary">导入json文件</a-button> -->
    </div>
</template>
<script>
export default {
  name: "Demo",
  data() {
    return {
      jsonData: {
        list: [
          {
            type: "group",
            label: "预扫",
            list: [
              {
                type: "checkbox",
                label: "试验频段",
                options: {
                  disabled: false,
                  hidden: false,
                  defaultValue: [],
                  dynamicKey: "",
                  dynamic: false,
                  options: [{ value: "1", label: "30MHz~1GHz" }]
                },
                model: "checkbox_1647236958306",
                key: "checkbox_1647236958306",
                help: "",
                rules: [{ required: false, message: "必填项" }]
              },
              {
                type: "textarea",
                label: "检波器类型",
                options: {
                  width: "100%",
                  minRows: 4,
                  maxRows: 6,
                  maxLength: null,
                  defaultValue: "",
                  clearable: false,
                  hidden: false,
                  disabled: false,
                  placeholder: "请输入"
                },
                model: "textarea_1647236946618",
                key: "textarea_1647236946618",
                help: "",
                rules: [{ required: false, message: "必填项" }]
              },
              {
                type: "checkbox",
                label: "扫描/扫频模式",
                options: {
                  disabled: false,
                  hidden: false,
                  defaultValue: [],
                  dynamicKey: "",
                  dynamic: false,
                  options: [
                    {
                      value: "1",
                      label: "Sweep Analyzer 6dB"
                    },
                    {
                      value: "2",
                      label: "Sweep Analyzer 3dB"
                    }
                  ]
                },
                model: "checkbox_1647236902461",
                key: "checkbox_1647236902461",
                help: "",
                rules: [{ required: false, message: "必填项" }]
              },
              {
                type: "textarea",
                label: "中频/分辨率带宽",
                options: {
                  width: "100%",
                  minRows: 4,
                  maxRows: 6,
                  maxLength: null,
                  defaultValue: "",
                  clearable: false,
                  hidden: false,
                  disabled: false,
                  placeholder: "请输入"
                },
                model: "textarea_1647237008865",
                key: "textarea_1647237008865",
                help: "",
                rules: [{ required: false, message: "必填项" }]
              },
              {
                type: "checkbox",
                label: "视频带宽",
                options: {
                  disabled: false,
                  hidden: false,
                  defaultValue: [],
                  dynamicKey: "",
                  dynamic: false,
                  options: [
                    { value: "1", label: "500kHz" },
                    { value: "2", label: "" }
                  ]
                },
                model: "checkbox_1647237117742",
                key: "checkbox_1647237117742",
                help: "",
                rules: [{ required: false, message: "必填项" }]
              },
              {
                type: "input",
                label: "输入框",
                options: {
                  type: "text",
                  width: "100%",
                  defaultValue: "",
                  placeholder: "请输入",
                  clearable: false,
                  maxLength: null,
                  addonBefore: "",
                  addonAfter: "",
                  hidden: false,
                  disabled: false
                },
                model: "input_1647237131754",
                key: "input_1647237131754",
                help: "",
                rules: [{ required: false, message: "必填项" }]
              },
              {
                type: "checkbox",
                label: "频率步进/扫描点数",
                options: {
                  disabled: false,
                  hidden: false,
                  defaultValue: [],
                  dynamicKey: "",
                  dynamic: false,
                  options: [
                    { value: "1", label: "20001" },
                    { value: "2", label: "" }
                  ]
                },
                model: "checkbox_1647237408784",
                key: "checkbox_1647237408784",
                help: "",
                rules: [{ required: false, message: "必填项" }]
              },
              {
                type: "input",
                label: "输入框",
                options: {
                  type: "text",
                  width: "100%",
                  defaultValue: "",
                  placeholder: "请输入",
                  clearable: false,
                  maxLength: null,
                  addonBefore: "",
                  addonAfter: "",
                  hidden: false,
                  disabled: false
                },
                model: "input_1647237432643",
                key: "input_1647237432643",
                help: "",
                rules: [{ required: false, message: "必填项" }]
              },
              {
                type: "checkbox",
                label: "测量/扫描时间",
                options: {
                  disabled: false,
                  hidden: false,
                  defaultValue: [],
                  dynamicKey: "",
                  dynamic: false,
                  options: [
                    { value: "1", label: "2s" },
                    { value: "2", label: "" }
                  ]
                },
                model: "checkbox_1647237430273",
                key: "checkbox_1647237430273",
                help: "",
                rules: [{ required: false, message: "必填项" }]
              },
              {
                type: "input",
                label: "输入框",
                options: {
                  type: "text",
                  width: "100%",
                  defaultValue: "",
                  placeholder: "请输入",
                  clearable: false,
                  maxLength: null,
                  addonBefore: "",
                  addonAfter: "",
                  hidden: false,
                  disabled: false
                },
                model: "input_1647237434658",
                key: "input_1647237434658",
                help: "",
                rules: [{ required: false, message: "必填项" }]
              }
            ],
            key: "group_1647236525305",
            model: "group_1647236525305"
          },
          {
            type: "group",
            label: "最大值扫描",
            list: [
              {
                type: "checkbox",
                label: "试验频段",
                options: {
                  disabled: false,
                  hidden: false,
                  defaultValue: [],
                  dynamicKey: "",
                  dynamic: false,
                  options: [{ value: "1", label: "30MHz~1GHz" }]
                },
                model: "checkbox_1647236958306",
                key: "checkbox_1647236958306",
                help: "",
                rules: [{ required: false, message: "必填项" }]
              },
              {
                type: "textarea",
                label: "检波器类型",
                options: {
                  width: "100%",
                  minRows: 4,
                  maxRows: 6,
                  maxLength: null,
                  defaultValue: "",
                  clearable: false,
                  hidden: false,
                  disabled: false,
                  placeholder: "请输入"
                },
                model: "textarea_1647236946618",
                key: "textarea_1647236946618",
                help: "",
                rules: [{ required: false, message: "必填项" }]
              },
              {
                type: "checkbox",
                label: "扫描/扫频模式",
                options: {
                  disabled: false,
                  hidden: false,
                  defaultValue: [],
                  dynamicKey: "",
                  dynamic: false,
                  options: [
                    { value: "1", label: "ScanLin" },
                    { value: "2", label: "  ScanLog" }
                  ]
                },
                model: "checkbox_1647236902461",
                key: "checkbox_1647236902461",
                help: "",
                rules: [{ required: false, message: "必填项" }]
              },
              {
                type: "textarea",
                label: "中频/分辨率带宽",
                options: {
                  width: "100%",
                  minRows: 4,
                  maxRows: 6,
                  maxLength: null,
                  defaultValue: "",
                  clearable: false,
                  hidden: false,
                  disabled: false,
                  placeholder: "请输入"
                },
                model: "textarea_1647237008865",
                key: "textarea_1647237008865",
                help: "",
                rules: [{ required: false, message: "必填项" }]
              },
              {
                type: "checkbox",
                label: "频率步进/扫描点数",
                options: {
                  disabled: false,
                  hidden: false,
                  defaultValue: [],
                  dynamicKey: "",
                  dynamic: false,
                  options: [
                    { value: "1", label: "40kHz" },
                    { value: "2", label: "" }
                  ]
                },
                model: "checkbox_1647237117742",
                key: "checkbox_1647237117742",
                help: "",
                rules: [{ required: false, message: "必填项" }]
              },
              {
                type: "input",
                label: "输入框",
                options: {
                  type: "text",
                  width: "100%",
                  defaultValue: "",
                  placeholder: "请输入",
                  clearable: false,
                  maxLength: null,
                  addonBefore: "",
                  addonAfter: "",
                  hidden: false,
                  disabled: false
                },
                model: "input_1647237131754",
                key: "input_1647237131754",
                help: "",
                rules: [{ required: false, message: "必填项" }]
              },
              {
                type: "checkbox",
                label: "测量/扫描时间",
                options: {
                  disabled: false,
                  hidden: false,
                  defaultValue: [],
                  dynamicKey: "",
                  dynamic: false,
                  options: [
                    { value: "1", label: "500ms" },
                    { value: "2", label: "" }
                  ]
                },
                model: "checkbox_1647237346327",
                key: "checkbox_1647237346327",
                help: "",
                rules: [{ required: false, message: "必填项" }]
              }
            ],
            key: "group_1647237266731",
            model: "group_16472365253051"
          },
          {
            type: "radio",
            label: "单选框",
            options: {
              disabled: false,
              hidden: false,
              defaultValue: "",
              dynamicKey: "",
              dynamic: false,
              options: [
                { value: "1", label: "选项1" },
                { value: "2", label: "选项2" },
                { value: "3", label: "选项3" }
              ]
            },
            model: "radio_1647237429564",
            key: "radio_1647237429564",
            help: "",
            rules: [{ required: false, message: "必填项" }]
          }
        ],
        config: {
          layout: "horizontal",
          labelCol: { xs: 4, sm: 4, md: 4, lg: 4, xl: 4, xxl: 4 },
          labelWidth: 100,
          labelLayout: "flex",
          wrapperCol: {
            xs: 18,
            sm: 18,
            md: 18,
            lg: 18,
            xl: 18,
            xxl: 18
          },
          hideRequiredMark: false,
          customStyle: ""
        }
      },
      dataJson: {
        radio_1647237429564: "2",
        group_1647236525305: {
          checkbox_1647236958306: ["1"],
          textarea_1647236946618: "333",
          checkbox_1647236902461: ["1"],
          textarea_1647237008865: "33",
          checkbox_1647237117742: ["2"],
          input_1647237131754: "344",
          checkbox_1647237408784: ["2"],
          input_1647237432643: "44",
          checkbox_1647237430273: ["1"],
          input_1647237434658: "44"
        },
        group_16472365253051: {
          checkbox_1647236958306: ["1"],
          textarea_1647236946618: "44",
          checkbox_1647236902461: ["1"],
          textarea_1647237008865: "33",
          checkbox_1647237117742: ["1"],
          input_1647237131754: "33",
          checkbox_1647237346327: ["1"]
        }
      }
    };
  },
  methods: {
    getData() {
      this.$refs.MFB.getAllData().then(res => {
        console.log(res);
      });

      // this.$refs.MFV.jsonData = this.jsonData;
      // this.$refs.MFV.visible = true;
      // 通过函数获取数据
      // this.$refs.MFM.importData(this.jsonData);
      // const data = this.$refs.MFM.getData();
      // console.log(data);
    },
    save() {
      this.$refs.MFB.setAllData(this.dataJson);
    }
  }
};
</script>

