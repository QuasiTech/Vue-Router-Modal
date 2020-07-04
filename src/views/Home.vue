<template>
  <div class="home">
    <button @click="showModal('info')">Info</button>
    <button @click="showModal('success')">Success</button>
    <v-modal :open="open" @close="open = false">
      <template slot="title" v-if="open">{{modalContents[currentModal].title}}</template>
      <template slot="content" v-if="open">{{modalContents[currentModal].content}}</template>
    </v-modal>
  </div>
</template>

<script>
  import vModal from '@/components/Modal.vue'

  export default {
    data() {
      return {
        open: true,
        modalContents: {
          info: {
            title: 'This Is Information',
            content: 'Info To Be Passed to The User'
          },
          success: {
            title: 'Success!',
            content: 'Request Completed'
          }
        },
        currentModal: 'info'
      }
    },
    components: {
      vModal
    },
    created() {
      this.checkModal();
    },
    watch: {
      $route(to,from) {
        this.checkModal();
      },
      open(to,from) {
        if (to == false) {
          if (this.$route.params.modal) {
            this.$router.push('/')
          }
        }
      }
    },
    methods: {
      checkModal() {
        if (this.$route.params.modal && this.modalContents[this.$route.params.modal]) {
          this.open = true;
          this.currentModal = this.$route.params.modal
        } else {
          this.open = false;
        }
      },
      showModal(name) {
        this.$router.push(name).catch(()=>{})
      }
    }
  }
</script>