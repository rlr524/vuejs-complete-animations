<template>
  <div id="app" class="container">
    <div class="row">
      <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
        <h1>Animations</h1>
        <hr />
        <div>
          <select v-model="alertAnimation" class="form-control mt-4 mb-4">
            <option value="fade">Fade</option>
            <option value="slide">Slide</option>
          </select>
        </div>
        <button
          class="btn mb-4"
          :class="[show ? greenClass : redClass]"
          @click="toggleShow"
        >
          Show Alert
        </button>
        <!-- We use v-show here vs v-if because we're guessing that this may get toggled a lot
        and v-show has a lower toggle cost, though a higher initial render cost because v-show 
        is simply css display toggle (so loads the element no matter what) vs v-if which is true 
        conditional rendering (only loads if conditional is met). We can use animations with either.  -->
        <transition :name="alertAnimation">
          <div class="alert alert-info" v-show="show">
            This is Some Info and it Changes Based on the Selection
          </div>
        </transition>
        <!-- We can tell Vue which animation type to use from our CSS, animation or transition, using a type property -->
        <transition name="slide" type="animation">
          <div class="alert alert-info" v-show="show">This is a Slide</div>
        </transition>
        <transition
          enter-active-class="animated bounce"
          leave-active-class="animated shake"
        >
          <div class="alert alert-info" v-if="show">
            This is some info that Animates on Load
          </div>
        </transition>
        <!-- The mode has two choices, out-in lets the old element animate out first and in-out is the reverse -->
        <transition :name="alertAnimation" mode="out-in">
          <div class="alert alert-info" v-if="show" key="info">
            This is also some info
          </div>
          <div class="alert alert-warning" v-if="!show" key="warning">
            This is some warning
          </div>
        </transition>
        <hr />
        <hr />
        <button class="btn btn-primary" @click="load = !load">
          Load / Remove Element (Using Js)
        </button>
        <!-- :css="false" tells Vue to not look for css classes, we're not using them in this case -->
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
          <div
            class="mt-4"
            style="width: 300px; height: 100px; background-color: lightgreen"
            v-if="load"
          ></div>
        </transition>
        <hr />
        <hr />
        <button class="btn btn-primary mb-4" @click="changeAlert">
          Change Alert
        </button>
        <transition name="fade" mode="out-in">
          <component :is="selectedComponent"></component>
        </transition>
      </div>
    </div>
  </div>
</template>

<script>
import Danger from "@/components/Danger.vue";
import Success from "@/components/Success.vue";

export default {
  name: "App",
  components: {
    appDanger: Danger,
    appSuccess: Success,
  },
  data() {
    return {
      show: false,
      load: false,
      redClass: "button-red",
      greenClass: "button-green",
      alertAnimation: "fade",
      elementWidth: 100,
      selectedComponent: "app-danger",
    };
  },
  methods: {
    changeAlert() {
      this.selectedComponent === "app-danger"
        ? (this.selectedComponent = "app-success")
        : (this.selectedComponent = "app-danger");
    },
    toggleShow() {
      this.show = !this.show;
    },
    beforeEnter(el) {
      console.log("beforeEnter");
      this.elementWidth = 100;
      el.style.width = this.elementWidth + "px";
    },
    enter(el, done) {
      console.log("enter");
      let round = 1;
      const interval = setInterval(() => {
        el.style.width = this.elementWidth + round * 10 + "px";
        round++;
        if (round > 20) {
          clearInterval(interval);
          done();
        }
      }, 20);
    },
    afterEnter() {
      console.log("afterEnter");
    },
    enterCancelled() {
      console.log("enterCancelled");
    },
    beforeLeave(el) {
      console.log("beforeLeave");
      this.elementWidth = 300;
      el.style.width = this.elementWidth + "px";
    },
    leave(el, done) {
      console.log("leave");
      let round = 1;
      const interval = setInterval(() => {
        el.style.width = this.elementWidth - round * 10 + "px";
        round++;
        if (round > 20) {
          clearInterval(interval);
          done();
        }
      }, 20);
    },
    afterLeave() {
      console.log("afterLeave");
    },
    leaveCancelled() {
      console.log("leaveCancelled");
    },
  },
};
</script>

<style>
/* enter is attached for only one frame at the beginning of the animation; typically we use this to just set an opacity of zero for one frame */
.fade-enter {
  opacity: 0;
}

.fade-enter-active {
  transition: opacity 1s;
}

/* .alert-fade-leave {
} */

.fade-leave-active {
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
  animation: slide-out 1s ease-out forwards;
  transition: opacity 1s;
  opacity: 0;
}

.fade-enter {
  opacity: 0;
}

.fade-enter-active {
  transition: opacity 5s;
}

.fade-leave-active {
  transition: opacity 1s;
  opacity: 0;
}

.button-red {
  background-color: red;
}

.button-green {
  background-color: green;
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
