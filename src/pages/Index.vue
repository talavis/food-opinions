<template>
<q-page class="flex justify-center">
  <q-list>
    <q-item>
      <q-input v-model="filterText" label="Filter" class="fit"/>
    </q-item>
    <q-item>
      <div class="row">
        <q-chip v-for="tag in filterTags"
                :key="tag"
                :label="tag"
                square
                color="primary"
                text-color="white"
                removable
                @remove="removeFilterTag(tag)" />
      </div>
    </q-item>

    <q-item v-for="review in visible" :key="review.title">
      <review-entry :data="review" @tag-clicked="addFilterTag"/>
    </q-item>
  </q-list>
</q-page>
</template>

<script>
import { defineComponent } from 'vue';

import ReviewEntry from 'components/ReviewEntry.vue';

import reviewData from 'assets/reviews.json';

export default defineComponent({
  name: 'PageIndex',

  computed: {
    visible: {
      get() {
        let result = JSON.parse(JSON.stringify(reviewData))
        for (let tag of this.filterTags) 
          result = result.filter((entry) => entry.tags.includes(tag))

        let filterTextLow = this.filterText.toLowerCase()
        if (this.filterText.length > 0)
          result = result.filter((entry) => entry.title.toLowerCase().includes(filterTextLow) || entry.comment.toLowerCase().includes(filterTextLow))
        return result;
      }
    }
  },

  data () {
    return {
      filterText: '',
      filterTags: []
    }
  },

  components: {
    "review-entry": ReviewEntry
  },

  methods: {
    removeFilterTag(tag) {
      this.filterTags = this.filterTags.filter((entry) => entry !== tag)
    },

    addFilterTag(tag) {
      if (!this.filterTags.includes(tag))
        this.filterTags.push(tag)
    }
  }
  
})
</script>
