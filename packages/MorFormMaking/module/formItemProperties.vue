<template>
    <div class="properties-centent kk-checkbox">
        <div class="properties-body">
            <a-empty class="hint-box" description="未选择组件" style="color: #aaa;" v-show="selectItem.key === ''" />

            <a-form v-show="selectItem.key !== ''">
                <a-form-item label="标签" v-if="typeof selectItem.label !== 'undefined'">
                    <a-input placeholder="请输入" v-model="selectItem.label" />
                </a-form-item>

                <a-form-item label="数据字段" v-if="!hideModel && typeof selectItem.model !== 'undefined'">
                    <a-input placeholder="请输入" v-model="selectItem.model" />
                </a-form-item>
                <!-- 选项配置及动态数据配置 start -->
                <a-form-item label="选项配置" v-if="typeof options.options !== 'undefined'">

                    <KChangeOption v-model="options.options" v-show="!options.dynamic" />
                </a-form-item>

                <a-form-item label="列配置项" v-if="selectItem.type === 'grid'">
                    <KChangeOption type="colspan" v-model="selectItem.columns" />
                </a-form-item>
            </a-form>
        </div>
    </div>
</template>
<script>
/*
 * author kcz
 * date 2019-11-20
 * description 表单控件属性设置组件,因为配置数据是引用关系，所以可以直接修改
 */
import KChangeOption from "../../KChangeOption/index.vue";
export default {
  name: "formItemProperties",
  data() {
    return {
      familyOptions: [
        // 字体选择设置
        {
          value: "SimSun",
          label: "宋体"
        },
        {
          value: "FangSong",
          label: "仿宋"
        },
        {
          value: "SimHei",
          label: "黑体"
        },
        {
          value: "PingFangSC-Regular",
          label: "苹方"
        },
        {
          value: "KaiTi",
          label: "楷体"
        },
        {
          value: "LiSu",
          label: "隶书"
        }
      ],
      sizeOptions: [
        //字号选择设置
        {
          value: "26pt",
          label: "一号"
        },
        {
          value: "24pt",
          label: "小一"
        },
        {
          value: "22pt",
          label: "二号"
        },
        {
          value: "18pt",
          label: "小二"
        },
        {
          value: "16pt",
          label: "三号"
        },
        {
          value: "15pt",
          label: "小三"
        },
        {
          value: "14pt",
          label: "四号"
        },
        {
          value: "12pt",
          label: "小四"
        },
        {
          value: "10.5pt",
          label: "五号"
        },
        {
          value: "9pt",
          label: "小五"
        }
      ]
    };
  },
  computed: {
    options() {
      return this.selectItem.options || {};
    }
  },
  props: {
    selectItem: {
      type: Object,
      required: true
    },
    hideModel: {
      type: Boolean,
      default: false
    }
  },
  components: {
    KChangeOption
  }
};
</script>
