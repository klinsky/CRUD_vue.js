<template>
  <transition name="modal">
    <div class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">

          <div class="modal-header">
            <slot name="header">
              <span v-if="!isEdit">Добавление нового бота</span> 
              <span v-if="isEdit">Редактирование бота</span>
            </slot>
          </div>

          <div class="modal-body">
            <slot name="body">
              <form action="">
                <label>Название</label>
                <input v-model="botInfo.botName" placeholder="Введите название бота">
                <label>Описание</label>
                <textarea v-model="botInfo.botDescription" placeholder="Введите описание бота"></textarea>
                <label>Фото</label>
                <div class="dropBox-wrapper">
                  <div id="dropBox">
                      <div class="file-upload-form">
                          <label for="dragInput">Перетащите файл сюда...
                            <input id="dragInput" type="file" @change="previewImage" accept="image/*">
                          </label>
                      </div>
                  </div>
                  <div class="image-preview">
                      <div v-if="!botInfo.imageData.length">
                        preview
                      </div>
                      <div v-if="botInfo.imageData.length > 0">
                        <img class="preview" :src="botInfo.imageData">
                      </div>
                  </div>
                </div>
                <label class="mt-3" >Дата</label>
                <input type="date" v-model="botInfo.date"/>
              </form>
            </slot>
          </div>

          <div class="modal-footer">
            <slot name="footer">
              <button class="modal-default-button btn btn-outline-secondary" @click="$emit('close')">
                <span v-show="!isEdit">отмена</span> 
                <span v-show="isEdit">закрыть</span> 
              </button>
              <button 
                :disabled="!botInfo.botName" 
                class="modal-default-button btn btn-success" @click="save">
                сохранить
              </button>
            </slot>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>

  export default {
    name: 'Modal',
    props: {
      botInfo: {
        type: Object,
        default: function () {
          return {
            botName: '',
            botDescription: '',
            imageData: '',
            date: '',
          }
        },
      },
      isEdit: {
        type: Boolean,
        default: false,
      },
    },

    methods: {
      save () {
        this.$emit('save', { ...this.botInfo })
      },
      previewImage: function(event) {
            let input = event.target;
            if (input.files && input.files[0]) {
                let reader = new FileReader();
                reader.onload = (e) => {
                    this.botInfo.imageData = e.target.result;
                }
                reader.readAsDataURL(input.files[0]);
            }
        }
    },
  };
</script>

<style lang="scss">

input, optgroup, select, textarea {
  width: 100%;
  -webkit-box-shadow: 0px 0px 7px #afafaf;
          box-shadow: 0px 0px 7px #afafaf;
  border-radius: 5px;
  border: none;
  padding: 6px;
  margin-bottom: 8px;
    &:focus {
      outline-color: #689ed4;
    }
}
.dropBox-wrapper {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  #dropBox {
    position: relative;
    width: 80%;
    height: 80px;
    border: 2px dashed gray;
    border-radius: 5px;
    margin-right: 5px;
    background: lightyellow;
    background-size: 100%;
    background-repeat: no-repeat;
    text-align: center;
    div label {
      color: orange;
      font-size: 15px;
      font-family: Verdana, Arial, sans-serif;
    }
    label {
      display: -webkit-box;
      display: -ms-flexbox;
      display: flex;
      -webkit-box-align: center;
          -ms-flex-align: center;
              align-items: center;
      -ms-flex-wrap: wrap;
          flex-wrap: wrap;
      -webkit-box-pack: center;
          -ms-flex-pack: center;
              justify-content: center;
      height: 80px;
    }
  }
}

label {
    color: #777777;
    font-weight: 600;
}

.image-preview {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-align: center;
      -ms-flex-align: center;
          align-items: center;
  width: 20%;
  border: 2px dashed gray;
  border-radius: 5px;
  img {
    width: 100%;
    max-height: 76px;
  }
}

input[type="file"] {
  position: absolute;
  left: 0;
  top: 0;
  height: 80px;
  margin-bottom: 0;
  opacity: 0;
}

.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  -webkit-transition: opacity 0.3s ease;
  -o-transition: opacity 0.3s ease;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 360px;
  margin: 0px auto;
  overflow: auto;
  padding: 10px;
  background-color: #fff;
  max-height: calc(100vh - 20px);
  border-radius: 5px;
  -webkit-box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
          box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  -webkit-transition: all 0.3s ease;
  -o-transition: all 0.3s ease;
  transition: all 0.3s ease;
  font-family: Helvetica, Arial, sans-serif;
}

.modal-header  {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-pack: center;
      -ms-flex-pack: center;
          justify-content: center;
  font-weight: 600;
  h3 {
    margin-top: 0;
    color: #42b983;
  }
}

.modal-body {
  margin: 20px 0;
  text-align: left;
}

.modal-default-button {
  float: right;
}

.modal-enter {
  opacity: 0;
}

.modal-leave-active {
  opacity: 0;
}

.modal-enter .modal-container,
.modal-leave-active .modal-container {
  -webkit-transform: scale(1.1);
  -ms-transform: scale(1.1);
      transform: scale(1.1);
}

</style>
