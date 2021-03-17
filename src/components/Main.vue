<template>
  <div>
<router-link to="/logout">Выйти?</router-link>
  <div class="vid">
<video controls width="400" height="300">
  <source src="https://firebasestorage.googleapis.com/v0/b/admin-2069e.appspot.com/o/5ttxlu9r4yy21fj1wvwp.mp4?alt=media&token=65045462-c77f-42bb-814d-9d8b781a67af">
</video>
  </div>
  </div>
</template>

<script>
import firebase from "firebase";


export default {
  name: 'Main',
   data: function(){
      return {
          videos: [{"src": "https://firebasestorage.googleapis.com/v0/b/admin-2069e.appspot.com/o/5ttxlu9r4yy21fj1wvwp.mp4?alt=media&token=65045462-c77f-42bb-814d-9d8b781a67af"}]
      }
      },
  components: {
  
  },
  created: function () {
    const user = window.localStorage.getItem('user') 
    console.log(user)
    if (user === null){
      this.$router.replace({ name: "Login" });
    }
     let db = firebase.firestore();
    db.collection('users').get(user).then((querySnapshot) => {
        querySnapshot.forEach((doc) => {
        console.log(this.mas, " => ", doc.data());
        let offset = doc.data().offset;
        db.collection('videos').orderBy('timestamp', 'desc').startAt(offset).get().then(docs => {
          docs.forEach((doc) => {
            console.log(this.mas, " => ", doc.data());
        })    
    });
})
    })
  }
  
}

</script>

<style scoped>
.vid {
  margin-right: auto;
  margin-left: auto;
  width: 500px;
}
</style>