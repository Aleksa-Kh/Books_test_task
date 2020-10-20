<template>
  <div class="book_container">
    <h1>Книги</h1>
    <hr>
    <div class="tag_container">
      <div v-for="(cat, ind) in categories" :key='ind'
        class="tag_item btn" :class="{selected: cat.selected}" v-on:click="selectCategory(ind)"
      >{{cat.name}}</div>
    </div>
    <BookItem v-for="(book, ind) in showedBooks" :key='ind' :item="book" />
    <div v-show="showLoadButton" v-on:click="showedBooksCount+=10" class="load btn">Загрузить ещё</div>
  </div>
</template>

<script>
import BookItem from '@/components/BookItem.vue'

export default {
  name: 'Books',
  props: {
    msg: String
  },
  components: {
    BookItem
  },
  data: () => ({
    categories: [],
    books: [],
    showedBooksCount: 0,
    loadNext: false,
    page: 1
  }),
  computed: {
    selectedCategories () {
      let arSelected = this.categories.filter(item => item.selected).map(el => el.id)
      if (arSelected.length === 0) {
        arSelected = this.categories.map(el => el.id)
      }
      return arSelected
    },
    showedBooks () {
      if (this.showedBooksCount > this.books.length && this.loadNext) {
        this.changeBooksList(false)
      }
      return this.books.slice(0, this.showedBooksCount)
    },
    showLoadButton () {
      return this.showedBooksCount < this.books.length || this.loadNext
    }
  },
  async mounted () {
    const res = await fetch(
      'https://webdev.modumlab.com/api/book/categories',
      {
        method: 'post',
        headers: {
          'Content-type': 'application/json; charset=UTF-8'
        },
        body: ''
      }
    )
    const categories = await res.json()
    this.getCategories(categories)
  },
  methods: {
    getCategories (obj) {
      obj.data.list.forEach(el => {
        this.categories.push({
          id: el.id,
          name: el.name,
          selected: false
        })
      })
      this.changeBooksList()
    },
    async changeBooksList (newLoad = true) {
      let arBooks = []
      if (newLoad) {
        this.books = []
        this.showedBooksCount = 10
        this.page = 1
        this.loadNext = false
      }

      try {
        const res = await fetch(
          'https://webdev.modumlab.com/api/book/list',
          {
            method: 'post',
            headers: {
              'Content-type': 'application/json; charset=UTF-8'
            },
            body: JSON.stringify({
              categories: this.selectedCategories,
              page: this.page
            })
          }
        )
        arBooks = await res.json()

        arBooks.data.list.forEach(el => {
          this.books.push(el)
        })
        this.loadNext = arBooks.data.next
        this.page = arBooks.data.page + 1
      } catch (error) {
      }
    },
    selectCategory (ind) {
      this.categories[ind].selected = !this.categories[ind].selected

      this.changeBooksList()
    }
  }
}
</script>

<style scoped lang="scss">
$green: #13a89e;

.book_container {
  padding: 1em;
}

h1 {
  text-align: center;
}

.btn {
  border-radius: .2em;
  padding: .5em 1em;
  cursor: pointer;
  text-align: center;
  transition: background-color 0.5s ease;
}

.tag {
  &_container {
    margin: .5em;
  }

  &_item {
    display: inline-block;
    margin-right: .5em;
    margin-top: .5em;
    background-color: #f0f5f4;

    &.selected {
      background-color: $green;
      color: #fff;
    }
  }
}

.load {
  margin: .5em auto;
  width: 15em;
  color: #fff;
  background-color: $green;

  &:hover {
    background-color: #bcbcbc;
  }
}
</style>
