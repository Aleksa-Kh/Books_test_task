<template>
  <div class="book">
    <div><span class="caption">Название книги:</span> {{item.name}}</div>
    <div><span class="caption">Описание книги:</span> {{item.description}}</div>
    <div><span class="caption">Имя автора:</span> {{item.author}}</div>
    <div><span class="caption">Год выпуска книги:</span> {{item.year}}</div>
  </div>
</template>

<script>
export default {
  name: 'BookItem',
  props: {
    item: Object
  }
}
</script>

<style scoped lang="scss">
.book {
  border-top: 1px solid #ddd;
  color: #0b2423;
  padding: 1em;
}

.caption {
  color: #BCBCBC;
}
</style>
