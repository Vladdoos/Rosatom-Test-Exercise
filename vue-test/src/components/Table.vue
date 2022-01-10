<template>
<div class="table">
  <div class="container-table">
    <button @click="isVisible">Добавить</button>
    <div class="block-table">
      <table border="2" width="100%" cellpadding="4">
        <tr>
          <th class="table__headline-name"
            @click="sortActive = !sortActive">Имя</th>
          <th>Телефон</th>
        </tr>

        <tr
          v-for="dataUsers in (sortActive ? sortedUsers : users)"
          :key="dataUsers.id"
          v-if="hideUser(dataUsers.id) || userChilds.length === 0"
        >

          <td @click="showList(dataUsers.id)">
            <div class="plus alt"></div>
            <p class="table__name">{{dataUsers.name}}</p></td>
          <td><p>{{dataUsers.tel}}</p></td>
          <list-form
            v-if="childUser.id === dataUsers.id"
            :childUser = "userChilds"
            @hideUserChilds = 'hideUserChilds'
            @hideUserChildsLength = 'hideUserChildsLength'
            :sortActive = 'sortActive'
          />
        </tr>

      </table>
    </div>
  </div>

  <div class="container-form">
    <Form
      v-if = 'addItem'
      :invisible = 'invisible'
      @dataUser = 'dataUser'
    />
  </div>
</div>

</template>

<script>
import Form from './Form'
import ListForm from './listTable'
export default {
  name: 'Table',
  components: {
    ListForm,
    Form
  },
  data () {
    return {
      addItem: false,
      users: [],
      childUser: [],
      userChilds: [],
      hideUsers: [],
      hideUsersLength: [],
      sortActive: false
    }
  },
  mounted () {
    if (localStorage.getItem('users')) {
      this.users = JSON.parse(localStorage.getItem('users'))
    }
  },
  computed: {
    sortedUsers() {
      return [...this.users].sort((a, b) => a.name.localeCompare(b.name));
    },
  },
  methods: {
    // Показать форму
    isVisible () {
      this.addItem = true
    },
    // Закрыть форму
    invisible () {
      this.addItem = ''
    },
    // Получение новых данных о всех пользователях из дочернего компонента
    dataUser (data) {
      this.users = data
      this.addItem = false
    },
    // Получение данных о пользователях которых надо скрыть
    hideUserChilds (data) {
      this.hideUsers = data
    },
    // Получение количества пользователей которых надо скрыть
    hideUserChildsLength (data) {
      this.hideUsersLength = data
    },
    // Скрытие пользователей
    hideUser (id) {
      for (let j = 0; this.userChilds.length > j; j++) {
        if (this.userChilds[j].id === id) {
          return false
        } else { this.show = true }
      }
      for (let i = 0; this.hideUsersLength > i; i++) {
        if (this.hideUsers[i].id === id) {
          return false
        } else {
          this.show = true
        }
      }
      return this.show
    },
    // Показать подчиненного
    showList (id) {
      this.childUser = this.users[id]
      this.userChilds = this.users.filter(function (user) {
        if (user.selectUser === null || user.selectUser === undefined) {
          return false
        } else {
          return user.selectUser.id === id
        }
      })
      this.hideUser()
      this.childUser.showTableList = true

    }
  }
}
</script>

<style scoped>
table {
  border: none;
  display: grid;
  grid-template-columns: 100%;
}
button{
  padding: 10px 50px;
  border-radius: 20px;
  font-size: 15px;
  margin: 22px 0;
  cursor: pointer;
}
tr{
  display: grid;
  padding: 0;
  flex-wrap: wrap;
  grid-template-columns: 475px 475px;
}
td,
th{
  list-style: none;
  padding: 10px;
  text-align: left;
  border: 1px solid black;
  position: relative;
}
tr:hover .table__name{
  padding-left: 35px;
}
.table{
  text-align: right;
  margin: auto;
  display: flex;
  padding: 40px;
  flex-wrap: wrap;
}
.container-table{
  width: 60%;
  margin-right: 40px;
  overflow-x : auto;
}
.container-form{
  width: 30%;
}
.table__name,
.table__headline-name{
cursor: pointer;
}
td:hover .plus {
  display:inline-block;
  width:30px;
  height:30px;
  position: absolute;
  left: 7px;
  top: 22px;
  background:
    linear-gradient(#fff,#fff),
    linear-gradient(#fff,#fff),
    #000;
  background-position:center;
  background-size: 50% 2px,2px 50%; /*thickness = 2px, length = 50% (25px)*/
  background-repeat:no-repeat;
}

td:hover .alt {
  background:
    linear-gradient(#000,#000),
    linear-gradient(#000,#000);
  background-position:center;
  background-size: 50% 2px,2px 50%; /*thickness = 2px, length = 50% (25px)*/
  background-repeat:no-repeat;
}
</style>
