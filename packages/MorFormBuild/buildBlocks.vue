<template>
    <!-- 组 -->
    <a-card :title="record.label" class="grid-row" v-if="record.type === 'group'" style="width: 100%;">
        <!-- <buildBlocks
            :config="config"
            :disabled="disabled"
            :dynamicData="dynamicData"
            :formConfig="formConfig"
            :key="item.key"
            :record="item"
            @change="handleChange"
            @handleReset="$emit('handleReset')"
            ref="nestedComponents"
            v-for="item in record.list"
        />-->
        <mor-form-build :value="record" ref="MorFormBuild" />
    </a-card>
    <!-- 卡片布局 -->
    <!-- <a-card
    v-else-if="record.type === 'card'"
    class="grid-row"
    :title="record.label"
  >
    <buildBlocks
      ref="nestedComponents"
      @handleReset="$emit('handleReset')"
      @change="handleChange"
      v-for="item in record.list"
      :disabled="disabled"
      :dynamicData="dynamicData"
      :key="item.key"
      :record="item"
      :formConfig="formConfig"
      :config="config"
    />
    </a-card>-->

    <KFormItem
        :config="config"
        style="width: 49%;margin-right: 1%;"
        :disabled="disabled"
        :dynamicData="dynamicData"
        :key="record.key"
        :record="record"
        @change="handleChange"
        @handleReset="$emit('handleReset')"
        ref="nestedComponents"
        v-else-if="!record.options.hidden"
    />
</template>
<script>
/*
 * author kcz
 * date 2019-11-20
 */
import KFormItem from "../KFormItem/index";
export default {
  name: "buildBlocks",
  props: {
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
      required: true
    },
    disabled: {
      type: Boolean,
      default: false
    },
    validatorError: {
      type: [Object, null],
      default: () => ({})
    }
  },
  components: {
    KFormItem
  },
  data() {
    return {
      activeKey: 0
    };
  },
  methods: {
    validationSubform() {
      // 验证动态表格
      const nestedComponents = this.$refs.nestedComponents;
      if (
        typeof nestedComponents === "object" &&
        nestedComponents instanceof Array
      ) {
        for (let i = 0; nestedComponents.length > i; i++) {
          if (!nestedComponents[i].validationSubform()) {
            return false;
          }
        }
        return true;
      } else if (typeof nestedComponents !== "undefined") {
        return nestedComponents.validationSubform();
      } else {
        return true;
      }
    },
    handleChange(value, key) {
      this.$emit("change", value, key);
    }
  },
  watch: {
    /**
     * @author: lizhichao<meteoroc@outlook.com>
     * @description: 监视validatorError对象，当检测到Tabs中有表单校验无法通过时，切换到最近校验失败的tab页。
     */
    validatorError: {
      deep: true,
      handler: function(n) {
        const errorItems = Object.keys(n);
        if (errorItems.length) {
          if (!this.record.columns) return false;
          for (let i = 0; i < this.record.columns.length; i++) {
            const err = this.record.columns[i].list.filter(item =>
              errorItems.includes(item.model)
            );
            if (err.length) {
              this.activeKey = i;
              break;
            }
          }
        }
      }
    }
  }
};
</script>
