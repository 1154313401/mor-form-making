<template>
    <a-config-provider :locale="locale">
        <a-form :form="form" :ref="value.model||'form'" @submit="handleSubmit" class="mor-form-build-9136076486841527" v-if="
        typeof value.list !== 'undefined' 
      ">
            <buildBlocks
                :config="config"
                :disabled="disabled"
                :dynamicData="getDynamicData"
                :key="index"
                :record="record"
                :validatorError="validatorError"
                @change="handleChange"
                @handleReset="reset"
                ref="buildBlocks"
                v-for="(record, index) in value.list"
            />
        </a-form>
    </a-config-provider>
</template>
<script>
/*
 * author kcz
 * date 2019-11-20
 * description 将json数据构建成表单
 */
import buildBlocks from "./buildBlocks";
import zhCN from "ant-design-vue/lib/locale-provider/zh_CN";
// import moment from "moment";
export default {
  name: "MorFormBuild",
  data() {
    return {
      locale: zhCN,
      form: this.$form.createForm(this),
      validatorError: {},
      defaultDynamicData: {},
      allData: {}
    };
  },
  // props: ["value", "dynamicData"],
  props: {
    value: {
      type: Object,
      required: true
    },
    dynamicData: {
      type: Object,
      default: () => {
        return {};
      }
    },
    config: {
      type: Object,
      default: () => ({})
    },
    disabled: {
      type: Boolean,
      default: false
    },
    outputString: {
      type: Boolean,
      default: false
    },
    defaultValue: {
      type: Object,
      default: () => ({})
    }
  },
  components: {
    buildBlocks
  },
  computed: {
    getDynamicData() {
      return typeof this.dynamicData === "object" &&
        Object.keys(this.dynamicData).length
        ? this.dynamicData
        : window.$kfb_dynamicData || {};
    }
  },
  created() {},
  methods: {
    //遍历对象
    traverseObj(obj, coord, key, val) {
      let isFind = false;
      Object.keys(obj).forEach(objKey => {
        // if (typeof obj[objKey] == "object") {
        if (objKey == coord) {
          // obj[objKey] = {};
          isFind = true;
          obj[objKey][key] = JSON.parse(val);
          return JSON.parse(JSON.stringify(obj));
        }
        // }
      });
      if (!isFind) {
        const keys = Object.keys(obj);
        for (let i = 0; i < keys.length; i++) {
          if (typeof obj[keys[i]] == "object") {
            this.traverseObj(obj[keys[i]], coord, key, val);
          }
        }
        // Object.keys(obj).forEach(objKey => {
        //   if (typeof obj[objKey] == "object") {
        //     return this.traverseObj(obj[objKey], coord, key, val);
        //   }
        // });
      }
      // return JSON.parse(JSON.stringify(obj));
      // return JSON.stringify(obj);
    },
    // moment,
    handleSubmit(e) {
      // 提交按钮触发，并触发submit函数，返回getData函数
      e.preventDefault();
      this.$emit("submit", this.getData);
    },
    reset() {
      // 重置表单
      this.form.resetFields();
    },
    // getAllData() {
    getAllData(isForm = true, vueComponent, obj, data, coord) {
      let witch, newObj, name;
      if (isForm) {
        witch = this.$refs.form;
        newObj = this.value;
      } else {
        if (vueComponent.$refs[obj.model]) {
          name = obj.model;
          witch = vueComponent.$refs[name];
        } else {
          if (vueComponent.$children && vueComponent.$children.length > 0) {
            vueComponent.$children.forEach(el => {
              if (typeof obj !== "object") obj = JSON.parse(obj);
              return this.getAllData(false, el, obj, data, coord);
            });
          }
          return;
        }
        newObj = obj;
      }
      let temData, temCoord;
      return this.getData(witch).then(res => {
        if (isForm) {
          temData = res;
          // temData = JSON.stringify(temData);
          temCoord = "first";
        } else {
          if (typeof data !== "object") temData = JSON.parse(data);
          else temData = data;

          if (coord == "first") {
            temData[name] = res;
          } else {
            temData = this.traverseObj(
              temData,
              coord,
              name,
              JSON.stringify(res)
            );
            // temData = JSON.parse(this.traverseObj(temData, coord, name, res));
          }
          temCoord = name;
        }
        if (newObj.list && newObj.list.length > 0) {
          newObj.list.forEach(el => {
            if (el.model && el.type && el.type == "group") {
              if (typeof temData !== "object") temData = JSON.parse(temData);
              return this.getAllData(false, witch, el, temData, temCoord);
            }
          });
        }
        return temData;
      });
    },
    // traverse(true).then(res => {
    // });
    // // return data;
    // },
    getData(witch) {
      // 提交函数，提供父级组件调用
      return new Promise((resolve, reject) => {
        try {
          witch.form.validateFields((err, values) => {
            if (err) {
              reject(err);
              /**
               * @author: lizhichao<meteoroc@outlook.com>
               * @Description: 多容器校验时，提供error返回给多容器进行判断。
               */
              this.validatorError = err;
              return;
            }
            this.validatorError = {};
            this.$refs.buildBlocks.forEach(item => {
              if (!item.validationSubform()) {
                reject(err);
              }
            });
            if (this.outputString) {
              // 需要所有value转成字符串
              for (const key in values) {
                const type = typeof values[key];
                if (type === "string" || type === "undefined") {
                  continue;
                } else if (type === "object") {
                  values[key] = `mor-form-making#${type}#${JSON.stringify(
                    values[key]
                  )}`;
                } else {
                  values[key] = `mor-form-making#${type}#${String(
                    values[key]
                  )}`;
                }
              }
              resolve(values);
            } else {
              resolve(values);
            }
          });
        } catch (err) {
          console.error(err);
          reject(err);
        }
      });
    },
    // setAllData() {
    setAllData(dataJson) {
      // const json = JSON.parse(dataJson);
      // const templateJson = JSON.parse(templateJson);
      // const dataJson = {
      //   radio_1647237429564: "2",
      //   group_1647236525305: {
      //     checkbox_1647236958306: ["1"],
      //     textarea_1647236946618: "333",
      //     checkbox_1647236902461: ["1"],
      //     textarea_1647237008865: "33",
      //     checkbox_1647237117742: ["2"],
      //     input_1647237131754: "344",
      //     checkbox_1647237408784: ["2"],
      //     input_1647237432643: "44",
      //     checkbox_1647237430273: ["1"],
      //     input_1647237434658: "44"
      //   },
      //   group_16472365253051: {
      //     checkbox_1647236958306: ["1"],
      //     textarea_1647236946618: "44",
      //     checkbox_1647236902461: ["1"],
      //     textarea_1647237008865: "33",
      //     checkbox_1647237117742: ["1"],
      //     input_1647237131754: "33",
      //     checkbox_1647237346327: ["1"]
      //   }
      // }
      const newJson = {};
      Object.keys(dataJson).forEach(el => {
        if (typeof dataJson[el] != "object") newJson[el] = dataJson[el];
      });
      const vueComponent = this.$refs.form;
      this.setData(vueComponent, newJson);
      Object.keys(dataJson).forEach(el => {
        this.getRef(vueComponent, el, dataJson[el]);
      });
    },
    getRef(vueComponent, witch, json) {
      if (vueComponent.$refs[witch]) {
        this.setData(vueComponent.$refs[witch], json);
      } else {
        if (vueComponent.$children && vueComponent.$children.length > 0) {
          vueComponent.$children.forEach(el => {
            this.getRef(el, witch, json);
          });
        }
        return;
      }
    },
    setData(witch, json) {
      return new Promise((resolve, reject) => {
        try {
          if (this.outputString) {
            // 将非string数据还原
            for (const key in json) {
              if (!json[key].startsWith("mor-form-making#")) {
                continue;
              }
              const array = json[key].split("#");
              if (array[1] === "object") {
                json[key] = JSON.parse(array[2]);
              } else if (array[1] === "number") {
                json[key] = Number(array[2]);
              } else if (array[1] === "boolean") {
                json[key] = Boolean(array[2]);
              }
            }
            witch && witch.form && witch.form.setFieldsValue(json);
          } else {
            witch && witch.form && witch.form.setFieldsValue(json);
          }
          resolve(true);
        } catch (err) {
          console.error(err);
          reject(err);
        }
      });
    },

    // 批量设置某个option的值
    setOptions(fields, optionName, value) {
      fields = new Set(fields);

      // 递归遍历控件树
      const traverse = array => {
        array.forEach(element => {
          if (fields.has(element.model)) {
            this.$set(element.options, optionName, value);
          }
          if (element.type === "grid" || element.type === "tabs") {
            // 栅格布局 and 标签页
            element.columns.forEach(item => {
              traverse(item.list);
            });
          } else if (element.type === "card" || element.type === "batch") {
            // 卡片布局 and  动态表格
            traverse(element.list);
          } else if (element.type === "table") {
            // 表格布局
            element.trs.forEach(item => {
              item.tds.forEach(val => {
                traverse(val.list);
              });
            });
          }
        });
      };
      traverse(this.value.list);
    },
    // 隐藏表单字段
    hide(fields) {
      this.setOptions(fields, "hidden", true);
    },
    // 显示表单字段
    show(fields) {
      this.setOptions(fields, "hidden", false);
    },
    // 禁用表单字段
    disable(fields) {
      this.setOptions(fields, "disabled", true);
    },
    // 启用表单字段
    enable(fields) {
      this.setOptions(fields, "disabled", false);
    },
    handleChange(value, key) {
      // 触发change事件
      this.$emit("change", value, key);
    }
  },

  mounted() {
    this.$nextTick(() => {
      this.setData(this.defaultValue);
    });
  }
};
</script>
