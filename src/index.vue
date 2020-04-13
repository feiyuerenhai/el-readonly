<template>
  <div class="fly-readonly" :style="{display: inline ? 'inline-block': 'block'}">
    <div
      v-if="enabled"
      class="text"
      :class="{ellipsis: ellipsis}"
      :title="text"
    >{{text + '' || defaultText}}</div>
    <div class="inner" :class="{none: enabled}">
      <slot />
    </div>
  </div>
</template>

<script>
export default {
  name: "fly-readonly",
  props: {
    inline: { type: Boolean, default: false },
    enabled: { type: Boolean, default: false },
    ellipsis: { type: Boolean, default: true },
    formatter: { type: Function },
    "default-text": { type: String, default: "-" }
  },
  data() {
    return {
      text: "",
      slotInstance: this.$slots.default[0].componentInstance
    }
  },
  methods: {
    _getValueText(tag, componentInstance) {
      if (this.formatter) {
        return this.formatter(componentInstance.value)
      }
      if (tag.indexOf("ElSelect") !== -1) {
        return componentInstance.selectedLabel
      }
      if (tag.indexOf("ElInput") !== -1) {
        return componentInstance.value
      }
      if (tag.indexOf("ElCheckboxGroup") !== -1) {
        return Object.values(componentInstance.value).join(", ")
      }
      // if (tag.indexOf("ElRadioGroup") !== -1) {
      //   // 暂未实现
      // }
      return componentInstance.value
    },
    updateText(tag, componentInstance) {
      this.text = this._getValueText(tag, componentInstance)
    }
  },
  watch: {
    "slotInstance.value": {
      handler() {
        const { tag } = this.$slots.default[0]
        this.updateText(tag, this.slotInstance)
      },
      deep: true
    },
    "slotInstance.selectedLabel": {
      handler() {
        const { tag } = this.$slots.default[0]
        this.updateText(tag, this.slotInstance)
      },
      deep: true
    }
  },
  mounted() {
    if (!Object.getOwnPropertyNames(this.$slots).length) return
    const { componentInstance } = this.$slots.default[0]
    this.slotInstance = componentInstance
  }
}
</script>

<style lang="less" scoped>
.fly-readonly {
  vertical-align: middle;
  .text {
    &.ellipsis {
      width: 100%;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
      word-break: break-all;
    }
  }
  .inner {
    display: inline-block;
    width: 100%;
    &.none {
      display: none;
    }
  }
}
</style>
