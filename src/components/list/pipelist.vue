<template>
  <div>
    <ul class="tlist">
      <li v-for="item in items" :key="'pipe:'+item.id" @click="$router.push(`/pipeline/info/${item.id}`)">
        <div class="tit">
          <div class="icons rotateDiv" v-if="item.build&&item.build.id!=''">
            <i class="iconfont icon-success color-success" style="font-size:20px" v-if="item.build.status=='ok'" />
            <i class="iconfont icon-chacha color-error" style="font-size:20px" v-else-if="item.build.status=='error'" />
            <i class="iconfont icon-jinzhide color-cancel" style="font-size:20px"
              v-else-if="item.build.status=='cancel'" />
            <i class="iconfont icon-jiazaizhong color-runing" style="font-size:18px" v-else />
          </div>
          <div class="tits">
            {{ item.name }}
            <span v-if="item.displayName&&item.displayName!=''">{{item.displayName}}</span>
          </div>
          <div class="tips" v-show="item.public=='1'" v-c-tooltip.hover.click="'所有人可查看相关流水线'">
            公开
          </div>
          <div style="flex:1"></div>
          <div class="tipln">
            <i class="iconfont icon-build" v-c-tooltip.hover.click="'构建次数'" />
            &nbsp;{{item.buildln}}
          </div>
          <slot :item="item"></slot>
        </div>
        <div class="infos">
          <div class="fg">{{$dateFmt(item.created)}}</div>
          <div class="fg" v-if="item.build&&item.build.id!=''">上一次构建:&nbsp;{{$dateFmt(item.build.created)}}</div>
          <div style="flex:1"></div>
          创建者:&nbsp;
          <myavatar :src="item.avat" :nick="item.nick" imgw="18px" class="avat" />
        </div>
        <!-- <div class="tools">

        </div> -->
      </li>
    </ul>
    <div class="loadingCont" v-if="loading">
      <div style="flex:1"></div>
      <div class="rotateDiv"><i class="iconfont icon-Loadingalt2" /></div>
      <div>加载中</div>
      <div style="flex:1"></div>
    </div>
    <div class="emptyCont" v-else-if="!items||items.length<=0"><i class="iconfont icon-jinzhide" />没有内容</div>
  </div>
</template>
<script>
import myavatar from "@/components/avatar";
export default {
  components: { myavatar },
  props: {
    items: Array,
    loading: Boolean,
  },
  /* watch: {
    param (nv) {

    }
  }, */
  data () {
    return {}
  },
}
</script>
<style lang="sass" scoped>
.tlist
  li
    padding: 10px
    border-bottom: 1px solid #eee
    cursor: pointer
    border-radius: 8px
    // background: #fefefe
    // margin-bottom: 8px
    &:hover
      background: #eee
    .tit
      display: flex
      .icons
        width: 25px
        margin-top: 5px
        margin-right: 5px
        line-height: 30px
      .tits
        color: #24292e
        font-size: 18px
        font-weight: bold
        line-height: 35px
        span
          font-size: 14px
          color: #888
          margin-left: 10px
      .tips
        color: #aaa
        font-size: 10px
        margin: 10px 0 0 10px
        width: 30px
        height: 20px
        border: 1px solid #ddd
        text-align: center
      .tipln
        color: #537898
        font-size: 10px
        margin: 5px 0 0 10px
        line-height: 20px

    .infos
      display: flex
      color: #aaa
      font-size: 12px
      line-height: 30px
      .fg
        margin-right: 10px
      .avat
        line-height: 30px
    .tools
      display: flex
</style>