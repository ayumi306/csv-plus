<template lang="pug">
div.control-panel
  toolbar
    toolbar-switch(
      :title="$t('control_panel.add')"
      icon="file"
      @click="add"
    )
    toolbar-switch(
      :title="$t('control_panel.open')"
      icon="folder-open"
      @click="open"
    )
    toolbar-switch(
      :title="$t('control_panel.save')"
      icon="save"
      @click="save"
    )
    toolbar-separator
    toolbar-switch(
      v-model="options.enableSearch"
      :title="$t('control_panel.search')"
      icon="search"
    )
  toolbar
    // Has header
    toolbar-switch(
      v-model="options.hasHeader"
      :title="$t('control_panel.set_header')"
      icon="table-header"
    )

    // TODO: Search
    //text-input(
    //  v-model="keywords.search"
    //  icon="search"
    //)
    //toolbar-separator

    // TODO: Delimiter
    //toolbar-radio(
    //  v-model="options.delimiter"
    //  value=","
    //  title="コンマ区切り"
    //  icon="comma"
    //  size="33"
    //)
    //toolbar-radio(
    //  v-model="options.delimiter"
    //  value="\t"
    //  title="タブ区切り"
    //  icon="tab"
    //)
    //toolbar-separator
</template>

<script lang="ts">
import {
  defineComponent,
  PropType,
  WritableComputedRef,
} from 'vue'
import { Options } from '@/renderer/types'
import vModel from '@/renderer/utils/v-model'
import Toolbar from '@/renderer/components/ControlPanel/Toolbar.vue'
import ToolbarSwitch from '@/renderer/components/ControlPanel/ToolbarSwitch.vue'
import ToolbarRadio from '@/renderer/components/ControlPanel/ToolbarRadio.vue'
import ToolbarSeparator from '@/renderer/components/ControlPanel/ToolbarSeparator.vue'
import TextInput from '@/renderer/components/Form/TextInput.vue'
import useEvents from './composables/useEvents'
import registerListeners from './composables/registerListeners'

export default defineComponent({
  name: 'ControlPanel',
  components: {
    TextInput,
    Toolbar,
    ToolbarSwitch,
    ToolbarRadio,
    ToolbarSeparator,
  },
  props: {
    modelValue: { type: Object as PropType<Options> },
  },
  setup (props, context) {
    const options = vModel('modelValue', props, context)
    const events = useEvents(context)

    registerListeners(options as WritableComputedRef<Options>)

    return {
      options,
      ...events,
    }
  },
})
</script>
