<template>
  <div>
    <h1>Список ботов</h1>
    <button 
      @click="createNewBot()" 
      type="button" 
      class="btn btn-outline-primary mb-4"
    >
      Создать бота
    </button>

    <div class="bot-wrapper"
      v-for="(bot, i) in bots"
      :key="i"
    >
    <div 
      @click="editBot(bot,i)"
      class="row"
    >
      <div class="name">
        {{ bot.botName }}
      </div>
      <div>
        <button @click.stop="removeBot(i, bot.botName)" class="btn btn-danger">удалить</button>
      </div>
    </div>

    </div>

    <BotModal
      v-show="isModalVisible"
      @close="closeModal"
      @show="showModal"
      @save="saveBot"
      :botInfo="botInfo"
      :isEdit="isEdit"
    />
  </div>
</template>

<script>
export default {
  name: 'BotsPage',
  components: {
    BotModal: () => import('./BotModal.vue')
  },
  data () {
    return {
      bots: [
          { 
            botName: 'firstBot',
            botDescription: 'firstBot',
            imageData: '',
            date: '2020-07-29',
          },
          {
            botName: 'secondBot',
            botDescription: 'secondBot',
            imageData: '',
            date: '2020-07-29',
          },
          {
            botName: 'thirdBot',
            botDescription: 'thirdBot',
            imageData: '',
            date: '2020-07-29',
          },
          {
            botName: 'fourthBot',
            botDescription: 'fourthBot',
            imageData: '',
            date: '2020-07-29',
          },
          {
            botName: 'fifthBot',
            botDescription: 'fifthBot',
            imageData: '',
            date: '2020-07-29',
          },
      ],
      botInfo: {
        botName: '',
        botDescription: '',
        imageData: '',
        date: '',
      },
      isModalVisible: false,
      isEdit: false,
      editRowKey: ''
    }
  },
  methods: {
    createNewBot() {
      this.isEdit = false;
      this.clear()
      this.showModal()
    },

    showModal() {
      this.isModalVisible = true;
    },
    closeModal() {
      this.isModalVisible = false;
      this.isEdit = false;
    },
    saveBot(info) {
      if(!this.isEdit) {
        this.bots.push(info)
        this.isModalVisible = false;
      } else {
        this.bots[this.editRowKey] = {...info}
      }
    },
    // Использую botName т.к id должно с сервера приходить =) 
    removeBot (key, botName) {
      this.bots = this.bots.filter(item => item.botName !== botName)
    },
    editBot (bot, key) {
      this.isEdit = true
      this.showModal()
      this.editRowKey = key
      this.botInfo = {...bot}
    },
    clear() {
      for(const prop in this.botInfo) {
        this.botInfo[prop] = ''
      }
    },
  }
}
</script>

<style scoped lang="scss">

h3 {
    margin: 40px 0 0;
  }

  .bot-wrapper {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    padding: 0 100px;
    @media(max-width: 992px) {
      padding: 10px;
      margin-left: 20px;
    }
    .row {
      display: -webkit-box;
      display: -ms-flexbox;
      display: flex;
      -webkit-box-align: center;
          -ms-flex-align: center;
              align-items: center;
      -ms-flex-wrap: nowrap;
          flex-wrap: nowrap;
      overflow: auto;
      width:100%;
      padding: 10px 0;
      border-bottom: 1px solid;

      &:hover {
        cursor: pointer;
        background-color: #ededed;
      }
      .name {
        width: 80%;
      }
    }

  }
</style>
