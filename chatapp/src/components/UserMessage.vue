<template>
    <div class="user-message">
        <form @submit.prevent="onAddMessage()">
            <label for="user-message">User Message</label>
            <input 
                type="text" 
                placeholder="Type Something" 
                name="user-message"
                v-model="userMessage">
                <p class="red-text" v-if="feedback">{{feedback}}</p>
        </form>
    </div>
</template>

<script>
import db from '@/utils/fb';
export default {
  name: "UserMessage",
  props: ["name"],
  data() {
    return {
      userMessage: null,
      feedback: null
    };
  },
  methods: {
    onAddMessage() {
      if (this.userMessage) {
        // console.log("inside the onAddMessage", this.userMessage,this.name,Date.now());
        //Reach out to firebase and add it to firestore
        db.collection('messages').add({
            content: this.userMessage,
            name: this.name,
            timestamp: Date.now()
        }).catch(err => console.log(err));
        this.userMessage = null;
        this.feedback = null;
      
      } else {
        this.feedback = "You must enter a message";
        setTimeout(() => {
          this.feedback = null;
        }, 2000);
      }
    }
  }
};
</script>
