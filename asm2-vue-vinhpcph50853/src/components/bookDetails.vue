<template>
  <div>
    <div class="modal-backdrop fade show"></div>
    <div
      class="modal fade show"
      style="display: block"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">Chi tiết sách</h5>
            <button
              type="button"
              class="btn-close"
              @click="$emit('closeDetailModal')"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">
            <p><strong>ID:</strong> {{ book.id }}</p>
            <p><strong>Title:</strong> {{ book.title }}</p>
            <p><strong>Author:</strong> {{ book.author }}</p>
            <p><strong>Year:</strong> {{ book.year }}</p>
            <p>
              <strong>Cover:</strong>
              <img :src="book.cover" alt="Cover" style="width: 100px; height: auto;" />
            </p>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" @click="$emit('closeDetailModal')">
              Close
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps(['book'])
</script>

<style lang="scss" scoped></style>
