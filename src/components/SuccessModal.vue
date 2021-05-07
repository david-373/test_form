<template>
  <transition name="fade">
    <div class="modal" v-if="show">
      <div class="backdrop" @click="closeModal()" />
      <div class="dialog">
        <div class="body">
          <transition name="bounce">
            <div class="success" v-if="succesShow"></div>
          </transition>
          <slot name="content" />
          <button @click="closeModal">OK</button>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: "SuccessModal",
  data() {
    return {
      show: false,
      succesShow: false,
    };
  },
  methods: {
    closeModal() {
      this.show = false;
      this.succesShow = false;
      this.$parent.resetData();
    },
    openModal() {
      this.show = true;
      setTimeout(() => {
        this.succesShow = true;
      });
    },
  },
};
</script>

<style lang="sass" scoped>
@import "../sass/variables.sass"
.bounce-enter-active
  animation: bounce-in 1s

.bounce-leave-active
  animation: bounce-in 0.5s reverse

@keyframes bounce-in
  0%
    transform: scale(0)

  50%
    transform: scale(1.1)

  100%
    transform: scale(1)

.modal
  position: fixed
  top: 0
  right: 0
  bottom: 0
  left: 0

  .backdrop
    background-color: rgba(0, 0, 0, 0.3)
    position: fixed
    top: 0
    right: 0
    bottom: 0
    left: 0
    z-index: 1

  .dialog
    background-color: #fff
    position: relative
    width: 350px
    margin: auto
    top: 30%
    border-radius: 10px
    z-index: 2
    .body
      padding: 10px
      border-radius: 10px
      text-align: center
      box-shadow: $shadow
      height: 250px
      .success
        width: 40%
        height: 50%
        background-image: url("../assets/success.png")
        background-size: contain
        background-repeat: no-repeat
        margin: 0 auto

      button
        width: 30%
        height: 35px
        border-radius: 10px
        border: none
        background-color: #3d97f4
        color: #fff
        transition: 0.2s
        cursor: pointer
        &:hover
          box-shadow: $shadow
</style>