<template>
  <div class="v-field">
    <component :is="fieldset ? 'fieldset' : 'p'">
      <div>
        <div class="label">
          <component :is="fieldset ? 'legend' : 'label'" :for="field.field">
            {{ field.name }}<i v-tooltip="$t('required')" class="material-icons" v-if="field.required">star</i>
          </component>
          <label v-if="batchMode">
            <v-toggle
              :value="!blocked"
              @input="$emit(blocked ? 'activate' : 'deactivate', field.field)" />
          </label>
        </div>
        <small v-if="!readonly && field.comment">{{ field.comment }}</small>
        <div class="field-wrapper">
          <v-interface
            :id="field.interface"
            :name="field.field"
            :required="Boolean(field.required)"
            :readonly="readonly || blocked"
            :options="field.options"
            :type="field.type"
            :value="values[field.field]"
            :relationship="field.relationship"
            @input="readonly ? null : $emit('stage-value', {
              field: field.field,
              value: $event
            })"
            @setfield="readonly ? null : $emit('stage-value', {
              field: $event.field,
              value: $event.value,
            })" />
          <div class="blocker" v-if="blocked" @click="$emit('activate', field.field)" />
        </div>
      </div>
    </component>
  </div>
</template>

<script>
export default {
  name: "v-field",
  props: {
    field: {
      type: Object,
      required: true
    },
    values: {
      type: Object,
      required: true
    },
    readonly: {
      type: Boolean,
      default: false
    },
    blocked: {
      type: Boolean,
      default: false
    },
    batchMode: {
      type: Boolean,
      default: false
    }
  },
  computed: {
    fieldset() {
      const interfaceInfo = this.$store.state.extensions.interfaces[
        this.field.interface
      ];

      return (interfaceInfo && interfaceInfo.fieldset) || false;
    }
  }
};
</script>

<style lang="scss" scoped>
.field-wrapper {
  position: relative;
}

label,
legend {
  margin-bottom: 10px;
  text-transform: none;
  color: var(--dark-gray);
  font-size: 1rem;
  line-height: 1.18;
  font-weight: 500;
}

fieldset,
p {
  border: 0;
  padding: 0;
}

fieldset > div,
p {
  display: flex;
  flex-direction: column;
}

small {
  order: 2;
  margin-top: 10px;
  font-style: italic;
  font-size: 12px;
  color: var(--gray);
}

.label {
  display: flex;
  align-items: center;

  > * {
    display: inline-block;
    max-width: max-content;

    &:first-child {
      margin-right: 10px;
    }
  }

  i {
    color: var(--accent);
    vertical-align: super;
    font-size: 7px;
  }
}

.blocker {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  background-color: var(--body-background);
  opacity: 0.7;
  transition: opacity var(--fast) var(--transition-out);
  cursor: pointer;

  &:hover {
    opacity: 0.3;
    transition: opacity var(--fast) var(--transition-in);
  }
}
</style>
