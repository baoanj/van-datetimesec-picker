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
