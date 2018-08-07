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
        <transition name="fade" appear>
          <div class="alert alert-info" v-if="show2">This is some info With Show Data 2</div>
          <!-- <div class="alert alert-info" v-show="show">This is some info</div> -->
        </transition>

        <transition
          enter-class="" enter-active-class="animated bounce"
          leave-class="" leave-active-class="animated shake">
          <div class="alert alert-info" v-if="show">Animated Using Anime It!</div>
        </transition>

      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      show: false,
      show2: true,
      alertAnimation: 'fade'
    }
  }
}
</script>

<style>
  .fade-enter {
    opacity: 0;
  }
  .fade-enter-active {
    transition: opacity 2s;
  }
  .fade-leave {
    /* opacity: 1; */
  }
  .fade-leave-active {
    transition: opacity 2s;
    opacity: 0;
  }

  .slide-enter {
    opacity: 0;
    /* transform: translateY(20px); */
  }
  .slide-enter-active {
    animation: slide-in 3s linear forwards;
    transition: opacity 0.5s;
  }
  .slide-leave {

  }
  .slide-leave-active {
    animation: slide-out 3s linear forwards;
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
