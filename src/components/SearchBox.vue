<script>
export default {
  data() {
    return {
      searchQuery: null,
      users: [],
      selectedId: 0
    }
  },
  beforeMount() {
    fetch('https://jsonplaceholder.typicode.com/users')
      .then(r => r.json())
      .then(json => {
        this.users = json
      })
  },
  mounted() {
    window.addEventListener('keydown', this.handleKeyDown);
  },
  destroyed() {
    window.removeEventListener('keydown', this.handleKeyDown);
  },
  methods: {
    select(id) {
      this.selectedId = id
    },
    handleKeyDown(e) {
      switch (e.keyCode) {
        // In case of left arrow key move to the last item
        case 37:
          if (this.selectedId > 0) {
            this.select(this.selectedId - 1)
          }
          // Prevent the default scroll event from firing
          e.preventDefault()
          break
          // In case of up arrow key, move to the last item
        case 38:
          if (this.selectedId > 0) {
            this.select(this.selectedId - 1)
          }
          e.preventDefault()
          break;
          // In case of right arrow key, move to the next item
        case 39:
          if (this.selectedId < this.searchFilter.length - 1) {
            this.select(this.selectedId + 1)
          }
          e.preventDefault()
          break;
          // In case of down arrow key, move to the next item
        case 40:
          if (this.selectedId < this.searchFilter.length - 1) {
            this.select(this.selectedId + 1)
          }
          e.preventDefault()
          break;
      }
    }
  },
  computed: {
    searchFilter() {
      if (this.searchQuery != '' && this.searchQuery != null) {
        return this.users.filter((item) => {
          return item.name.toLowerCase().includes(this.searchQuery)
        })
      } else {
        return null
      }
    }
  }
}
</script>

<template>
<input type="text" v-model="searchQuery" placeholder="Select a User" />
<ul class="results" v-if="users.length">
  <li v-for="item in searchFilter" :key="item.id" :class="item.id === selectedId ? 'selected' : ''">{{item.name}}<span>{{item.phone}}</span></li>
</ul>
</template>
