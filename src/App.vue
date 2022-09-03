<script setup>
import { ref } from "vue"
import { useUserStore } from "./stores/users"

const user_store = useUserStore()

const user_input = ref({
  name: "",
  email: ""
})

const sort = ref(false)

const CreateUser = () => {
  if (!user_input.value.name || !user_input.value.email)
    return alert("Please enter both name and email")

  user_store.create(user_input.value)

  user_input.value = {
    name: "",
    email: ""
  }
}

const DeleteUser = (id) => {
  user_store.delete(id)
}
</script>

<template>
  <div>
    <main>
      <h1>Team manager</h1>
      <form @submit.prevent="CreateUser">
        <label for="name">
          Name:
          <input
            type="text"
            placeholder="e.g Naruto Uzumaki"
            v-model="user_input.name"
            id="name"
          /> </label
        ><br />
        <label for="email">
          Email:
          <input
            type="email"
            placeholder="e.g. hokage@ninja.com"
            v-model="user_input.email"
            id="email"
          />
        </label>
        <br />
        <input type="submit" value="Create User" />
      </form>

      <label for="sort">
        <span>Sort</span>
        <input type="checkbox" v-model="sort" id="sort" />
      </label>

      <div class="users" v-if="!sort">
        <div class="user" v-for="user in user_store.users">
          <span>ID: {{ user.id }}</span>
          <h3>{{ user.name }}</h3>
          <p>{{ user.email }}</p>
          <button @click="DeleteUser(user.id)" class="delete">Delete</button>
        </div>
      </div>

      <div class="users" v-else>
        <div class="user" v-for="user in user_store.usersByName">
          <span>ID: {{ user.id }}</span>
          <h3>{{ user.name }}</h3>
          <p>{{ user.email }}</p>
          <button @click="DeleteUser(user.id)" class="delete">Delete</button>
        </div>
      </div>
    </main>
  </div>
</template>

<style scoped>
ul {
  list-style: none;
}

table {
  margin-top: 40px;
}

.user {
  border: 1px solid hotpink;
  padding: 10px;
  margin-bottom: 40px;
}
.delete {
  background: red;
  border: none;
  border-radius: 10px;
}
</style>
