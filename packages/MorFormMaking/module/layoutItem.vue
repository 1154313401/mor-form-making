<template>
    <div :class="{
      'layout-width': ['grid', 'table', 'card', 'divider', 'html'].includes(
        record.type
      )
    }">
        <!-- 动态表格设计模块 start -->
        <!-- <template v-if="record.type === 'batch'">
            <div :class="{ active: record.key === selectItem.key }" @click.stop="handleSelectItem(record)" class="batch-box">
                <a-form-item
                    :label="!record.options.showLabel ? '' : record.label"
                    :label-col="
            config.layout === 'horizontal' && record.options.showLabel
              ? config.labelLayout === 'flex'
                ? { style: `width:${config.labelWidth}px` }
                : config.labelCol
              : {}
          "
                    :style="
            config.layout === 'horizontal' &&
            config.labelLayout === 'flex' &&
            record.options.showLabel
              ? { display: 'flex' }
              : {}
          "
                    :wrapper-col="
            config.layout === 'horizontal' && record.options.showLabel
              ? config.labelLayout === 'flex'
                ? { style: 'width:auto;flex:1' }
                : config.wrapperCol
              : {}
          "
                >
                    <draggable
                        @add="$emit('handleColAdd', $event, record.list)"
                        @start="$emit('dragStart', $event, record.list)"
                        class="draggable-box"
                        tag="div"
                        v-bind="{
              group: insertAllowed ? 'form-draggable' : '',
              ghostClass: 'moving',
              animation: 180,
              handle: '.drag-move'
            }"
                        v-model="record.list"
                    >
                        <transition-group class="list-main" name="list" tag="div">
                            <formNode
                                :config="config"
                                :hideModel="hideModel"
                                :key="item.key"
                                :record="item"
                                :selectItem.sync="selectItem"
                                @handleColAdd="handleColAdd"
                                @handleCopy="$emit('handleCopy')"
                                @handleDelete="$emit('handleDelete')"
                                @handleSelectItem="handleSelectItem"
                                @handleShowRightMenu="handleShowRightMenu"
                                class="drag-move"
                                v-for="item in record.list"
                            />
                        </transition-group>
                    </draggable>
                </a-form-item>
                <div :class="record.key === selectItem.key ? 'active' : 'unactivated'" @click.stop="$emit('handleCopy')" class="copy">
                    <a-icon type="copy" />
                </div>
                <div :class="record.key === selectItem.key ? 'active' : 'unactivated'" @click.stop="$emit('handleDelete')" class="delete">
                    <a-icon type="delete" />
                </div>
            </div>
        </template>-->
        <!-- 动态表格设计模块 end -->
        <!-- 选择输入列 start -->
        <!-- <template v-else-if="record.type === 'selectInputList'">
            <div :class="{ active: record.key === selectItem.key }" @click.stop="handleSelectItem(record)" class="select-input-list-box">
                <a-form-item
                    :label="!record.options.showLabel ? '' : record.label"
                    :label-col="
            config.layout === 'horizontal' && record.options.showLabel
              ? config.labelLayout === 'flex'
                ? { style: `width:${config.labelWidth}px` }
                : config.labelCol
              : {}
          "
                    :style="
            config.layout === 'horizontal' &&
            config.labelLayout === 'flex' &&
            record.options.showLabel
              ? { display: 'flex' }
              : {}
          "
                    :wrapper-col="
            config.layout === 'horizontal' && record.options.showLabel
              ? config.labelLayout === 'flex'
                ? { style: 'width:auto;flex:1' }
                : config.wrapperCol
              : {}
          "
                >
                    <div :key="index" class="column-box" v-for="(column, index) in record.columns">
                        <div class="check-box">
                            <a-checkbox disabled v-if="record.options.multiple">{{ column.label }}</a-checkbox>
                            <a-radio-group disabled name="radio" v-else>
                                <a-radio :value="column.value">{{ column.label }}</a-radio>
                            </a-radio-group>
                        </div>
                        <draggable
                            @add="$emit('handleColAdd', $event, column.list)"
                            @start="$emit('dragStart', $event, column.list)"
                            class="draggable-box"
                            tag="div"
                            v-bind="{
                group: insertAllowed ? 'form-draggable' : '',
                ghostClass: 'moving',
                animation: 180,
                handle: '.drag-move'
              }"
                            v-model="column.list"
                        >
                            <transition-group class="list-main" name="list" tag="div">
                                <formNode
                                    :config="config"
                                    :hideModel="hideModel"
                                    :key="item.key"
                                    :record="item"
                                    :selectItem.sync="selectItem"
                                    @handleCopy="$emit('handleCopy')"
                                    @handleDelete="$emit('handleDelete')"
                                    @handleSelectItem="handleSelectItem"
                                    @handleShowRightMenu="handleShowRightMenu"
                                    class="drag-move"
                                    v-for="item in column.list"
                                />
                            </transition-group>
                        </draggable>
                    </div>
                </a-form-item>
                <div :class="record.key === selectItem.key ? 'active' : 'unactivated'" @click.stop="$emit('handleCopy')" class="copy">
                    <a-icon type="copy" />
                </div>
                <div :class="record.key === selectItem.key ? 'active' : 'unactivated'" @click.stop="$emit('handleDelete')" class="delete">
                    <a-icon type="delete" />
                </div>
            </div>
        </template>-->
        <!-- 选择输入列 end -->
        <!-- 标签Tabs布局 start -->
        <!-- <template v-else-if="record.type === 'tabs'">
            <div :class="{ active: record.key === selectItem.key }" @click.stop="handleSelectItem(record)" class="grid-box">
                <a-tabs
                    :animated="record.options.animated"
                    :default-active-key="0"
                    :size="record.options.size"
                    :tabBarGutter="record.options.tabBarGutter || null"
                    :tabPosition="record.options.tabPosition"
                    :type="record.options.type"
                    class="grid-row"
                >
                    <a-tab-pane :key="index" :tab="tabItem.label" v-for="(tabItem, index) in record.columns">
                        <div class="grid-col">
                            <draggable
                                @add="$emit('handleColAdd', $event, tabItem.list)"
                                @start="$emit('dragStart', $event, tabItem.list)"
                                class="draggable-box"
                                tag="div"
                                v-bind="{
                  group: 'form-draggable',
                  ghostClass: 'moving',
                  animation: 180,
                  handle: '.drag-move'
                }"
                                v-model="tabItem.list"
                            >
                                <transition-group class="list-main" name="list" tag="div">
                                    <layoutItem
                                        :config="config"
                                        :hideModel="hideModel"
                                        :insertAllowedType="insertAllowedType"
                                        :key="item.key"
                                        :record="item"
                                        :selectItem.sync="selectItem"
                                        :startType="startType"
                                        @handleColAdd="handleColAdd"
                                        @handleCopy="$emit('handleCopy')"
                                        @handleDelete="$emit('handleDelete')"
                                        @handleSelectItem="handleSelectItem"
                                        @handleShowRightMenu="handleShowRightMenu"
                                        class="drag-move"
                                        v-for="item in tabItem.list"
                                    />
                                </transition-group>
                            </draggable>
                        </div>
                    </a-tab-pane>
                </a-tabs>

                <div :class="record.key === selectItem.key ? 'active' : 'unactivated'" @click.stop="$emit('handleCopy')" class="copy">
                    <a-icon type="copy" />
                </div>
                <div :class="record.key === selectItem.key ? 'active' : 'unactivated'" @click.stop="$emit('handleDelete')" class="delete">
                    <a-icon type="delete" />
                </div>
            </div>
        </template>-->
        <!-- 标签Tabs布局 end -->
        <!-- 栅格布局 start -->
        <!-- <template v-else-if="record.type === 'grid'">
            <div :class="{ active: record.key === selectItem.key }" @click.stop="handleSelectItem(record)" class="grid-box">
                <a-row :gutter="record.options.gutter" class="grid-row">
                    <a-col :key="idnex" :span="colItem.span || 0" class="grid-col" v-for="(colItem, idnex) in record.columns">
                        <draggable
                            @add="$emit('handleColAdd', $event, colItem.list)"
                            @start="$emit('dragStart', $event, colItem.list)"
                            class="draggable-box"
                            tag="div"
                            v-bind="{
                group: 'form-draggable',
                ghostClass: 'moving',
                animation: 180,
                handle: '.drag-move'
              }"
                            v-model="colItem.list"
                        >
                            <transition-group class="list-main" name="list" tag="div">
                                <layoutItem
                                    :config="config"
                                    :hideModel="hideModel"
                                    :insertAllowedType="insertAllowedType"
                                    :key="item.key"
                                    :record="item"
                                    :selectItem.sync="selectItem"
                                    :startType="startType"
                                    @handleColAdd="handleColAdd"
                                    @handleCopy="$emit('handleCopy')"
                                    @handleDelete="$emit('handleDelete')"
                                    @handleSelectItem="handleSelectItem"
                                    @handleShowRightMenu="handleShowRightMenu"
                                    class="drag-move"
                                    v-for="item in colItem.list"
                                />
                            </transition-group>
                        </draggable>
                    </a-col>
                </a-row>

                <div :class="record.key === selectItem.key ? 'active' : 'unactivated'" @click.stop="$emit('handleCopy')" class="copy">
                    <a-icon type="copy" />
                </div>
                <div :class="record.key === selectItem.key ? 'active' : 'unactivated'" @click.stop="$emit('handleDelete')" class="delete">
                    <a-icon type="delete" />
                </div>
            </div>
        </template>-->
        <!-- 栅格布局 end -->
        <!-- 卡片布局 start -->
        <template v-if="record.type === 'card'">
            <div :class="{ active: record.key === selectItem.key }" @click.stop="handleSelectItem(record)" class="grid-box">
                <a-card :title="record.label" class="grid-row">
                    <div class="grid-col">
                        <draggable
                            @add="$emit('handleColAdd', $event, record.list)"
                            @start="$emit('dragStart', $event, record.list)"
                            class="draggable-box"
                            tag="div"
                            v-bind="{
                group: 'form-draggable',
                ghostClass: 'moving',
                animation: 180,
                handle: '.drag-move'
              }"
                            v-model="record.list"
                        >
                            <transition-group class="list-main" name="list" tag="div">
                                <layoutItem
                                    :config="config"
                                    :hideModel="hideModel"
                                    :insertAllowedType="insertAllowedType"
                                    :key="item.key"
                                    :record="item"
                                    :selectItem.sync="selectItem"
                                    :startType="startType"
                                    @handleColAdd="handleColAdd"
                                    @handleCopy="$emit('handleCopy')"
                                    @handleDelete="$emit('handleDelete')"
                                    @handleSelectItem="handleSelectItem"
                                    @handleShowRightMenu="handleShowRightMenu"
                                    class="drag-move"
                                    v-for="item in record.list"
                                />
                            </transition-group>
                        </draggable>
                    </div>
                </a-card>

                <div :class="record.key === selectItem.key ? 'active' : 'unactivated'" @click.stop="$emit('handleCopy')" class="copy">
                    <a-icon type="copy" />
                </div>
                <div :class="record.key === selectItem.key ? 'active' : 'unactivated'" @click.stop="$emit('handleDelete')" class="delete">
                    <a-icon type="delete" />
                </div>
            </div>
        </template>
        <!-- 卡片布局 end -->
        <!-- 组 start -->
        <template v-else-if="record.type === 'group'">
            <div :class="{ active: record.key === selectItem.key }" @click.stop="handleSelectItem(record)" class="grid-box">
                <div>{{record.label}}</div>
                <div class="grid-col">
                    <draggable
                        @add="$emit('handleColAdd', $event, record.list)"
                        @start="$emit('dragStart', $event, record.list)"
                        class="draggable-box"
                        tag="div"
                        v-bind="{
                group: 'form-draggable',
                ghostClass: 'moving',
                animation: 180,
                handle: '.drag-move'
              }"
                        v-model="record.list"
                    >
                        <transition-group class="list-main min-height_50" name="list" tag="div">
                            <layoutItem
                                :config="config"
                                :hideModel="hideModel"
                                :insertAllowedType="insertAllowedType"
                                :key="item.key"
                                :record="item"
                                :selectItem.sync="selectItem"
                                :startType="startType"
                                @handleColAdd="handleColAdd"
                                @handleCopy="$emit('handleCopy')"
                                @handleDelete="$emit('handleDelete')"
                                @handleSelectItem="handleSelectItem"
                                @handleShowRightMenu="handleShowRightMenu"
                                class="drag-move"
                                v-for="item in record.list"
                            />
                        </transition-group>
                    </draggable>
                </div>

                <div :class="record.key === selectItem.key ? 'active' : 'unactivated'" @click.stop="$emit('handleCopy')" class="copy">
                    <img alt class=".img" src="../../../static/img/copy.png" width="20px" />
                </div>
                <div :class="record.key === selectItem.key ? 'active' : 'unactivated'" @click.stop="$emit('handleDelete')" class="delete">
                    <img alt class=".img" src="../../../static/img/empty.png" width="20px" />
                </div>
                <div class="show-key-box" style="text-align:right">{{record.label + (record.model ? '/' + record.model : '')}}</div>
            </div>
        </template>
        <!-- 组 end -->
        <!-- 表格布局 start -->
        <!-- <template v-else-if="record.type === 'table'">
            <div :class="{ active: record.key === selectItem.key }" @click.stop="handleSelectItem(record)" class="table-box">
                <table
                    :class="{
            bright: record.options.bright,
            small: record.options.small,
            bordered: record.options.bordered
          }"
                    :style="record.options.customStyle"
                    class="table-layout kk-table-9136076486841527"
                >
                    <tr :key="trIndex" v-for="(trItem, trIndex) in record.trs">
                        <td
                            :colspan="tdItem.colspan"
                            :key="tdIndex"
                            :rowspan="tdItem.rowspan"
                            @contextmenu.prevent="
                $emit('handleShowRightMenu', $event, record, trIndex, tdIndex)
              "
                            class="table-td"
                            v-for="(tdItem, tdIndex) in trItem.tds"
                            v-show="tdItem.colspan && tdItem.rowspan"
                        >
                            <draggable
                                @add="$emit('handleColAdd', $event, tdItem.list)"
                                @start="$emit('dragStart', $event, tdItem.list)"
                                class="draggable-box"
                                tag="div"
                                v-bind="{
                  group: 'form-draggable',
                  ghostClass: 'moving',
                  animation: 180,
                  handle: '.drag-move'
                }"
                                v-model="tdItem.list"
                            >
                                <transition-group class="list-main" name="list" tag="div">
                                    <layoutItem
                                        :config="config"
                                        :hideModel="hideModel"
                                        :insertAllowedType="insertAllowedType"
                                        :key="item.key"
                                        :record="item"
                                        :selectItem.sync="selectItem"
                                        :startType="startType"
                                        @handleColAdd="handleColAdd"
                                        @handleCopy="$emit('handleCopy')"
                                        @handleDelete="$emit('handleDelete')"
                                        @handleSelectItem="handleSelectItem"
                                        @handleShowRightMenu="handleShowRightMenu"
                                        class="drag-move"
                                        v-for="item in tdItem.list"
                                    />
                                </transition-group>
                            </draggable>
                        </td>
                    </tr>
                </table>

                <div :class="record.key === selectItem.key ? 'active' : 'unactivated'" @click.stop="$emit('handleCopy')" class="copy">
                    <a-icon type="copy" />
                </div>
                <div :class="record.key === selectItem.key ? 'active' : 'unactivated'" @click.stop="$emit('handleDelete')" class="delete">
                    <a-icon type="delete" />
                </div>
            </div>
        </template>-->
        <!-- 表格布局 end -->
        <template v-else>
            <formNode
                :config="config"
                :hideModel="hideModel"
                :key="record.key"
                :record="record"
                :selectItem.sync="selectItem"
                @handleCopy="$emit('handleCopy')"
                @handleDelete="$emit('handleDelete')"
                @handleSelectItem="handleSelectItem"
                @handleShowRightMenu="$emit('handleShowRightMenu')"
            />
        </template>
    </div>
</template>
<script>
/*
 * author kcz
 * date 2019-11-20
 * description 使用递归组件调用自己，生成布局结构及表单
 */
import draggable from "vuedraggable";
import formNode from "./formNode";
export default {
  name: "layoutItem",
  props: {
    record: {
      type: Object,
      required: true
    },
    selectItem: {
      type: Object,
      required: true
    },
    config: {
      type: Object,
      required: true
    },
    startType: {
      type: String,
      required: true
    },
    insertAllowedType: {
      type: Array,
      required: true
    },
    hideModel: {
      type: Boolean,
      default: false
    }
  },
  computed: {
    insertAllowed() {
      return this.insertAllowedType.includes(this.startType);
    }
  },
  components: {
    formNode,
    draggable
  },
  methods: {
    handleShowRightMenu(e, record, trIndex, tdIndex) {
      this.$emit("handleShowRightMenu", e, record, trIndex, tdIndex);
    },
    handleSelectItem(record) {
      this.$emit("handleSelectItem", record);
    },
    handleColAdd(e, list) {
      this.$emit("handleColAdd", e, list);
    }
  }
};
</script>
<style scoped>
.min-height_50 {
    min-height: 50px !important;
}
</style>
