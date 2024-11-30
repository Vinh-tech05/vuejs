<template>
  <div>
    <button type="button" class="btn btn-primary"data-bs-toggle="modal"data-bs-target="#exampleModal">Create</button>
    <div class="modal fade"
      id="exampleModal"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <form @submit.prevent="handleCreate">
            <div class="modal-header">
              <h1 class="modal-title fs-5" id="exampleModalLabel">
                Create book
              </h1>
              <button
                type="button"
                class="btn-close"
                data-bs-dismiss="modal"
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
                  v-model="book.title" required
                />
              </div>
              <div class="form-group">
                <label for="author">Author</label>
                <input
                  type="text"
                  class="form-control"
                  id="author"
                  v-model="book.author"required
                />
              </div>
              <div class="form-group">
                <label for="year">Year</label>
                <input
                  type="number"
                  class="form-control"
                  id="year"
                  v-model="book.year"required
                />
              </div>
              <div class="form-group">
                <label for="cover">Cover</label>
                <input
                  type="url"
                  class="form-control"
                  id="cover"
                  v-model="book.cover"required
                />
              </div>
            </div>
            <div class="modal-footer">
              <button
                type="button"
                class="btn btn-secondary"
                data-bs-dismiss="modal"
              >
                Close
              </button>
              <button type="submit" class="btn btn-primary">
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
import { reactive } from 'vue'

const emit = defineEmits(['createBook'])

const book = reactive({
  title: '',
  author: '',
  year: '',
  cover: '',
})

const handleCreate = () => {
  emit('createBook', book)
}
</script>

<style lang="scss" scoped></style>
