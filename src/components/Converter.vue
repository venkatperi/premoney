<template>
  <div class="container-fluid converter form-group p-0">
    <div class="row">
      <div class="col" style="min-height:27px;">
        <span class="message" v-html="errorMessage"></span>
      </div>
      <div class="col-1 float-right ">
        <slot name="top-right" :text="entry"></slot>
      </div>
    </div>
    <div class="row">
      <div class="col">
        <div class="input-outer">
          <transform-text-area
            v-model="entry"
            rows="1"
            :title="caption"
            :placeholder="caption"
            :transform="preprocessor"
            :class="['form-control','input', error? 'error-border':'']"
          />
          <div
            v-if="!empty"
            class="counter"
            v-html="counter"></div>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col">
        <span class="hint" v-html="hint"></span>
      </div>
    </div>
  </div>
</template>

<script>
  import TransformTextArea from './TransformTextArea'

  // noinspection JSUnusedGlobalSymbols
  export default {
    name: 'converter',
    components: { TransformTextArea },
    props: {
      value: [Number, String],

      process: [Number, String],

      hint: String,

      options: Object,

      caption: {
        type: String,
        required: true,
      },

      converter: {
        type: Function,
        default: x => x,
      },

      preprocessor: {
        type: Function,
        default: x => x,
      },
    },

    data() {
      return {
        entry: '',
        result: '',
        error: undefined,
      }
    },

    watch: {
      value( x ) {
        this.entry = x
      },

      process( x ) {
        this.entry = x
        this.updateResult()
      },

      error() {
        this.error && console.log( this.error )
      },

      options: {
        handler() {
          this.updateResult()
        },
        deep: true,
      },

      result( r ) {
        this.$emit( 'result', r );
      },

    },

    methods: {
      updateResult() {
        this.error = undefined
        try {
          this.result = this.converter( this.entry, this.options )
        } catch ( e ) {
          this.error = e
        }
      },
    },

    computed: {
      counter() {
        return (this.entry || '').toString().length
      },

      errorMessage() {
        return this.error ? this.error.message : '&nbsp;'
      },

      empty() {
        return this.counter === 0
      },
    },
  }
</script>


<style lang="scss" scoped>

  label, .message, .hint {
    margin-bottom: 0;
    color: #666;
    font-size: 0.9rem;
    font-family: 'Share Tech Mono', sans-serif;
  }

  .hint {
    color: #999;
  }

  label {
    font-weight: 600;
  }

  .message {
    color: red;
  }

  .input-outer {
    position: relative;
  }

  .counter {
    position: absolute;
    top: 10px;
    right: 10px;
    font-family: 'Share Tech Mono', monospace;
    font-size: 12px;
  }

  textarea.input {
    resize: none;

    color: rgba(0, 0, 0, 0.7);
    width: 100%;
    padding: 0 20px;
    min-height: 80px;
    max-height: 300px;

    font-family: 'News Cycle', sans-serif;
    font-size: 3rem;
    text-overflow: clip;
    overflow-wrap: break-word;
    position: relative;
    text-align: left;
    border-radius: 0;
    background: rgba(125, 140, 115, 0.5);
    text-shadow: 0 0 7px rgba(255, 255, 255, 0.25);
    box-shadow: inset 0 0 25px rgba(0, 0, 0, 0.25);
    &::placeholder {
      text-align: center;
      vertical-align: middle;
      font-family: 'Share Tech Mono', monospace;
      font-size: 0.25em;
    }
  }

  .decimal textarea {
    font-family: 'Share Tech Mono', monospace;
  }

  .error-border {
    background: rgba(200, 100, 100, 0.25) !important;
  }

</style>
