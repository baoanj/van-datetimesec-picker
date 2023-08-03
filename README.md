# 基于 Vant 的支持秒的日期时间组件

依赖 `vue` `sass` `vant`

## Props

| 参数 | 说明 | 类型 | 默认值 |
| ---- | ---- | ---- | ----- |
| v-model (value) | 值（时间戳） | *number* | 当前时间 |
| datetimePickerProps | van-datetime-picker 的 props | *object* | - |
| pickerProps | van-picker 的 props | *object* | - |

## Events

| 事件 | 说明 | 回调参数 |
| ---- | ---- | ------- |
| cancel | 点击取消时触发 | 无

## Example

```html
<template>
  <div>
    <van-datetimesec-picker
      v-model="dateTime"
      :datetimePickerProps="{ 'visible-item-count': 3, title: '选择完整时间' }"
      :pickerProps="{ 'visible-item-count': 3 }"
    />
    <van-field
      label="日期时间"
      :value="new Date(dateTime).toLocaleString()"
      readonly
    />
    <van-field
      label="时间戳"
      :value="dateTime"
      @input="dateTime = +$event"
      type="number"
    />
  </div>
</template>

<script>
import VanDatetimesecPicker from 'van-datetimesec-picker'

export default {
  components: { VanDatetimesecPicker },
  data() {
    return {
      dateTime: Date.now()
    }
  }
}
<script>
```

## Custom

想要定制化开发，可以直接使用源码，也就70+行

```html
<template>
  <div class="van-datetimesec-picker">
    <van-datetime-picker
      v-model="dateValue"
      type="datetime"
      v-bind="datetimePickerProps"
      @cancel="$emit('cancel')"
      @confirm="handleConfirm"
    />
    <van-picker
      :columns="Array(60).fill().map((_, i) => `0${i}`.slice(-2))"
      v-bind="pickerProps"
      :default-index="secondIdx"
      @change="handleChange"
    />
  </div>
</template>

<script>
export default {
  name: 'van-datetimesec-picker',
  props: {
    datetimePickerProps: Object,
    pickerProps: Object,
    value: Number
  },
  data() {
    return {
      secondIdx: 0,
      dateValue: null
    }
  },
  watch: {
    value: {
      handler() {
        this.dateValue = new Date(this.value)
        this.secondIdx = this.dateValue.getSeconds()
      },
      immediate: true
    }
  },
  methods: {
    handleConfirm(val) {
      val.setSeconds(this.secondIdx)
      this.$emit('input', val.getTime())
    },
    handleChange() {
      this.secondIdx = arguments[2]
    }
  }
}
</script>

<style lang="scss" scoped>
.van-datetimesec-picker {
  display: flex;
  .van-picker:first-of-type {
    flex: 5;
    ::v-deep {
      .van-picker__toolbar {
        width: 120%;
      }
      .van-picker__frame {
        width: 100%;
      }
    }
  }
  .van-picker:last-of-type {
    flex: 1;
    margin-top: 44px;
  }
}
</style>
```
