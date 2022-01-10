<template>
  <div class="container-listForm">
    <tr
      v-for="dataUsers in (sortActive ? sortedUsers : childUser)"
      :key="dataUsers.id"
    >
      <td @click="showList(dataUsers.id)">
        <div class="plus alt"></div>
        <p class="list__name-p">{{dataUsers.name}}</p></td>
      <td><p>{{dataUsers.tel}}</p></td>
      <list-form
        v-if="childUsers.showTableList && childUsers.id === dataUsers.id"
        :childUser = "userChilds"
        :style="'margin-left: 40px;'"
        :sortActive = 'sortActive'
        @hideUserChilds = 'hideUserChilds'
        @hideUserChildsLength = 'hideUserChildsLength'
      />
    </tr>
  </div>
</template>

<script>
import listForm from './listTable'
export default {
  name: 'listForm',
  props: {
    childUser: [],
    sortActive: ''

  },
  comments: {
    listForm
  },

  data () {
    return {
      childUsers: [],
      userChilds: [],
      users: [],
      usersChildId: [],
      hideUsersLength: []
    }
  },
  mounted () {
    if (localStorage.getItem('users')) {
      this.users = JSON.parse(localStorage.getItem('users'))
    }
  },
  computed: {
    sortedUsers () {
      return [...this.childUser].sort((a, b) => a.name.localeCompare(b.name))
    }
  },
  methods: {
    // Получение данных о пользователях из рекурсии и передача в родительский компонент
    hideUserChilds (data) {
      this.userChildsData = data
      this.$emit('hideUserChilds', this.userChilds.concat(this.userChildsData))
    },
    // Получение данных о количестве пользователей из рекурсии и передача в родительский компонент
    hideUserChildsLength (data) {
      this.userChildsLength = data
      this.$emit('hideUserChildsLength', this.userChilds.length + this.userChildsLength)
    },
    showList (id) {
      this.childUsers = this.users[id]
      this.userChilds = this.users.filter(function (user) {
        if (user.selectUser === null || user.selectUser === undefined) {
          return false
        } else {
          return user.selectUser.id === id
        }
      })

      this.$emit('hideUserChilds', this.userChilds)
      this.$emit('hideUserChildsLength', this.userChilds.length)
      this.childUsers.showTableList = true
    }
  }

}
</script>

<style scoped>
tr{
  display: grid;
  grid-template-columns: 100% 100%;
}
td{
  list-style: none;
  padding: 10px;
  text-align: left;
  border: 1px solid black;
}
.container-listForm{
  width: 100%;
}
.list__name-p{
  cursor: pointer;
}
td:first-child{
  margin-left: 24px;
  position: relative;
}
tr:hover .list__name-p{
  padding-left: 35px;
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
