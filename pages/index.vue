<template>
  <section class="container">
    <nuxt-link to="./users/10">About Page</nuxt-link>
    <div v-for="(item, index) in questions" :key="item.id" >
       <section v-if="step === index + 1">
        <h2 class="title">step{{ index + 1 }} </h2>
        <div class="content">
          <h1 class="questions"> {{ item.question }} </h1>
          <ul>
              <li class="person" v-for="(person, indexInner) in persons"
                :key="indexInner"
                :class="{ active: person.isClick }"
                :style="person.style"
                 @click.once="chooseAnswer(person.name, indexInner)" 
                 @mouseover="hoverBackgroundImg(indexInner)"
              >
              </li>
          </ul>
        </div>
      </section>
    </div>
    <div v-if="step === 4">
      <div class="result">
        <ul>
          <li class="correct">YES: {{ result.correct }}</li>
          <li class="wrong">NO: {{ result.wrong }}</li>
        </ul>
      </div>
      <button class="reset" @click="gameReset()">RESTART</button>
    </div>
    <div class="currentAnswer">Current Answer {{ currentAnswer.name }}<img alt="" :src="currentAnswer.img"></div>
  </section>
</template>

<script>
import AppLogo from '~/components/AppLogo.vue'
import mixin from '~/assets/mixin.js'

export default {
  mixins: [mixin],
  data: function () {
    return {
    step: 1,
    result: {
      'correct': 0,
      'wrong': 0
    },
    currentAnswer: {
      name: "LuLu Me",
      src: ''
    },
    questions: [
      {
        question: 'I was obliged to be industrious.',
        answer: 'Johannes Sebastian Bach',
        id: this.getUniqueID()
      },
      {
        question: 'Lose your dream, you lose your mind.',
        answer: 'Rolling Stones',
        id: this.getUniqueID()
      },
      {
        question: 'Trust is like a mirror, you can fix it if it\'s broken, but you can still see the crack in that mother fucker\'s reflection.',
        answer: 'Lady Gaga',
        id: this.getUniqueID()
      }
    ],
    persons: [{
        name: 'Johannes Sebastian Bach',
        profileImg: "http://www.bach-cantatas.com/thefaceofbach/Pic-FOB/his37t7det300.jpg",
        isClick: false,
        style: {
          background: ''
        }
      },
      {
        name: 'Rolling Stones',
        profileImg: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRnxTZoHKjCT6z0_aE9dV0Ycm400YmZYpTz0o3EAgT2rx47mMWdow",
        isClick: false,
        style: {
          background: ''
        }
      },
      {
        name: 'Lady Gaga',
        profileImg: "https://vignette.wikia.nocookie.net/ladygaga/images/a/a6/Lady_Gaga_Twitter_profile_2011.jpg/revision/latest?cb=20130116131720",
        isClick: false,
        style: {
          background: ''
        }
      }]
    } 
  },
  methods: {
    chooseAnswer (respondence, index) {

      let personId = index

      this.questions[this.step - 1].answer === respondence ? this.result.correct++ : this.result.wrong++
      this.persons[personId].isClick = true
      this.currentAnswer.img = this.persons[personId].profileImg
      this.currentAnswer.name = this.persons[personId].name

      setTimeout(()=>{
        this.step += 1
        this.persons[personId].isClick = false
      }, 500)

    },
    hoverBackgroundImg (index) {
      console.log(index)
      let personId = index
      this.persons[personId].style = {
        'background': `url("${this.persons[personId].profileImg}")`,
        'background-size': 'cover',
        'color': '#fff'
      }
      setTimeout(()=>{
        this.persons[personId].style = {
          'background': ''
        }
      }, 1000)
    },
    gameReset() {
      this.step = 1;
      this.result = { 'correct': 0, 'wrong': 0 }
      this.currentAnswer = {
        name: "LuLu Me",
        src: ''
      }
    }
  },
  components: {
  }
}
</script>

<style>
.container {
  margin: auto 0;
  text-align: center;
  margin: 100px;
}
.person {
  position: relative;
  width: 150px;
  height: 150px;
  display: inline-block;
  border: solid 2px gray;
  border-radius: 50%;
  text-align: center;
  margin: 10px;
  cursor: pointer;
}
.person > p {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.person:hover {
  border-style: dotted;
}

.active {
  border: solid 2px blue;
  color: white;
}
.questions {
  font-size: 50px;
}

</style>

