<template>
  <div class="container">
    <div class="row">
      <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
        <h1>Animations</h1>
        <hr>
        <select v-model="alertAnimation" class="form-control">
          <option value="fade">Fade</option>
          <option value="slide">Slide</option>
        </select>
        <br>
        <button class="btn btn-primary" @click="show = !show">Show Alert</button>
        <button class="btn btn-warning" @click="show2 = !show2">Show Alert 2</button>
        <br><br>
        <transition :name="alertAnimation">
          <!--Can Use V-If and Can Use V-Show  -->
          <!-- <div class="alert alert-info" v-if="show">This is some info</div> -->
          <div class="alert alert-info" v-show="show">This is some info</div>
        </transition>

        <!-- Use Appear For On Load Page Start Animate -->
        <transition :name="alertAnimation" appear>
          <div class="alert alert-info" v-if="show2">This is some info With Show Data 2</div>
          <!-- <div class="alert alert-info" v-show="show">This is some info</div> -->
        </transition>

        <transition
          enter-active-class="animated bounce"
          leave-active-class="animated shake">
          <div class="alert alert-info" v-if="show">Animated Using Anime It!</div>
        </transition>
        <br>
        <hr>
        <button class="btn btn-primary" @click="load = !load">Load / Remove Element</button>
        <br><br>
        <transition
          @before-enter="beforeEnter"
          @enter="enter"
          @after-enter="afterEnter"
          @enter-cancelled="enterCancelled"

          @before-leave="beforeLeave"
          @leave="leave"
          @after-leave="afterLeave"
          @leave-cancelled="leaveCancelled"
          :css="false"
        >
          <div style="width:300px; height: 100px; background-color: lightgreen;" v-if="load">

          </div>
        </transition>
        <hr>
        <button
        class="btn btn-primary" @click="selectedComponent == 'appSuccess' ? selectedComponent = 'appDanger' : selectedComponent = 'appSuccess'"
        > Toogle Button
        </button>
        <br><br>
        <transition name="fade" mode="out-in">
          <component :is="selectedComponent"></component>
        </transition>
        <hr>
        <button class="btn btn-success" @click="addItem">Add Item</button>
        <br><br>
        <ul class="list-group">
          <li class="list-group-item" v-for="(number, index) in numbers" @click="removeItem(index)" style="cursor: crosshair;">{{ number }}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import DangerAlert from './components/DangerAlert.vue'
import SuccessAlert from './components/SuccessAlert.vue'

export default {

  data() {
    return {
      show: false,
      show2: true,
      load: false,
      alertAnimation: 'fade',
      elementWidth: 100,
      selectedComponent: 'appSuccess',
      numbers: [1,2,3,4,5]
    }
  },
  methods: {
    beforeEnter(el) {
      console.log('beforeEnter');
      this.elementWidth = 100
      el.style.width = this.elementWidth + 'px'
    },
    enter(el, done) {
       console.log('Enter')
       let round = 1
       const interval = setInterval(() => {
         el.style.width = (this.elementWidth + round * 10) + 'px'
         round++
         if (round > 20) {
           clearInterval(interval)
           done()
         }
       }, 20)
    },
    afterEnter(el) {
      console.log('afterEnter');
    },
    enterCancelled(el) {
      console.log('enterCancelled');
    },
    beforeLeave(el) {
      console.log('beforeLeave');
      this.elementWidth = 300
      el.style.width = this.elementWidth + 'px'
    },
    leave(el, done) {
      console.log('Leave')
      let round = 1
      const interval = setInterval(() => {
        el.style.width = (this.elementWidth - round * 10) + 'px'
        round++
        if (round > 20) {
          clearInterval(interval)
          done()
        }
      }, 20)
    },
    afterLeave(el) {
      console.log('after leave');
    },
    leaveCancelled(el) {
      console.log('leave cancelled');
    },
    removeItem(index) {
      this.numbers.splice(index, 1)
    },
    addItem() {
      const num = Math.floor(Math.random() * this.numbers.length)
      this.numbers.splice(num, 0, this.numbers.length + 1)
    }
  },
  components: {
    appDanger: DangerAlert,
    appSuccess: SuccessAlert
  }
}
</script>

<style>
  .fade-enter {
    opacity: 0;
  }
  .fade-enter-active {
    transition: opacity 1s;
  }
  .fade-leave {
    /* opacity: 1; */
  }
  .fade-leave-active {
    transition: opacity 1s;
    opacity: 0;
  }

  .slide-enter {
    opacity: 0;
    /* transform: translateY(20px); */
  }
  .slide-enter-active {
    animation: slide-in 1s linear forwards;
    transition: opacity 0.5s;
  }
  .slide-leave {

  }
  .slide-leave-active {
    animation: slide-out 1s linear forwards;
    transition: opacity 1;
    opacity: 0;
  }

  @@keyframes slide-in {
    from {
      transform: translateY(50px);
    }
    to {
      transform: translateY(0px);
    }
  }

  @@keyframes slide-out {
    from {
      transform: translateY(0);
    }
    to {
      transform: translateY(50px);
    }
  }
</style>
