<template>
  <div class="card mt-3">
      <div class="card-body">
          <div class="card-title">
              <h3>Strategic Machines</h3>
              <hr>
          </div>
          <div class="card-body">
              <div class="messages" v-for="(msg, index) in messages" :key="index">
                  <p><span class="font-weight-bold">{{ msg.user }}: </span>{{ msg.message }}</p>
              </div>
          </div>
      </div>
      <div class="card-footer">
          <form @submit.prevent="sendMessage">
              <div class="gorm-group">
                  <label for="user">User:</label>
                  <input type="text" v-model="user" class="form-control">
              </div>
              <div class="gorm-group pb-3">
                  <label for="message">Message:</label>
                  <input type="text" v-model="message" class="form-control">
              </div>
               <div class="gorm-group pb-3">
                  <label for="action">Select an Action:</label>
                  <input type="text" v-model="action" class="form-control">
              </div>
              <button type="submit" class="btn btn-success">Send</button>
          </form>
      </div>
  </div>
</template>

<script>
import io from 'socket.io-client';

export default {
    data() {
        return {
            user: '',
            message: '',
            action: '',
            netId: 'rest001',
            messages: [],
            socket : io('localhost:3100', { query: "netId=rest001" })
        }
    },
    methods: {
        sendMessage(e) {
            e.preventDefault();
            
            this.socket.emit('MESSAGE', {
                user: this.user,
                netId: this.netId,
                message: this.message,
                action: this.action
            });
            this.message = ''
            this.action = ''
        }
    },
    mounted() {
        this.socket.on('RESPONSE', (data) => {
            this.messages = [...this.messages, data];
            console.log(data)
            // you can also do this.messages.push(data)
        });
    }
}
</script>

<style>

</style>
