<template>
  <div
    class="miniplayer"
    :class="{ miniplayer__show: currentPlaySong.id !== '' }"
    :style="miniPlayerStyle"
  >
    <div class="miniplayer-content" @click="handleNavigateTo('/pages/SongDetail/index')">
      <div class="cover">
        <div class="cover-pic">
          <img
            :src="currentPlaySong.coverImg ? currentPlaySong.coverImg : ''"
            class="cover-pic--image"
            alt=""
          />
        </div>
      </div>
      <div class="name">
        {{ currentPlaySong.name }} - {{ currentPlaySong.singer }}
      </div>
      <div class="contral">
        <!-- <i class="iconfont icon-zanting"></i> -->
        <i class="iconfont icon-bofang"></i>
      </div>
      <i class="iconfont icon-caidan" @click.stop="handleSetPlayerListShow"></i>
    </div>

    <!-- 播放列表 -->
    <player-list v-model="playerListShow"></player-list>
  </div>
</template>

<script setup lang="ts">
import { computed, ref, watch } from 'vue'
import { useStore } from 'vuex'
import PlayerList from '../PlayerList/index.vue'
import { useNavigateMethods } from '@/utils/global'

const store = useStore()
const currentPlaySong = computed(() => store.state.player.currentPlaySong)

const { handleNavigateTo } = useNavigateMethods()

/** 播放列表展示与否 */
const playerListShow = ref(false)
const handleSetPlayerListShow = () => {
  playerListShow.value = true
}

// 添加层级样式
const miniPlayerStyle = ref({})
watch(
  () => currentPlaySong.value,
  (newVal, oldVal) => {
    if (newVal) {
      setTimeout(() => {
        miniPlayerStyle.value = {
          zIndex: 10
        }
      }, 500)
    }
  }
)
</script>

<style lang="scss" scoped>
.miniplayer {
  position: fixed;
  left: 0;
  right: 0;
  bottom: 136rpx;
  height: 0;
  background-color: rgba(255, 255, 255, 0.96);
  border-top: 0.5rpx solid #f1f1f1;
  overflow: hidden;
  z-index: 9;
  transition: all 0.3s ease-in-out;

  &__show {
    height: 88rpx;
    overflow: inherit;
  }

  &-content {
    display: flex;
    align-items: center;
    position: relative;
    padding: 0 28rpx;

    .cover {
      display: flex;
      justify-content: center;
      align-items: center;
      position: absolute;
      top: -6rpx;
      width: 88rpx;
      height: 88rpx;
      background: url('http://img.fechen.cn/uploadfile/static/2022/01/11/player-disc.png')
        no-repeat;
      background-size: 100% 100%;

      &-pic {
        width: 60rpx;
        height: 60rpx;
        border-radius: 60rpx;
        overflow: hidden;

        &--image {
          width: 100%;
          height: 100%;
        }
      }
    }

    .name {
      flex: 1;
      height: 88rpx;
      line-height: 88rpx;
      margin: 0 28rpx 0 112rpx;
      @include --mixins-ellipsis(1);
    }

    .contral {
      margin-right: 28rpx;

      .iconfont {
        font-size: 58rpx;
        color: #4d4d4d;
      }
    }

    .icon-caidan {
      font-size: 40rpx;
      color: #4d4d4d;
    }
  }
}
</style>
