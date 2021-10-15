<script setup>
import { ref, reactive, watch } from 'vue'
import { toPng } from 'html-to-image'
import download from 'downloadjs'

// eslint-disable-next-line
const emit = defineEmits(['messagesChange'])

const messages = reactive([])

const uploadedSendersAvatarURL = ref('')

watch([messages, uploadedSendersAvatarURL], () => {
  setSendersAvatar()
})

function setSendersAvatar () {
  if (messages[0].type === 'sender') messages[0].avatar = uploadedSendersAvatarURL.value

  const lastMessage = messages[messages.length - 1]
  const preLastMessage = messages[messages.length - 2]
  if (lastMessage.type === 'sender') {
    if (messages.length > 2 && preLastMessage.type === 'sender') preLastMessage.avatar = ''
    lastMessage.avatar = uploadedSendersAvatarURL.value
  }

  if (messages.length > 2 && preLastMessage.type === 'sender' && lastMessage.type === 'recepient') preLastMessage.avatar = uploadedSendersAvatarURL.value
}

function likeMessageClassList (isLiked) {
  return [
    'construct__like',
    { 'construct__like_is-liked': isLiked }
  ]
}

function pushMessage ({ type, avatar, text, isLiked }) {
  messages.push({
    type,
    avatar,
    text,
    isLiked
  })

  emit('messagesChange', messages)
}

function onAddMessage () {
  pushMessage({
    type: 'sender',
    avatar: '',
    text: '',
    isLiked: false
  })
}

function onDeleteMessage (index) {
  messages.splice(index, 1)
  emit('messagesChange', messages)
}

function onGenerateImage () {
  toPng(document.querySelector('.messages'))
    .then(dataUrl => {
      download(dataUrl, 'insta-messages-generator.png')
    })
}

function onLikeClick (index) {
  messages[index].isLiked = !messages[index].isLiked
}

async function onSendersAvatarChange (val) {
  const inputFileEl = document.querySelector('[name="sendersAvatar"]')
  const [file] = inputFileEl.files
  uploadedSendersAvatarURL.value = URL.createObjectURL(file)
}

function setDefaultMessages () {
  pushMessage({
    type: 'sender',
    avatar: '',
    text: 'Hello, Recepient!',
    isLiked: false
  })

  pushMessage({
    type: 'recepient',
    text: 'Hello, Sender!',
    isLiked: true
  })
}

setDefaultMessages()
</script>

<template>
  <div class="construct">
    <div class="construct-group">
      <label>Avatar</label>
      <input
        name="sendersAvatar"
        type="file"
        class="senders-avatar__input"
        @change="onSendersAvatarChange"
      >
    </div>
    <div
      v-for="(message, messageIndex) in messages"
      :key="messageIndex"
      class="construct-group"
    >
      <select
        v-model="message.type"
        :key="`${messageIndex}-type`"
        :name="`${messageIndex}-type`"
        class="construct__select"
      >
        <option value="sender">Sender</option>
        <option value="recepient">Recepient</option>
      </select>
      <input
        v-model="message.text"
        type="text"
        class="construct__input"
      >
      <div
        :class="likeMessageClassList(message.isLiked)"
        @click="onLikeClick(messageIndex)"
      >
        ❤️
      </div>
      <button
        class="construct__button construct__button_delete-message"
        @click="onDeleteMessage(messageIndex)"
      >
        Delete
      </button>
    </div>
    <button
      @click="onAddMessage"
      class="construct__button construct__button_add-message"
    >
      Add message
    </button>
    <button
      @click="onGenerateImage"
      class="construct__button construct__button_generate-image"
    >
      Generate image
    </button>
  </div>
</template>

<style lang="scss" scoped>
.construct {
  display: flex;
  flex-direction: column;
  margin-right: 32px;
  width: 400px;
  height: calc(1637px * 0.4);
  overflow: auto;

  &-group {
    display: flex;
    margin-bottom: 8px;
    width: 100%;
  }
  .senders-avatar {
    &__input {
      margin-left: 8px;
      flex: 1;
    }
  }

  &__select {
    margin-right: 8px;
  }

  &__input {
    margin-right: 8px;
    flex: 1;
  }

  &__like {
    font-size: 12px;
    text-align: center;
    margin-right: 8px;
    opacity: 0.3;
    cursor: pointer;
    transition: opacity 0.3s;

    &:hover {
      opacity: 0.6;
    }

    &_is-liked {
      opacity: 1;

      &:hover {
        opacity: 1;
      }
    }
  }

  &__button {

    &_add-message {
      margin-bottom: 8px;
    }

    &_delete-message {}
  }
}
</style>
