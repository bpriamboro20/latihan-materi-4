<template>
  <div class="container">
    <h1>To Do List</h1>
    <ol class="orderlist">
      <li v-for="(activity, index) in todoList" :key="index" class="listitems">
        <span v-if="notEditing(index)">{{ activity }}</span>
        <input v-else v-model="editList" @keyup.enter="selesaiEdit" />
        <button v-if="notEditing(index)" @click="hapus(index)">Hapus</button>
        <button v-if="notEditing(index)" @click="edit(index)">Edit</button>
        <button v-else @click="selesaiEdit">Selesai</button>
      </li>
    </ol>
    <form @submit.prevent="tambahkan">
      <input type="text" v-model="list" placeholder="Tambah aktivitas baru" />
      <button type="submit">Tambahkan</button>
    </form>
    <p v-if="isMoreFour">{{ isMoreFour }}</p>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      todoList: [],
      list: "", // Input untuk menambahkan item baru
      editList: "", // Input untuk mengedit item
      editIndex: null, // Indeks item yang sedang diedit
    };
  },
  methods: {
    tambahkan() {
      if (this.list.trim() === "") {
        alert("Inputan harus diisi dan tidak boleh kosong ya ");
      } else {
        if (this.editIndex !== null) {
          // Jika sedang dalam mode edit
          this.todoList = this.todoList.map((item, index) => (index === this.editIndex ? this.editList : item));
          this.editIndex = null; // Reset editIndex setelah edit selesai
          this.editList = ""; // Kosongkan input edit setelah selesai
        } else {
          // Jika tidak dalam mode edit
          this.todoList.push(this.list);
          this.list = ""; // Kosongkan input setelah item ditambahkan
        }
      }
    },
    hapus(index) {
      this.todoList = this.todoList.filter((item, idx) => idx !== index);
    },
    edit(index) {
      this.editList = this.todoList[index];
      this.editIndex = index;
    },
    selesaiEdit() {
      if (this.editList.trim() === "") {
        alert("Inputan harus diisi dan tidak boleh kosong ya ");
      } else {
        this.todoList[this.editIndex] = this.editList;
        this.editIndex = null;
        this.editList = "";
      }
    },
    notEditing(index) {
      return this.editIndex !== index;
    },
  },
  computed: {
    isMoreFour() {
      return this.todoList.length >= 4 ? "Hebat" : "";
    },
  },
};
</script>

<style scoped>
.container {
  width: 100%;
}

.container form input {
  width: 80%;
  margin-right: 20px;
}

.orderlist {
  width: 80%;
}

.listitems {
  width: 80%;
  padding: 7px 0;
}

.listitems span {
  width: 60%;
  display: inline-block;
}

ol li button {
  margin: 0px 50px;
}
</style>
