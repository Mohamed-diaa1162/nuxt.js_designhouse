<template>
  <div>
    <input
      class="form-control form-control-lg font-14 fw-300"
      :placeholder="placeholder"
      :type="inputType"
      :value="value"
      :class="{ 'is-invalid': form.errors.has(field) }"
      @input="handleInput"
    />
    <HasError :form="form" :field="field" />
  </div>
</template>
<script>
import HasError from 'vform/src/components/bootstrap5/HasError.vue'
export default {
  name: 'base-input',
  props: {
    value: {
      type: String,
      default: '',
    },
    placeholder: {
      type: String,
      default: '',
    },
    inputType: {
      type: String,
      default: 'text',
    },
    form: {
      type: Object,
      required: true,
    },
    field: {
      type: String,
      required: true,
    },
  },

  components: { HasError },

  methods: {
    handleInput(e) {
      this.$emit('input', e.target.value)
    },
  },
}
</script>
