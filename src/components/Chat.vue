<template>
  <div>
    <div class="row">
      <div class="layout-padding col-8" >
      <q-list highlight class="col-auto">
        <q-list-header>Messages</q-list-header>
        <q-item v-for="message in messages">
          <q-item-main>
            <q-item-tile label>{{message.text}}</q-item-tile>
            <q-item-tile label>{{message.id}}</q-item-tile>
            <q-item-tile label>{{messageDate(message)}}</q-item-tile>
            <q-item-tile label>{{isSent(message) ? true : false}}</q-item-tile>
          </q-item-main>
        </q-item>
      </q-list>
      </div>
      <q-list highlight class="col-auto">
        <q-list-header>People</q-list-header>
        <q-item v-for="user in users">
          <q-item-main>
            <q-item-tile label>{{user.email}}</q-item-tile>
          </q-item-main>
          <q-item-side right>
            <q-item-tile icon="chat_bubble" color="green" />
          </q-item-side>
        </q-item>
      </q-list>
    </div>
    <q-input class="row col-12 fixed-bottom"
      v-model="message"
      v-on:keyup.enter="send"
      type="textarea"
      float-label="Enter your message"
      :min-rows="1"
    />
  </div>
</template>

<script>
import {
  QInput,
  QList,
  QListHeader,
  QItem,
  QItemTile,
  QItemMain,
  QItemSide,
  QChatMessage
} from 'quasar'
import moment from 'moment'
import api from 'src/api'

export default {
  name: 'chat',
  components: {
    QInput,
    QList,
    QListHeader,
    QItem,
    QItemTile,
    QItemMain,
    QItemSide,
    QChatMessage
  },
  props: ['user'],
  data () {
    return {
      message: '',
      messages: [],
      users: []
    }
  },
  computed: {
  },
  methods: {
    isSent (message) {
      return (message.userId === this.userId)
    },
    messageDate (message) {
      return moment(message.createdAt).format('MMM Do, hh:mm:ss')
    },
    send () {
      if (this.$data.message) {
        api.service('messages').create({ text: this.$data.message }).then(() => {
          this.$data.message = ''
        })
      }
    }
  },
  mounted () {
    const messages = api.service('messages')
    const users = api.service('users')

    // Get all users and messages
    messages.find({
      query: {
        $sort: { createdAt: -1 },
        $limit: 25
      }
    })
      .then((response) => {
        // We want the latest messages but in the reversed order
        this.$data.messages = response.data.reverse()
      })
    users.find()
      .then((response) => {
        this.$data.users = response.data
      })

    // Add new messages to the message list
    messages.on('created', message => {
      console.log('message received')
      this.$data.messages.unshift(message)
    })
    // Add new users to the user list
    users.on('created', user => {
      console.log('user received')
      this.$data.users = this.$data.users.concat(user)
    })
  },
  beforeDestroy () {
  }
}
</script>

<style lang="styl">

</style>
