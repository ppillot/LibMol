<template>
    <div class="toolbar">
        <a class="pdb-code" 
          v-if="molHref !== ''" 
          :href="molHref"
          target="_blank">
          {{ molCode.toUpperCase() }}
        </a>
        <div class="molname">{{ molName }}</div>
        <div class="commands">
          <el-popover
            ref="settings"
            placement="bottom-end"
            width="400"
            trigger="click">
            <settings></settings>
          </el-popover>
          <!-- <el-popover
            ref="warninghidden"
            placement="bottom-end"
            width="400"
            trigger="click">
            <span>Some atoms are not currently displayed</span>
          </el-popover>
          <el-button v-show="isHidden" v-popover:warninghidden icon="warning" class="button warning" type="text" size="large"></el-button>
          -->
          <el-popover
            ref="measures"
            placement="bottom-end"
            width="400"
            trigger="click">
            <measures-panel></measures-panel>
          </el-popover>
          <el-button v-popover:measures class="button" type="text" size="small">
            {{ $t('ui.toolbar.measures.button') }}
          </el-button>
          <el-button class="button" type="text" size="large" @click="screenCapture">
            <i class="icon-camera"></i>
          </el-button>
          <el-button class="button" type="text" size="large" @click="toggleFullscreen" v-if="isFullscreenEnabled">
            <i :class="[isFullScreen ? 'icon-resize-small' : 'icon-resize-full']"></i>
          </el-button>
          <el-button v-popover:settings icon="setting" class="button" type="text" size="large"></el-button>
        </div>
    </div>
</template>

<script>
  import settings from './Settings'
  import measuresPanel from './MeasuresPanel'
  import Screenfull from 'screenfull'

  const PDBCodeRegEx = /^\d\w{3}$/i
  const PDBHeteroCode = /^\w{3}$/i

  export default {
    name: 'theToolbar',
    components: {
      settings,
      measuresPanel
    },
    data: function () {
      return {
        isFullScreen: false
      }
    },
    computed: {
      isFullscreenEnabled: function () {
        return Screenfull.enabled
      },
      molName: function () {
        return this.$store.state.name
      },
      isHidden: function () {
        return this.$store.state.isHidden
      },
      /* isFullScreen: function () {
        return this.$store.state.fullscreen
      }, */
      molCode: function () {
        return this.$store.state.molCode
      },
      molHref: function () {
        if (this.molCode !== '') {
          if (PDBCodeRegEx.test(this.molCode)) {
            return `http://www.rcsb.org/pdb/explore/explore.do?structureId=${this.molCode.toLowerCase()}`
          } else if (PDBHeteroCode.test(this.molCode)) {
            return `http://www4.rcsb.org/ligand/${this.molCode.toUpperCase()}`
          }
        }
        return ''
      }
    },
    methods: {
      toggleFullscreen () {
        // let view = document.getElementById('view')
        Screenfull.toggle()
        // this.$store.dispatch('toggleFullscreen')
      },
      screenCapture () {
        this.$store.dispatch('screenCapture')
      }
    },
    mounted: function () {
      if (Screenfull.enabled) {
        Screenfull.onchange(function () { // user can use ESC key to cancel fullscreen
          if (this.$store.state.fullscreen && Screenfull.isFullscreen === false) {
            this.$store.commit('setFullscreen', false)
          } else {
            this.$store.commit('setFullscreen', Screenfull.isFullscreen)
          }
          this.isFullScreen = Screenfull.isFullscreen
        }.bind(this))
      }
    }
  }
</script>

<style>
  .toolbar {
    width: 100%;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 2;
    background: rgba(249, 250, 252, 0.95);
    vertical-align: middle;
    display: flex;
    flex-direction: row;
    max-height: 2em;
    padding: 0.2em;
    transition: max-height 0.6s;
    align-items: center;
  } 

  .molname {
    flex: 1;
    white-space: nowrap;
    text-overflow: ellipsis;
        overflow: hidden;
  }

  .molname:hover {
    white-space: normal;
  }

  .toolbar:hover {
    max-height: none;
    transition: max-height 0.6s;
  }

  .button {
    margin: 0 0.2em;
    padding: 0;
    color: #88a9d4;
  }

  .warning {
    color: orangered;
  }
  
  .commands {
    margin-right: 0.5em;
  }

  .pdb-code {
    font-size: 0.8em;
    background: #E5E9F2;
    color: rgb(44, 62, 80);
    height: 1em;
    padding: 2px;
    border-radius: 3px;
    font-weight: 500;
    line-height: 1.1em;
    margin-right: 0.5em;
    text-decoration: none;
  }

  .pdb-code:hover {
    background: #FFF;
    color: #03a9f4

  }
</style>