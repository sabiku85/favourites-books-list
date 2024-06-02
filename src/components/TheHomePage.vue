<script setup>
import { ref, reactive, computed } from 'vue'

const allBooks = reactive([
  { title: 'name of the wind', author: 'patrick rothfuss', img: 'assets/1.jpg', isFavourite: ref(false) },
  { title: 'the way of kings', author: 'brandon sanderson', img: 'assets/2.jpg', isFavourite: ref(false) },
  { title: 'the final empire', author: 'brandon sanderson', img: 'assets/3.jpg', isFavourite: ref(false) }
])

const favBooks = computed(() => {
  return allBooks.filter((book) => book.isFavourite)
})

let books = allBooks

let isVisible = ref(true)
let isActiveList = ref(true)

function toggleVisibility() {
  return isVisible.value = !isVisible.value
}

function toggleFavourites(book) {
  return book.isFavourite = !book.isFavourite
}

function toggleActiveList(e) {
  if (e?.target.getAttribute('id') === 'favBooks') {
    books = favBooks
  } else {
    books = allBooks
  }

  return isActiveList.value = !isActiveList.value
}

</script>

<template>
  <h1>Good morning!</h1>
  <div class="wrapper">
    <input type="radio" name="listType" id="allBooks" @change="toggleActiveList" />
    <label for="allBooks" :class="{ active: isActiveList }">Your list of books</label>
    <input type="radio" name="listType" id="favBooks" @change="toggleActiveList" />
    <label for="favBooks" :class="{ active: !isActiveList }">Your favourites</label>
    <button type="button" @click="toggleVisibility">
      <span v-show="!isVisible">Show list</span>
      <span v-show="isVisible">Hide list</span>
    </button>
  </div>
  <ul v-if="isVisible">
    <li v-for="book in books" :key="book.title" :class="{ marked: book.isFavourite }" @click="toggleFavourites(book)">
      <img :src="book.img" :alt="book.title">
      <h3>{{ book.title }}</h3>
      <p>{{ book.author }}</p>
    </li>
  </ul>
</template>

<style>
.wrapper {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
  max-width: 340px;
  min-height: 150px;
}

.wrapper input {
  display: none;
}

.wrapper label {
  font-size: 21px;
  font-weight: 700;
  border-bottom: 3px dashed transparent;
  transition: 0.2s border-bottom-color;
}

.wrapper label:hover {
  cursor: pointer;
  border-bottom: 3px dashed indianred;
}

.wrapper label.active {
  border-bottom: 3px solid indianred;
}

.wrapper button {
  min-width: 90px;
  height: 40px;
  padding: 10px 15px;
  background-color: bisque;
  color: #555;
  outline: none;
  border: none;
  border-radius: 4px;
  font-family: inherit;
  font-size: 14px;
  font-weight: 700;
}

ul {
  display: flex;
  flex-wrap: wrap;
  row-gap: 30px;
  padding: 0;
  list-style: none;
}

ul li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  column-gap: 30px;
  width: 100%;
  padding: 15px;
  border: 1px solid gainsboro;
  border-radius: 4px;
  color: wheat;
}

ul li.marked {
  background-color: rgba(209, 2, 23, 0.5);
  box-shadow: 0 0 10px 1px wheat;
}

ul li h3 {
  text-transform: uppercase;
}

ul li p {
  text-transform: capitalize;
}
</style>
