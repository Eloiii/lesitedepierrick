<script setup>
import {ref} from 'vue'
import {useCollection, useFirestore} from 'vuefire'
import {addDoc, collection} from 'firebase/firestore'

const db = useFirestore()
let messages = useCollection(collection(db, 'messages'))

function moveTete() {
  const tete = document.querySelector('.moving-tete')
  // console.log(tete.style)
  tete.style.top = parseInt(tete.style.top, 10) + 3 + "px"
  tete.style.left = parseInt(tete.style.left, 10) + 3 + "px"
}

setInterval(moveTete, 5)

function getMessages() {
  return messages.value.sort(
      (objA, objB) => Number(objA.date) - Number(objB.date),
  );
}


let input = ref("")

async function validate() {
  const docRef = await addDoc(collection(db, 'messages'), {
    date: new Date(),
    content: input.value
  });
  input.value = ""
  // messages.value = useCollection(collection(db, 'messages'))
  // console.log(input.value)
}

</script>

<template>
  <img class="moving-tete" style="top: 1px; left: 1px" src="../assets/tete2.jpg"/>
  <section class="images">
    <div class="tete1 container">
      <img class="tete" src="../assets/tete.jpeg"/>

    </div>
    <div class="tete2 container">
      <img class="tete" src="../assets/tete.jpeg"/>

    </div>
    <div class="tete3 container">
      <img class="tete" src="../assets/tete.jpeg"/>

    </div>
    <div class="tete4 container">
      <img class="tete" src="../assets/tete.jpeg"/>
    </div>
  </section>
  <section class="texte">
    <div>
      <a href="/" class="title">Pierricklamerde.fr !!</a>
    </div>
    <div class="messages">
      <div class="messages-container">
        <span v-for="message in getMessages()" :key="message.id" v-if="message !== null" class="content">
          <span class="date" v-if="message.date">
            Le {{
              message.date.toDate().toLocaleDateString('fr-FR',
                  {
                    weekday: "long",
                    year: "numeric",
                    month: "short",
                    day: "numeric",
                    hour: "2-digit",
                    minute: "2-digit"
                  })
            }}
           :
          </span>
          {{ message.content }}
        </span>
      </div>
      <div class="text-input">
        <input id="text" name="text" placeholder="encore une dinguerie ?" required type="text" v-model="input"
               @keydown.enter="validate">
      </div>
    </div>
  </section>
</template>

<style>
html {
  min-height: 100%;
  background: linear-gradient(rgba(255, 0, 0, 1) 0%, rgba(255, 154, 0, 1) 10%, rgba(208, 222, 33, 1) 20%, rgba(79, 220, 74, 1) 30%, rgba(63, 218, 216, 1) 40%, rgba(47, 201, 226, 1) 50%, rgba(28, 127, 238, 1) 60%, rgba(95, 21, 242, 1) 70%, rgba(186, 12, 248, 1) 80%, rgba(251, 7, 217, 1) 90%, rgba(255, 0, 0, 1) 100%) 0 0/100% 200%;
  animation: a 2s linear infinite;
}

@keyframes a {
  to {
    background-position: 0 -200%
  }
}

.title {
  font-size: 4vmax;
  font-family: "Impact", Serif;
  font-style: italic;
  font-weight: bolder;
  text-decoration: underline;
}

.images {
  z-index: 1;
  position: absolute;
  top: 0;
  left: 0;
  height: 100vh;
  width: 100vw;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: repeat(2, 1fr);
  grid-column-gap: 0px;
  grid-row-gap: 0px;
}

.texte {
  z-index: 99;
  position: absolute;
  top: 0;
  left: 0;
  height: 100vh;
  width: 100vw;
  display: flex;
  align-items: center;
  flex-flow: column wrap;
}

.messages {
  margin-top: 10vh;
}

.messages-container {
  width: 60vw;
  height: 60vh;
  border: dot-dash black;
  background: rgba(255, 255, 0, .4);
  padding: 15px 15px;
  flex: 1;
  display: flex;
  flex-direction: column-reverse;
  overflow: auto;
  min-height: 0;
}


.text-input input {
  width: 60vw;
}

.date {
  color: lightgray;
}

.tete1 {
  grid-area: 1 / 1 / 2 / 2;
  display: flex;
  justify-content: start;
  align-items: start;
}

.tete2 {
  grid-area: 1 / 2 / 2 / 3;
  display: flex;
  justify-content: end;
  align-items: start;
}

.tete3 {
  grid-area: 2 / 1 / 3 / 2;
  display: flex;
  justify-content: start;
  align-items: end;
}

.tete4 {
  grid-area: 2 / 2 / 3 / 3;
  display: flex;
  justify-content: end;
  align-items: end;
}

.tete {
  width: 15vw;
  height: 20vh;
  border-radius: 50%;
  animation: rotation 2s infinite linear;
}

.moving-tete {
  overflow: hidden;
  position: absolute;
  z-index: 2;
  width: 15vw;
  height: 20vh;
  border-radius: 25%;
  animation: rotation 8s infinite linear;
}


@keyframes rotation {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(359deg);
  }
}
</style>
