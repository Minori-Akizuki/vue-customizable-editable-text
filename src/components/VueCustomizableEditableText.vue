<template>
  <span>
    <!-- on editable -->
    <span
      v-if="isEditable"
      ref="content-input"
      @keypress.enter="editableOff"
      @input="$emit('input', $event.target.value)"
    >
      <slot
        name="textbox"
        :value="value"
      >
        <input
          :value="value"
          type="text"
        />
      </slot>
    </span>
    <!-- or not editable -->
    <span
      v-else
      ref="plain-text"
      class="box"
      @dblclick="editableOn"
      @click="editableOn"
    >
      <slot
        v-if="value"
        :value="value"
        name="default"
      >
        {{ value }}
      </slot>
      <slot
        v-else
        name="placeholder"
      >
        (none)
      </slot>
    </span>
    <span
      v-if="isEdited()"
      @dblclick="revertValue"
      @click="revertValue"
    >
      <slot
        name="revert"
      >
        <button>
          revert
        </button>
      </slot>
    </span>
  </span>
</template>

<script>
export default {
  props: {
    value: {
      type: String,
      required: true,
      default: ''
    },
    origValue: {
      type: String,
      required: false,
      default: null
    },
    identify: {
      type: [Object, String],
      required: false,
      default: () => null
    },
    dblClickMode: {
      type: Boolean,
      required: false,
      default: false
    },
    dblClickModeEdit: {
      type: Boolean,
      required: false,
      default: false
    },
    dblClickModeRevert: {
      type: Boolean,
      required: false,
      default: false
    },
    disabled: {
      type: Boolean,
      required: false,
      default: false
    }
  },
  data () {
    return {
      isEditable: false,
      originText: '',
    }
  },
  mounted () {
    this.originText = this.value
  },
  methods: {
    isDblClickModeEdit () {
      return this.dblClickMode || this.dblClickModeEdit
    },
    isDblClickModeRevert () {
      return this.dblClickMode || this.dblClickModeRevert
    },
    isClick (event) {
      return event.type === 'click'
    },
    editableOn (event) {
      if (event && this.isClick(event) && this.isDblClickModeEdit()){
        return
      }
      console.log
      if (this.disabled){
        return
      }
      this.isEditable = true
      this.$emit('editableOn', this.identify)
    },
    editableOff () {
      // Enterした時にフォーカスアウトで2回ここに来るからガードつけておく
      if (!this.isEditable) {
        return
      }
      this.isEditable = false
      if (this.isEdited()) {
        this.$emit('editedValue', this.identify, this.value, this.origin())
      }
      this.$emit('editableOff', this.identify)
    },
    origin () {
      return (this.origValue === null) ? this.originText : this.origValue
    },
    isEdited () {
      return this.value !== this.origin()
    },
    revertValue (event) {
      if ( event && (this.isClick(event) && this.isDblClickModeRevert())){
        return
      }
      this.$emit('input', this.origin())
      this.$emit('revert', this.identify, this.origin())
      this.isEditable = false
      return this.origin()
    }
  }
}
</script>

<style>
</style>