<template>
  <div>
    <div v-show="isShowEditModal" class="modal-backdrop fade show"></div>
    <div
      class="modal fade"
      :class="isShowEditModal ? 'show' : ''"
      :style="isShowEditModal ? 'display: block' : 'display: none'"
      id="exampleModal"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <form @submit.prevent="updateBook">
            <div class="modal-header">
              <h1 class="modal-title fs-5" id="exampleModalLabel">
                Edit Students
              </h1>
              <button
                type="button"
                class="btn-close"
                @click="handleCloseEditModal"
                aria-label="Close"
              ></button>
            </div>
            <div class="modal-body">
              <div class="form-group">
                <label for="title">Title</label>
                <input
                  type="text"
                  class="form-control"
                  id="title"
                  v-model="book.title"
                />
              </div>
              <div class="form-group">
                <label for="author">Author</label>
                <input
                  type="text"
                  class="form-control"
                  id="author"
                  v-model="book.author"
                />
              </div>
              <div class="form-group">
                <label for="year">Year</label>
                <input
                  type="number"
                  class="form-control"
                  id="year"
                  v-model="book.year"
                />
              </div>
              <div class="form-group">
                <label for="cover">Cover</label>
                <input
                  type="url"
                  class="form-control"
                  id="cover"
                  v-model="book.cover"
                />
              </div>
            </div>
            <div class="modal-footer">
              <button
                type="button"
                class="btn btn-secondary"
                @click="handleCloseEditModal"
              >
                Close
              </button>
              <button
                type="submit"
                class="btn btn-primary"
                @click="handleSubmitEdit"
              >
                Save changes
              </button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import instanceAxios from '@/utils/configAxios'
import { onMounted, ref } from 'vue'

const book = ref({
  title: '',
  author: '',
  year: '',
  cover: '',
})

const props = defineProps([
  'isShowEditModal',
  'handleCloseEditModal',
  'editIdselected',
])

const emit = defineEmits(['handleEditBook'])

const fetchBook = async () => {
  try {
    const fetchBookReponsive = await instanceAxios.get(`books/${props.editIdselected}`)
    console.log(fetchBookReponsive);

    book.value = fetchBookReponsive.data
  } catch (error) {}
}

const handleSubmitEdit = () => {
  emit('handleEditBook', book.value)
}

onMounted(() => {
  fetchBook()
})
</script>
<style lang="scss" scoped></style>
