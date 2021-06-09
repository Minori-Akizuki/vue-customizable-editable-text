# vue-customizable-editable-text

## description

This component provides simple editable text. Click (or double-click) to make the text editable, and press enter to confirm. The edited text can be restored to its original state.

## install
```
npm install vue-customizable-editable-text
```

## usage

### sample code

:WIP!:

```vue
<template>
  <div id="app">
    <vue-customizable-editable-text
      ref="editable-text"
      v-model="msg"
      identify="editableTextIdentify"
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
import VueCustomizableEditableText from 'vue-customizable-editable-text'

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

```

### v-bind

Any strings. Its be binding.

### propertyes

| name | type | required | default | description |
|:-----|:-----|:---------|:--------|:------------|
| disabled | `String` | false | `false` | Disebled editable mode. |
| origValue | `String` | false | `false` | You may define any string to original. for example, it for useful in table by Bootstrap-Vue. |
| identify | `String` or `Object` | false | `null` | This is used to identify an element. It is returned as an argument to the event. |
| dblClickMode | `Boolean` | false | `false` | If this is set to true, clicking to switch modes will be disabled and double-clicking will be enabled instead.  This setting overrides `dblClickModeEdit` and `dblClickModeRevert`.|
| dblClickModeEdit | `Boolean` | false | `false` | Double-clicking is required when enabling editing. |
| dblClickModeRevert | `Boolean` | false | `false` | You will need to double-click to revert. |

### methods

| usage | return | description |
|:------|:-------|:------------|
| `editableOn()` | `void` | Turn on editable mode force. |
| `editableOff()` | `void` | Turn off editable mode force. |
| `origin()` | `String` | Get original text |
| `isEdited()` | `Boolean` | return the text is edited |
| `revertValue()` | `String` | Revert text force |

### events

| name, arguments | description |
|:----------------|:------------|
| `editableOn(identify)` | When edit mode on. |
| `editableOff(identify)` | When the edit mode off. |
| `editedValue(identify, value, origin)` | When the text has been changed. |
| `revert(identify, origin)` | When the text reverted. |

### template

all template is not requred. but it is SO simply.

| name | description |
|:-----|:------------|
| `textbox` | You can put any types customiszed input area. Please don't forget assign slot contents. |
| `default` | Visible when not turn on editable mode |
| `placeholder` | If the content is an empty string, the content you set will be displayed instead. |
| `revert` | You can put any contents to in palce revert buttons. For example, icon, card, text. | 


## tips

### auto-focus, blur

As sample code, this component can not reference that text box has own. You have to catch event, and focus or turn off edit mode.