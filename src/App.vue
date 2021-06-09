<template>
  <div id="app">
    <vue-customizable-editable-text
      ref="editable-text"
      v-model="msg"
      @editableOn="editableOn"
    >
      <!-- on editable -->
      <template #textbox="v">
        &lt;
        <input
          :value="v.value"
          ref="input"
          @blur="blur"
        />
        &gt;
      </template>

      <!-- or not editable -->
      <template #default="v">
        <span style="font-style: italic;">
          {{v.value}}
        </span>
      </template>

      <!-- revert button -->
      <template #revert>
        <button>
          revert this
        </button>
      </template>
    </vue-customizable-editable-text>
    <div>
      {{ msg }}
    </div>
  </div>
</template>

<script>
import VueCustomizableEditableText from './components/VueCustomizableEditableText.vue'

export default {
  name: 'App',
  components: {
    VueCustomizableEditableText
  },
  data () {
    return {
      msg: 'hello'
    }
  },
  methods: {
    blur () {
      console.log('[app.vue] catch blur', ...arguments)
      this.$refs['editable-text'].editableOff()
    },
    editableOn () {
      this.$nextTick(()=>{
        this.$refs['input'].select()
      })
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
