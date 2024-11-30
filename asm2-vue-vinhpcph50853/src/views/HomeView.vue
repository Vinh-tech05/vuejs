<template>
  <div>
    <h2 style="text-align: center;" >BOOK LIST</h2>
    <bookAdd @createBook="createBook" />
    <table class="table table-striped">
      <thead>
        <tr>
          <th scope="col">id</th>
          <th scope="col">Title</th>
          <th scope="col">Author</th>
          <th scope="col">Year</th>
          <th scope="col">Cover</th>
          <th scope="col"></th>
          <th scope="col"></th>
          <th scope="col"></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="book in books" :key="book.id">
          <td>{{ book.id }}</td>
          <td>{{ book.title }}</td>
          <td>{{ book.author }}</td>
          <td>{{ book.year }}</td>
          <td>
            <img :src="book.cover" alt="Cover" style="width: 100px; height: auto;" />
          </td>
          <td><button class="btn btn-warning" @click="viewBookDetail(book)">Xem chi tiết</button></td>
          <td>
            <button class="btn btn-danger" @click="handleDelete(book.id)">
              Xóa
            </button>
          </td>
          <td>
            <button class="btn btn-info" @click="handleEdit(book.id)">
              Sửa
            </button>
          </td>
        </tr>
      </tbody>
    </table>

    <!-- chi tiết -->
    <bookDetails
    v-if="isShowDetailModal"
    :book="selectedBook"
    @closeDetailModal="closeDetailModal"
    />

    <!-- sửa -->
    <bookEdit
      v-if="isShowEditModal"
      :isShowEditModal="isShowEditModal"
      :handleCloseEditModal="handleCloseEditModal"
      :editIdselected="editIdselected"
      @handleEditBook="handleEditBook"
    />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import instanceAxios from '@/utils/configAxios'
import bookAdd from '@/components/bookAdd.vue'
import bookEdit from '@/components/bookEdit.vue'
import bookDetails from '@/components/bookDetails.vue'

const books = ref()

//show danh sách
const fetchBooks = async () => {
  try {
    const { data } = await instanceAxios.get('books')
    // console.log(data)

    books.value = data
  } catch (error) {}
}

//xem chi tiết
const isShowDetailModal =ref(false)
const selectedBook = ref(null)

const viewBookDetail = (book) =>{
  selectedBook.value= book
  isShowDetailModal.value = true
}

//đóng modal chi tiết sách
const closeDetailModal = () =>{
  isShowDetailModal.value = false
  selectedBook.value = null
}

//xóa
const handleDelete = async (id) => {
  try {
    const isDelete = confirm('Có chắc muốn xóa ko???')
    if (!isDelete) return
    //call api xóa trên db
    const deleteBooks = await instanceAxios.delete(`books/${id}`)
    //gọi hàm filter lọc ra 1 mảng mớiko chứa í muốn xóa
    books.value = books.value.filter(book => book.id != id)
  } catch (error) {
    console.log('ko xóa thành công')
  }
}

//thêm
const createBook = async (book) => {
  const createBookReponse = await instanceAxios.post(`books`, book)
  fetchBooks()
}

//sửa

const isShowEditModal = ref(false)

const editIdselected = ref('')

//lấy id và bật form sửa
const handleEdit = (id) => {
  editIdselected.value = id
  // console.log(editIdselected)

  isShowEditModal.value = true
}

//tắt form sửa
const handleCloseEditModal = () => {
  isShowEditModal.value = false
}

const handleEditBook = async(book) => {
  try {
    const editBookReponsive = await instanceAxios.put(`books/${editIdselected.value}`,book)
    // console.log(editBookReponsive);

    fetchBooks()
  } catch (error) {}
}

onMounted(() => {
  fetchBooks()
})
</script>

<style lang="scss" scoped></style>
