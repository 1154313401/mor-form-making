<!--
 * @Description: 传入record数据，通过判断record.type，来渲染对应的组件
 * @Author: kcz
 * @Date: 2020-01-02 22:41:48
 * @LastEditors: kcz
 * @LastEditTime: 2021-05-28 00:59:02
 -->
<template>
  <a-form-item
    v-if="
      [
        'input',
        'textarea',
        'date',
        'time',
        'number',
        'radio',
        'checkbox',
        'select',
        'rate',
        'switch',
        'slider',
        'uploadImg',
        'uploadFile',
        'cascader',
        'treeSelect'
      ].includes(record.type)
    "
  >
    <span slot="label">
      <a-tooltip>
        <span v-text="record.label"></span>
        <span v-if="record.help" slot="title" v-html="record.help"></span>
        <a-icon
          v-if="record.help"
          class="question-circle"
          type="question-circle-o"
        />
      </a-tooltip>
    </span>
    <!-- 多行文本 -->
    <a-textarea
      :style="`width:${record.options.width}`"
      v-if="record.type === 'textarea'"
      :autoSize="{
        minRows: record.options.minRows,
        maxRows: record.options.maxRows
      }"
      :disabled="disabled || record.options.disabled"
      :placeholder="record.options.placeholder"
      :allowClear="record.options.clearable"
      :maxLength="record.options.maxLength"
      :rows="4"
      @change="handleChange($event.target.value, record.model)"
      v-decorator="[
        record.model, // input 的 name
        {
          initialValue: record.options.defaultValue, // 默认值
          rules: record.rules // 验证规则
        }
      ]"
    />
    <!-- 单选框 -->
    <a-radio-group
      v-else-if="record.type === 'radio'"
      :options="
        !record.options.dynamic
          ? record.options.options
          : dynamicData[record.options.dynamicKey]
          ? dynamicData[record.options.dynamicKey]
          : []
      "
      :disabled="disabled || record.options.disabled"
      :placeholder="record.options.placeholder"
      @change="handleChange($event.target.value, record.model)"
      v-decorator="[
        record.model,
        {
          initialValue: record.options.defaultValue,
          rules: record.rules
        }
      ]"
    />
    <!-- 多选框 -->
    <a-checkbox-group
      v-else-if="record.type === 'checkbox'"
      :options="
        !record.options.dynamic
          ? record.options.options
          : dynamicData[record.options.dynamicKey]
          ? dynamicData[record.options.dynamicKey]
          : []
      "
      :disabled="disabled || record.options.disabled"
      :placeholder="record.options.placeholder"
      @change="handleChange($event, record.model)"
      v-decorator="[
        record.model,
        {
          initialValue: record.options.defaultValue,
          rules: record.rules
        }
      ]"
    />
    <!-- 开关 -->
    <a-switch
      v-else-if="record.type === 'switch'"
      :disabled="disabled || record.options.disabled"
      @change="handleChange($event, record.model)"
      v-decorator="[
        record.model,
        {
          initialValue: record.options.defaultValue,
          valuePropName: 'checked',
          rules: record.rules
        }
      ]"
    />
    <!-- 滑块 -->
    <div
      v-else-if="record.type === 'slider'"
      :style="`width:${record.options.width}`"
      class="slider-box"
    >
      <div class="slider">
        <a-slider
          :disabled="disabled || record.options.disabled"
          :min="record.options.min"
          :max="record.options.max"
          :step="record.options.step"
          @change="handleChange($event, record.model)"
          v-decorator="[
            record.model,
            {
              initialValue: record.options.defaultValue,
              rules: record.rules
            }
          ]"
        />
      </div>
      <div class="number" v-if="record.options.showInput">
        <a-input-number
          style="width: 100%"
          :disabled="disabled || record.options.disabled"
          :min="record.options.min"
          :max="record.options.max"
          :step="record.options.step"
          @change="handleChange($event, record.model)"
          v-decorator="[
            record.model,
            {
              initialValue: record.options.defaultValue,
              rules: [
                {
                  validator: (rule, value, callback) => {
                    if (
                      record.options.step &&
                      value % record.options.step !== 0
                    ) {
                      callback('输入值必须是步长的倍数');
                    }
                    callback();
                  }
                }
              ]
            }
          ]"
        />
      </div>
    </div>
    <component
      v-else
      :style="`width:${record.options.width}`"
      v-bind="componentOption"
      :min="
        record.options.min || record.options.min === 0
          ? record.options.min
          : -Infinity
      "
      :max="
        record.options.max || record.options.max === 0
          ? record.options.max
          : Infinity
      "
      :precision="
        record.options.precision > 50 ||
        (!record.options.precision && record.options.precision !== 0)
          ? null
          : record.options.precision
      "
      :parentDisabled="disabled || record.options.disabled"
      :disabled="disabled || record.options.disabled"
      :record="record"
      :config="config"
      :filterOption="
        record.options.showSearch
          ? (inputValue, option) => {
              return (
                option.componentOptions.children[0].text
                  .toLowerCase()
                  .indexOf(inputValue.toLowerCase()) >= 0
              );
            }
          : false
      "
      :allowClear="record.options.clearable"
      :dynamicData="dynamicData"
      :treeData="
        !record.options.dynamic
          ? record.options.options
          : dynamicData[record.options.dynamicKey]
          ? dynamicData[record.options.dynamicKey]
          : []
      "
      :options="
        !record.options.dynamic
          ? record.options.options
          : dynamicData[record.options.dynamicKey]
          ? dynamicData[record.options.dynamicKey]
          : []
      "
      :mode="record.options.multiple ? 'multiple' : ''"
      @change="handleChange($event, record.model)"
      v-decorator="[
        record.model, // input 的 name
        {
          initialValue: record.options.defaultValue, // 默认值
          rules: record.rules // 验证规则
        }
      ]"
      :is="componentItem"
    ></component>
  </a-form-item>

  <div v-else>
    <!-- 分割线 -->
    <a-divider
      v-if="
        record.type === 'divider' &&
          record.label !== '' &&
          record.options.orientation
      "
      :orientation="record.options.orientation"
      >{{ record.label }}</a-divider
    >
    <a-divider v-else-if="record.type === 'divider' && record.label !== ''">{{
      record.label
    }}</a-divider>
    <a-divider v-else-if="record.type === 'divider' && record.label === ''" />
  </div>
</template>
<script>
/*
 * author kcz
 * date 2019-11-20
 */
// import moment from "moment";
import customComponent from "./customComponent";
import ComponentArray from "../core/components_use";
const _ = require("lodash/object");

export default {
  name: "KFormItem",
  props: {
    // 表单数组
    record: {
      type: Object,
      required: true
    },
    config: {
      type: Object,
      default: () => ({})
    },
    dynamicData: {
      type: Object,
      default: () => ({})
    },
    disabled: {
      type: Boolean,
      default: false
    }
  },
  components: {
    customComponent
  },
  computed: {
    customList() {
      if (window.$customComponentList) {
        return window.$customComponentList.map(item => item.type);
      } else {
        return [];
      }
    },
    /**
     * @description: 输出对应组件
     * @param {*}
     * @return {*} component
     */

    componentItem() {
      return ComponentArray[this.record.type];
    },
    componentOption() {
      return _.omit(this.record.options, ["defaultValue", "disabled"]);
    }
  },
  methods: {
    validationSubform() {
      // 验证动态表格
      if (!this.$refs.KBatch) return true;
      return this.$refs.KBatch.validationSubform();
    },
    handleChange(e, key) {
      let value = e;
      if (e && e.target) {
        value = e.target.value;
      }
      // 传递change事件
      this.$emit("change", value, key);
    }
  }
};
</script>
<style lang="less" scoped>
.slider-box {
  display: flex;
  > .slider {
    flex: 1;
    margin-right: 16px;
  }
  > .number {
    width: 70px;
  }
}
.anticon.anticon-question-circle-o {
  margin-left: 5px;
}
</style>
