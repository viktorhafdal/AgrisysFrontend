<script setup lang="ts">
  import { ref, type Ref } from 'vue'

  interface WidgetToggleItem 
  {
    label: string;
    value: Ref<boolean>;
  }

  defineProps<{
    widgetToggles: WidgetToggleItem[],
  }>()

  const widgetsPopover = ref();
  const showWidgetMenu = (event: MouseEvent | Event) => {
  if (widgetsPopover.value && typeof widgetsPopover.value.toggle === 'function') {
    widgetsPopover.value.toggle(event);
  }
}

</script>

<template>
  <div>
    <Button type="button" label="Widgets" @click="showWidgetMenu" />
    
    <Popover ref="widgetsPopover">
      <div class="flex flex-col gap-4">
        <div class="card flex justify-between items-center gap-4" v-for="widget in widgetToggles"
          :key="widget.label">
          <span>{{ widget.label }}</span>
          <ToggleSwitch v-model="widget.value.value">
            <template #handle="{ value: toggleValue }">
              <i :class="['!text-xs pi', { 'pi-check': widget.value.value, 'pi-times': !widget.value.value }]" />
            </template>
          </ToggleSwitch>
        </div>
      </div>
    </Popover>
  </div>
</template>
