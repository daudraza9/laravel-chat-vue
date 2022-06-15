

<template>
    <AppLayout title="Dashboard">
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                Chat
            </h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-xl sm:rounded-lg">
                   <messages-container/>
                   <input-container :room="currentRoom"></input-container>
                </div>
            </div>
        </div>
    </AppLayout>
</template>

<script>
import AppLayout from '@/Layouts/AppLayout.vue';
import MessagesContainer from "./messages-container";
import InputContainer from "./input-message";


export default {
    components :{
        MessagesContainer,
        InputContainer,
        AppLayout,
    },
    data: function(){
      return {
          chatRooms:[],
          currentRoom:[],
          messages:[]
      }
    },
    method:{
        getRooms()
        {
            axios.get('/chat/rooms')
            .then(response =>{
                this.chatRooms = response.data;
                this.setRoom(response.data[0]);
            })
            .catch(error =>{
                console.log(error);
            })
        },
        setRoom(room){
            this.currentRoom = room;
            this.getMessages();
        },
        getMessages()
        {
            axios.get('/chat/room/'+this.currentRoom.id+'/messages')
            .then(response=>{
                this.messages = response.data;
            })
            .catch(error =>{
                console.log(error);
            })
        }
    },
    create(){
        this.getRooms();
    }
}
</script>
