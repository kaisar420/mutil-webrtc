<template>
    <div class="container">
        <div class="row">
            <div class="col-md-6 col-md-offset-3">
                <h2>Welcome to WebRTC Video Demo</h2><br/>
                <form class="form" action="" @submit.prevent="submit()">
                    <input class="form-control" type="text" v-model="room_name" placeholder="请输入房间名"><br/>
                    <button class="btn btn-primary btn-block" type="submit">创建房间</button>
                </form>
            </div>
        </div>
        <br/>
        <div class="row" style="padding-top: 30px">
            <div class="col-md-6 col-md-offset-3">
                <h4 style="text-align: left">Current Active Rooms: </h4>
                <table class="table table-bordered">
                    <thead>
                    <tr>
                        <th style="text-align: center">房间号</th>
                        <th style="text-align: center">用户</th>
                    </tr>
                    </thead>
                    <tbody>
                    <tr v-for="(room, index) in rooms">
                        <td>
                            <router-link :to="{name: 'room', params: {room: index}}">
                                {{ index }}
                            </router-link>
                        </td>
                        <td>{{ room.join(', ') }}</td>
                    </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>

<script>
const socket = io.connect('http://127.0.0.1:3001');

export default {
  data() {
    return {
      room_name: '',
      rooms: '',
    };
  },
  created() {
    socket.emit(
      'message',
      JSON.stringify({
        event: 'get_room_info',
      })
    );
  },
  mounted() {
    socket.on(
      'message',
      function(data) {
        var data = JSON.parse(data);
        console.log(data);
        switch (data.event) {
          case 'show':
            this.rooms = data.allUser;
            break;
          default:
            break;
        }
      }.bind(this)
    );
  },
  methods: {
    submit() {
      this.$router.push({ name: 'room', params: { room: this.room_name } });
    },
  },
};
</script>

<style>
</style>