<template>
  <img :src="defaultSrc" class="vux-x-img" :class="cls"/>
</template>

<script>
import Blazy from 'vux-blazy'
import webpSupport from 'webp-support'
import uuidMixin from '../../libs/mixin_uuid'

export default {
  mixins: [uuidMixin],
  created(){
    this.props_src=this.src
  },
  mounted () {
    // use webp or default
    if (webpSupport() && this.props_src && this.webpSrc) {
      this.props_src = this.webpSrc
    }
  },
  mounted () {
    debugger
    const _this = this
    const id = `vux-ximg-${this.uuid}`
    this.$el.setAttribute('id', id)
    this.$el.setAttribute('data-src', this.props_src)
    this.blazy = new Blazy({
      scroller: this.scroller,
      container: this.container,
      selector: `#${id}`,
      offset: _this.offset,
      errorClass: _this.errorClass,
      successClass: _this.successClass,
      success (ele) {
        _this.$emit('on-success', _this.props_src, ele)
      },
      error (ele, msg) {
        _this.$emit('on-error', _this.props_src, ele, msg)
      }
    })
  },
  data(){
    return {
      props_src:null
    }
  },
  props: {
    src: String,
    webpSrc: String,
    defaultSrc: {
      type: String,
      default: 'data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw=='
    },
    errorClass: String,
    successClass: String,
    offset: {
      type: Number,
      defaut: 100
    },
    cls: String,
    scroller: Object,
    container: String
  },
  beforeDestroy () {
    this.blazy && this.blazy.destroy()
  }
}
</script>

<style>
.b-lazy {
  transition: opacity 500ms ease-in-out;
  max-width: 100%;
  opacity: 0;
}

.b-lazy.b-loaded {
  opacity: 1;
}
</style>
