<template>
<div class="form">
  <h1>Добавление пользователя</h1>

  <div
    class="close-btn"
    @click="isInvisible"
  ></div>

  <div class="block-inp">
    <p>Имя</p>
    <input
      type="text"
      v-model="user.name"
    >
  </div>

  <div class="block-inp">
    <p>Телефон</p>
    <input
      type="tel"
      v-model="user.tel"
    >
  </div>

  <div class="block-inp">
    <p>Начальник</p>
    <select v-model="user.selectUser">
      <option
        v-for="userSelect in users"
        :key="userSelect.index"
        :value="userSelect"
      >
        {{userSelect.name}}
      </option>
    </select>
  </div>

  <button class="save-btn" @click="addUser">Сохранить</button>

</div>
</template>

<script>
export default {
  name: 'Form',
  props: ['invisible', 'dataUser'],
  data () {
    return {
      users: [],
      user: {
        name: null,
        tel: null,
        selectUser: null,
        showTableList: false,
        id: ''

      }
    }
  },
  mounted () {
    if (localStorage.getItem('users')) {
      this.users = JSON.parse(localStorage.getItem('users'))
    }
  },
  methods: {
    // Закрытие формы
    isInvisible () {
      this.invisible({
        addItem: false
      })
    },
    // Добавление id пользователю
    addId () {
      if (this.users.length === 0) {
        return 0
      } else { return this.users.length }
    },
    // Добавление пользователя
    addUser () {
      if (!this.user.name || !this.user.tel) return
      this.user.id = this.addId()
      this.users.push(Object.assign({}, this.user))
      this.saveUsers()
      this.$emit('dataUser', this.users)
    },
    // Сохранение пользователя
    saveUsers () {
      let parsed = JSON.stringify(this.users)
      localStorage.setItem('users', parsed)
    }
  }
}
</script>

<style scoped>
.block-inp{
  display: flex;
}
h1{
  font-size: 20px;
  text-align: left;
}

p{
  width: 30%;
  text-align: left;
}

input{
  width: 70%;
  height: 20px;
  margin: auto;
  border-radius: 5px;

}
select{
  width: 70%;
  height: 29px;
  margin: auto;
  border-radius: 5px;
}
.form{
  padding: 20px;
  border: 1px solid aqua;
  text-align: left;
}
.save-btn{
  padding: 10px 70px 10px 10px;
  border-radius: 20px;
  font-size: 15px;
  margin: 22px 0;
}
.close-btn{
  position: relative;
  font-weight: bold;
}
.close-btn:after {
  position: absolute;
  top: -75px;
  bottom: 0;
  left: 0;
  right: 0;
  content: "\274C";
  font-size: 13px;
  color: black;
  line-height: 100px;
  text-align: right;
}

</style>
