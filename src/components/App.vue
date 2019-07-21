<template>
  <div class="container-fluid p-0 ">

    <div class="row">
      <div class="col">
        <span class="hint">Proposed Investment</span>
      </div>
    </div>

    <div class="row">
      <div class="col">
        <textarea class="input" :rows="1" v-model="investment"> </textarea>
      </div>
    </div>

    <div class="row">
      <div class="col">
        <span class="hint">For Equity (%)</span>
      </div>
    </div>

    <div class="row">
      <div class="col">
        <textarea class="input" :rows="1" v-model="equity"> </textarea>
      </div>
    </div>

    <div class="row">
      <div class="col">
        <span class="hint">Pre-money Valuation (before investment)</span>
      </div>
    </div>

    <div class="row">
      <div class="col">
        <textarea class="input" :rows="1" v-model="premoney">
        </textarea>
      </div>
    </div>
    <div class="row">
      <div class="col">
        <span class="hint">Valuation After Investment (Postmoney)</span>
      </div>
    </div>
    <div class="row">
      <div class="col">
        <textarea class="input" :rows="1" v-model="postmoney" readonly>
        </textarea>
      </div>
    </div>
  </div>
</template>

<script>
  import LinkLogo from 'images/Link.svg';

  const _ = require( 'lodash' );

  export default {
    name: 'app',

    components: {
      LinkLogo,
    },

    data: function () {
      return {
        options: {},
        investment: 1,
        equity: 10,
        premoney: 9,
        postmoney: 10,
        changing: false,
      }
    },

    created() {
    },

    watch: {
      investment() {
        if ( !this.changing ) {
          this.doChange()
          let e = Number( this.equity ) / 100
          this.premoney = (1 - e) / e * Number( this.investment );
        }
      },

      equity() {
        if ( !this.changing ) {
          this.doChange()
          let e = Number( this.equity ) / 100
          this.premoney = ((1 - e) / e * Number( this.investment ))
        }
      },

      premoney() {
        let pm = Number( this.premoney )
        let i = Number( this.investment )
        if ( !this.changing ) {
          this.doChange()
          this.equity = (i / (pm + i) * 100).toFixed(2)
        }
        this.postmoney = Number( this.premoney ) + Number( this.investment )
      },

    },

    methods: {
      doChange() {
        this.changing = true
        _.debounce( () => {
          this.changing = false
        }, 10, { leading: false } )()
      },
    },
  }
</script>

<style lang="scss" scoped>

  .options {
    font-family: 'Share Tech Mono', sans-serif;
    font-size: 0.75rem;
    color: #666;
    text-transform: uppercase;
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

  .error {
    background: rgba(200, 100, 100, 0.25) !important;
  }

  label, .message, .hint {
    margin-bottom: 0;
    color: #666;
    font-size: 0.9rem;
    font-family: 'Share Tech Mono', sans-serif;
  }

  .hint {
    color: #999;
  }

</style>
