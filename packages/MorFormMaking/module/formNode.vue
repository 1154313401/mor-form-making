<!--
 * @Description: 将数据通过k-form-item组件解析，生成控件
 * @Author: kcz
 * @Date: 2019-12-30 00:37:05
 * @LastEditTime: 2021-05-27 15:19:02
 * @LastEditors: kcz
 * @FilePath: \mor-form-design\packages\MorFormMaking\module\formNode.vue
 -->
<template>
    <div :class="{ active: record.key === selectItem.key }" @click.stop="$emit('handleSelectItem', record)" class="drag-move-box">
        <div class="form-item-box">
            <kFormItem :formConfig="config" :record="record" />
        </div>
        <div class="show-key-box" v-if="!hideModel" v-text="record.label + (record.model ? '/' + record.model : '')" />
        <div :class="record.key === selectItem.key ? 'active' : 'unactivated'" @click.stop="$emit('handleCopy')" class="copy">
            <img alt class=".img" src="../../../static/img/copy.png" width="20px" />
        </div>
        <div :class="record.key === selectItem.key ? 'active' : 'unactivated'" @click.stop="$emit('handleDelete')" class="delete">
            <img alt class=".img" src="../../../static/img/empty.png" width="20px" />
        </div>
    </div>
</template>
<script>
/*
 * author kcz
 * date 2019-11-20
 * description 通过json生成的单个表单节点
 */
import kFormItem from "../../KFormItem/index";
export default {
  props: {
    record: {
      type: Object,
      required: true
    },
    selectItem: {
      type: Object,
      default: () => {}
    },
    config: {
      type: Object,
      required: true
    },
    hideModel: {
      type: Boolean,
      default: false
    }
  },
  components: {
    kFormItem
  }
};
</script>
