<script setup>
import { defineProps } from 'vue'

defineProps({
  messages: {
    type: Array,
    default: () => [],
    required: true
  }
})

function messageClassList (type, isLiked) {
  return [
    'message',
    `message_${type}`,
    { 'message_is-liked': isLiked }
  ]
}
</script>

<template>
  <div class="preview">
    <div class="device device_iphone-13-mini">
      <div class="messages">
        <div
          v-for="({ type, avatar, text, isLiked }, index) in messages"
          :key="index"
          :class="messageClassList(type, isLiked)"
        >
          <img
            v-if="type === 'sender'"
            :src="avatar"
            class="message__avatar"
          >
          <div class="message__text">
            {{ text }}
            <div
              v-if="isLiked"
              class="message__like"
            >
              ❤️
            </div>
          </div>
          </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.device {
  background-color: #fff;
  padding: 48px 24px;

  &_iphone-13-mini {
    width: calc(797px * 0.4);
    height: calc(1637px * 0.4);
    background-image: url('/img/iphone-13-mini.png');
    background-size: contain;
    background-repeat: no-repeat;
    background-position: center;
  }
}

.messages {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
  overflow: auto;
}

.message {
  display: flex;
  align-items: flex-end;
  margin-bottom: 8px;

  &__text {
    position: relative;
    line-height: 18px;
    color: rgb(38, 38, 38);
    padding: 12px 16px;
    border-radius: 22px;
    width: fit-content;
    max-width: 200px;
    min-height: 44px;
    word-break: break-all;
  }

  /deep/ &__avatar {
    width: 21px;
    height: 21px;
    border-radius: 50%;
    margin-right: 8px;
    overflow: hidden;

    &[src=""] {
      opacity: 0;
    }
  }

  &_is-liked {
    margin-bottom: 25px;
  }

  &_sender {
    .message__text {
      border: 1px solid rgb(239, 239, 239);
      background-color: #fff;

      .message__like {
        left: 0;
      }
    }
  }

  &_recepient {
    .message__text {
      margin-left: auto;
      background-color: rgb(239,239,239);

      .message__like {
        right: 0;
      }
    }
  }

  &__like {
    position: absolute;
    bottom: -20px;
    text-align: center;
    font-size: 12px;
    color: #ff0000;
    background-color: rgb(239,239,239);
    border-radius: 20px;
    border: 2px solid #fff;
    padding: 6px;
    width: 32px;
    height: 32px;
    z-index: 1;
  }
}
</style>
