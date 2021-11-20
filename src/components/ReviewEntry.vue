<template>
<q-card :class="'fit ' + (data.recommended === 1 ? 'bg-green-2' : data.recommended === 0 ? '' : 'bg-red-2')">
  <q-card-section>
    <div class="text-h6">
      {{ data.title }}
    </div>
    <div>
      <q-icon v-for="index in data.burn"
              :key="index"
              name="local_fire_department"
              color="red"
              size="sm" />
    </div >
    {{ data.comment }}
  </q-card-section>
  <q-card-section class="flex row">
    <q-chip v-for="tag in data.tags"
            :key="tag"
            :label="tag"
            clickable
            @click="addTag(tag)"
            square
            color="primary"
            text-color="white" />
    <q-space />
    
    <q-btn v-if="data.link.length > 0"
           label="Link"
           icon="fas fa-external-link-alt"
           no-caps
           dense
           flat
           type="a"
           :href="data.source" />
  </q-card-section>
</q-card>
</template>

<script>
import { defineComponent } from 'vue';

import reviewData from 'assets/reviews.json';

export default defineComponent({
  name: 'ReviewEntry',

  props: {
    data: {
      type: Object,
      required: true,
    }
  },

  methods: {
    addTag(tag) {
      this.$emit('tag-clicked', tag)
    }
  },

  emits: ['tag-clicked']
})
</script>
