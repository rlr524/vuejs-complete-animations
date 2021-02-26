<template>
  <div id="app" class="container">
    <div class="row">
      <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
        <h1>Animations</h1>
        <hr />
        <button class="btn btn-primary mb-4" @click="toggleShow">
          Show Alert
        </button>
        <!-- We use v-show here vs v-if because we're guessing that this may get toggled a lot
        and v-show has a lower toggle cost, though a higher initial render cost because v-show 
        is simply css display toggle (so loads the element no matter what) vs v-if which is true 
        conditional rendering (only loads if conditional is met). We can use animations with either.  -->
        <transition name="alert-fade">
          <div class="alert alert-info" v-show="show">This is some Info</div>
        </transition>
        <!-- We can tell Vue which animation type to use from our CSS, animation or transition, using a type property -->
        <transition name="slide" type="animation">
          <div class="alert alert-info" v-show="show">This is a Slide</div>
        </transition>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      show: true,
    };
  },
  methods: {
    toggleShow() {
      this.show = !this.show;
    },
  },
  components: {},
};
</script>

<style>
/* enter is attached for only one frame at the beginning of the animation; typically we use this to just set an opacity of zero for one frame */
.alert-fade-enter {
  opacity: 0;
}

.alert-fade-enter-active {
  transition: opacity 1s;
}

/* .alert-fade-leave {
} */

.alert-fade-leave-active {
  transition: opacity 1s;
  opacity: 0;
}

/* .slide-enter {
  
} */

.slide-enter-active {
  animation: slide-in 1s ease-out forwards;
  transition: opacity 0.5s;
}

/* .slide-leave {
  
} */

.slide-leave-active {
  animation: slide-out 3s ease-out forwards;
  transition: opacity 1s;
  opacity: 0;
}

@keyframes slide-in {
  from {
    transform: translateY(20px);
  }
  to {
    transform: translateY(0);
  }
}

@keyframes slide-out {
  from {
    transform: translateY(0);
  }
  to {
    transform: translateY(20px);
  }
}
</style>
