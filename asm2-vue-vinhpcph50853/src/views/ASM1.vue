<template>
  <div>
    <!-- Thêm  -->
    <h3>Thêm mới sinh viên</h3>
    <form @submit.prevent="handleSubmit">
      <div class="mb-3">
        <label for="id" class="form-label">ID</label>
        <input
          type="text"
          v-model="id"
          class="form-control"
          id="id"
          placeholder="Nhập ID"
        />
      </div>
      <div class="mb-3">
        <label for="name" class="form-label">Name</label>
        <input
          type="text"
          v-model="name"
          class="form-control"
          id="name"
          placeholder="Nhập name"
        />
      </div>
      <div class="mb-3">
        <label for="mssv" class="form-label">MSSV</label>
        <input
          type="text"
          v-model="mssv"
          class="form-control"
          id="mssv"
          placeholder="Nhập mssv"
        />
      </div>
      <div class="mb-3">
        <label for="className" class="form-label">ClassName</label>
        <select v-model="className" id="className">
          <option value="" disabled>Tên Lớp</option>
          <option value="1">K19</option>
          <option value="2">K20</option>
          <option value="3">K21</option>
          <option value="4">K22</option>
          <option value="5">K23</option>
        </select>
      </div>
      <button class="btn btn-primary">
        {{ isEditing ? 'Cập nhật' : 'Submit' }}
      </button>
    </form>

    <!-- Đổ danh sách -->
    <h1>Danh sách sinh viên</h1>
    <div class="mt-4" v-if="students.length == 0">
      <p>KO có sinh viên nào</p>
    </div>
    <ul class="list-group" v-else>
      <li
        v-for="student in students"
        :key="student.id"
        class="list-group-item d-flex justify-content-between align-items-center"
      >
        <div>
          <h5>Id: {{ student.id }}</h5>
          <h5>Name: {{ student.name }}</h5>
          <h5>MSSV: {{ student.mssv }}</h5>
          <h5>ClassName: {{ student.className }}</h5>
          <button class="btn btn-danger" @click="deleteStudents(student.id)">
            Xóa
          </button>
          <button class="btn btn-success" @click="editStudents(student)">
            Sửa
          </button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { reactive, ref } from 'vue'

const id = ref('')
const name = ref('')
const mssv = ref('')
const className = ref('')
const students = reactive([
  { id: 1, name: 'Vinh', mssv: '12345', className: 'K19' }, //mảng gốc
])

const classname = {
  1: 'K19',
  2: 'K20',
  3: 'K21',
  4: 'K22',
  5: 'K23',
}

const isEditing = ref(false)
const editingStudentId = ref(null)

//Xử lí sự kiện
const handleSubmit = () => {
  if (isEditing.value) {
    updateStudent()
  } else {
    addStudents()
  }
}

//Thêm
const addStudents = () => {
  if (!id.value || !name.value || !mssv.value || !className.value) {
    return
  }
  const newStudents = {
    id: id.value,
    name: name.value,
    mssv: mssv.value,
    className: classname[className.value],
  }
  students.push(newStudents)
  clearForm()
}
//Xóa form khi submit
const clearForm = () => {
  id.value = ''
  name.value = ''
  mssv.value = ''
  className.value = ''
}
//Xóa
const deleteStudents = studentId => {
  if (confirm('Bạn có muốn xóa ko?')) {
    const index = students.findIndex(student => student.id === studentId)
    if (index !== -1) {
      students.splice(index)
    }
  }
}
//sửa
const editStudents = student => {
  id.value = student.id
  name.value = student.name
  mssv.value = student.mssv
  className.value = student.className
  isEditing.value = true
  editingStudentId.value = student.id
}

const updateStudent = () => {
  const studentIndex = students.findIndex(
    student => student.id === editingStudentId.value,
  )
  if (studentIndex !== -1) {
    students[studentIndex] = {
      id: id.value,
      name: name.value,
      mssv: mssv.value,
      className: className.value,
    }
  }
  clearForm()
}
</script>

<style scoped></style>
