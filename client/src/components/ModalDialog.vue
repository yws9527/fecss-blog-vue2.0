<template>
  <div class="modal-container">
    <div class="modal-mask" v-if="maskShow">
    </div>
    <transition name="slideIn"
      v-if="modalShow">
      <div class="modal-content">
        <h3 class="modal-title">{{modal.title}}</h3>
        <p class="modal-txt">
          {{modal.content}}
        </p>
        <div class="footer">
          <button type="button" class="btn-confirm"
           @click="confirm">确认</button>
           <button type="button" class="btn-cancel"
            @click="cancel">取消</button>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import { mapActions } from 'vuex'

export default {
  data () {
    return {
      maskShow: false
    }
  },
  computed: {
    modal () {
      return this.$root.MODAL
    },
    modalShow () {
      let showStatus = this.$root.MODAL.show
      if (showStatus) {
        this.maskShow = showStatus
      } else {
        setTimeout(() => {
          this.maskShow = showStatus
        }, 300)
      }
      return showStatus
    }
  },
  methods: {
    ...mapActions(['destroyModelMessage']),
    confirm () {
      if (this.modal.confirm) {
        this.modal.confirm()
      }
      this.destroyModelMessage()
      this.modal.show = false
    },
    cancel () {
      if (this.modal.cancel) {
        this.modal.cancel()
      }
      this.destroyModelMessage()
      this.modal.show = false
    }
  },
  created () {
  }
}
</script>

<style scoped>
.modal-mask {
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
}

.modal-content {
  position: absolute;
  left: 50%;
  top: 200px;
  width: 500px;
  height: 200px;
  margin-left: -250px;
  border-radius: 5px;
  background: #fff;
  .modal-title {
    font-size: 16px;
    text-align: center;
  }
  .modal-txt{
    padding: 30px;
    text-align: center;
  }
  .footer {
    display: flex;
    flex-flow: row;
    justify-content: center;
    .btn-cancel {
      width: 70px;
      height: 30px;
      border: 0;
      color: #fff;
      background: #eb7372;
      cursor: pointer;
    }
    .btn-confirm {
      width: 70px;
      height: 30px;
      margin-right: 10px;
      border: 0;
      color: #fff;
      background: #7bc585;
      cursor: pointer;
    }
  }
}

.slideIn-enter-active, .slideIn-leave-active {
  transition: all .3s ease-out;
  transform: scale(1);
}

.slideIn-enter, .slideIn-leave-active {
  transform: scale(0);
}
</style>
