<template>
    <textarea
      :value="internalValue"
      :title="title"
      :readonly="readonly"
      :rows="rows"
      :cols="cols"
      :disabled="disabled"
      @input="internalValue = $event.target.value"
    > </textarea>
</template>

<script>
  export default {
    name: 'transform-text-area',

    props: {
      value: { required: false },
      title: String,
      readonly: Boolean,
      height: [Number, String],
      disabled: Boolean,
      important: Boolean,
      transform: {
        type: Function,
        default: x => x,
      },
      cols: {
        type: [Number, String],
        default: null,
      },
      rows: {
        type: [Number, String],
        default: null,
      },
      minHeight: {
        type: [Number],
        default: null,
      },
      maxHeight: {
        type: [Number],
        default: null,
      },
    },

    data: vm => ({
      lazyValue: vm.value,
    }),

    computed: {
      internalValue: {
        get() {
          return this.lazyValue
        },
        set( val ) {
          this.lazyValue = this.transform( val )
          this.$emit( this.$_modelEvent, this.lazyValue )
        },
      },
    },

    beforeCreate() {
      this.$_modelEvent = (this.$options.model && this.$options.model.event) || 'input'
    },

    watch: {
      value( x ) {
        this.lazyValue = this.transform( x )
      },
      lazyValue() {
        this.$nextTick( this.resize )
      },
    },

    methods: {
      resize() {
        const important = this.isHeightImportant ? 'important' : ''
        this.$el.style.setProperty( 'height', 'auto', important )

        let contentHeight = this.$el.scrollHeight + 1
        if ( this.minHeight ) {
          contentHeight = contentHeight < this.minHeight ? this.minHeight : contentHeight
        }
        if ( this.maxHeight ) {
          if ( contentHeight > this.maxHeight ) {
            contentHeight = this.maxHeight
            this.maxHeightScroll = true
          } else {
            this.maxHeightScroll = false
          }
        }
        const heightVal = `${contentHeight}px`
        // this.$el.style.setProperty( 'height', '0', important )
        this.$el.style.setProperty( 'height', heightVal, important )
        return this
      },
    },

  }
</script>


