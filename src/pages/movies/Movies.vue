<template>
  <div class="movie-container">
    <nav>
      <ul>
        <li>
          北京
          <span class="yo-ico"></span>
        </li>
        <li>
          <ul>
            <router-link active-class="active" tag="li" to="/movies/intheater">正在热映</router-link>
            <router-link active-class="active" tag="li" to="/movies/comingsoon">即将上映</router-link>
          </ul>
        </li>
        <li>
          <span class="yo-ico">&#xf067;</span>
        </li>
      </ul>
    </nav>
    <section>
      <router-view></router-view>
    </section>
  </div>
</template>

<script>
import Vue from 'vue'
import { Icon } from 'vant'
Vue.use(Icon)

export default {
  
}
</script>

<style lang="stylus" scoped>
@import "~styles/border.styl"

.movie-container
  display flex
  flex-direction column
  height 100%
  nav
    height .44rem
    position static
    > ul
      height 100%
      display flex
      padding 0 .15rem
      border1px(0 0 1px 0)
      > li
        &:nth-child(1)
          flex 1
          font-size .15rem
          display flex
          align-items center
          color #666
          span
            font-size .12rem
            &::after
              content: '\f033'
        &:nth-child(2)
          flex 3
          ul
            display flex
            li
              flex 1
              line-height .44rem
              text-align center
              color #666
              margin 0 .1rem
              font-weight 700
              &.active
                border-bottom solid 2px #e54847
                height .44rem
                color #e54847
        &:nth-child(3)
          flex 1
          display flex
          justify-content flex-end
          align-items center
          span 
            font-size .16rem
            color #e54847
  section 
    flex 1
    overflow hidden

>>> .tab-content .list-wrap .item 
  padding-right .15rem !important
</style>