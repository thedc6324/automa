<template>
  <div class="px-4 overflow-auto scroll pb-1">
    <div class="sticky top-0 z-20 bg-white border-b border-gray-100 pb-4 mb-4">
      <button class="mr-2 align-middle" @click="$emit('close')">
        <v-remixicon name="riArrowLeftLine" />
      </button>
      <p class="font-semibold inline-block align-middle">
        {{ t(`workflow.blocks.${data.id}.name`) }}
      </p>
    </div>
    <component
      :is="data.editComponent"
      v-if="blockData"
      :key="data.blockId"
      v-model:data="blockData"
      :block-id="data.blockId"
    />
  </div>
</template>
<script>
import { computed } from 'vue';
import { useI18n } from 'vue-i18n';

const editComponents = require.context(
  './edit',
  false,
  /^(?:.*\/)?Edit[^/]*\.vue$/
);

/* eslint-disable-next-line */
const components = editComponents.keys().reduce((acc, key) => {
  const name = key.replace(/(.\/)|\.vue$/g, '');
  const componentObj = editComponents(key)?.default ?? {};

  acc[name] = componentObj;

  return acc;
}, {});

export default {
  components,
  props: {
    data: {
      type: Object,
      default: () => ({}),
    },
  },
  emits: ['close', 'update'],
  setup(props, { emit }) {
    const { t } = useI18n();

    const blockData = computed({
      get() {
        return props.data.data || {};
      },
      set(value) {
        emit('update', value);
      },
    });

    return {
      t,
      blockData,
    };
  },
};
</script>
