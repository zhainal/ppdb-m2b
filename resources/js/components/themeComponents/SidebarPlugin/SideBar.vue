<template>
  <div class="sidebar"
       :style="sidebarStyle"
       :data-color="backgroundColor"
       :data-image="backgroundImage">
    <div class="sidebar-wrapper">
      <div class="logo">
        <div class="simple-text logo__container">
            <div class="logo-img">
                <img :src="logoImg" alt="">
            </div>
          {{title}}
        </div>
      </div>

      <slot name="content"></slot>
      <ul class="nav nav-main__links">
        <!--By default vue-router adds an active class to each route link. This way the links are colored when clicked-->
        <slot>
          <sidebar-link v-for="(link,index) in sidebarLinks"
                        :key="link.name + index"
                        :to="link.path"
                        @click="closeNavbar"
                        :link="link">
            <i :class="link.icon"></i>
            <p>{{link.name}}</p>
          </sidebar-link>
        </slot>
      </ul>
      <ul class="nav nav-bottom" v-if="$slots['bottom-links']">
        <slot name="bottom-links"></slot>
      </ul>
    </div>
  </div>
</template>
<script>
  import SidebarLink from './SidebarLink.vue'
  import { mapState } from 'vuex';

  export default {
    components: {
      SidebarLink
    },
    data() {
      return {
        logoImg: ''
      }      
    },
    props: {
      title: {
        type: String,
        default: process.env.MIX_APP_DISPLAY_NAME
      },
      backgroundColor: {
        type: String,
        default: 'black',
        validator: (value) => {
          let acceptedValues = ['', 'blue', 'azure', 'green', 'orange', 'red', 'purple', 'black']
          return acceptedValues.indexOf(value) !== -1
        }
      },
      backgroundImage: {
        type: String,
        default: 'https://1.bp.blogspot.com/-i31UtuSiDIQ/XuWBDa6imDI/AAAAAAAADUA/a2yMX1z3hU0rvi_Q0ey6gSpSeD-oynEeACLcBGAsYHQ/s1600/sidebar-5.jpg'
      },
      activeColor: {
        type: String,
        default: 'success',
        validator: (value) => {
          let acceptedValues = ['primary', 'info', 'success', 'warning', 'danger']
          return acceptedValues.indexOf(value) !== -1
        }
      },
      sidebarLinks: {
        type: Array,
        default: () => []
      },
      autoClose: {
        type: Boolean,
        default: true
      }
    },
    provide () {
      return {
        autoClose: this.autoClose
      }
    },
    mounted() {
      this.logoImg = this.getfoto();
    },
    computed: {
      ...mapState(['app_setting']),
	  sidebarStyle () {
        return {
          backgroundImage: `url(${this.backgroundImage})`
        }
      }
    },
    methods: {
      getfoto(){
        return "/images/logo_m2b.png";
      }
    }
  }

</script>
<style>
  .sidebar .sidebar-wrapper {
    display: flex;
    flex-direction: column;
  }
 .sidebar .nav-main__links {
   flex: 1;
 }
 .sidebar .sidebar-wrapper .logo .logo__container {
   padding-left: 10px;
 }
</style>
