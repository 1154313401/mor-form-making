<!--
 * @Description: 折叠组件
 * @Author: kcz
 * @Date: 2020-01-13 00:37:54
 * @LastEditors: kcz
 * @LastEditTime: 2020-03-28 11:32:39
 -->
<template>
    <draggable
        :value="list"
        @start="handleStart($event, list)"
        tag="ul"
        v-bind="{
      group: { name: 'form-draggable', pull: 'clone', put: false },
      sort: false,
      animation: 180,
      ghostClass: 'moving'
    }"
    >
        <li :key="index" @click="handleListPush(val)" @dragstart="$emit('generateKey', list, index)" v-for="(val, index) in list">
            <!-- <svg v-if="val.icon" class="icon" aria-hidden="true">
        <use :xlink:href="`#${val.icon}`"></use>
            </svg>-->
            <div>
                <img :src="addImg(val.type)" alt />
            </div>
            <!-- <img :src="img" alt /> -->
            {{ val.label }}
        </li>
    </draggable>
</template>
<script>
import draggable from "vuedraggable";
export default {
  name: "collapseItem",
  props: ["list"],
  data() {
    return {
      img: require("../../../static/img/checkbox.png")
    };
  },
  components: {
    draggable
  },
  methods: {
    addImg(witch) {
      return require(`../../../static/img/${witch}.png`);
    },
    handleStart(e, list) {
      this.$emit("start", list[e.oldIndex].type);
    },
    handleListPush(val) {
      val.key = "";
      this.$emit("handleListPush", val);
    }
  },
  mounted() {}
};
</script>
