<template>
  <div id="wrapper">
    <nav class="navbar is-dark">
      <div class="navbar-brand">
        <router-link to="/" class="navbar-item"><strong>CompBuy</strong></router-link>
        <a class="navbar-burger" aria-label="menu" aria-expanded="false" data-target="navbar-menu" @click="showMobileMenu = !showMobileMenu">
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
        </a>
      </div>
      <div class="navbar-menu" id="navbar-menu" :class = "{'is-active': showMobileMenu}">
        
        <div class="navbar-start">
          <div class="navbar-item">
            <form method="get" action="/search">
              <div class="field has-addons">
                <div class="control">
                  <input type="text" name="query" class="input" placeholder="Nima qidirayapsiz?">
                </div>
                
                <div class="control">
                  <button class="button is-success">
                    <span class="icon">
                      <i class="fas fa-search"></i>
                    </span>
                  </button>
                </div>
              </div>
            </form>
          </div>
        </div>

        <div class="navbar-end">
          <router-link to="/computers" class="navbar-item">Computers</router-link>
          <router-link to="/keyboards" class="navbar-item">Keyboards</router-link>
          <router-link to="/printers" class="navbar-item">Printers</router-link>
          <router-link to="/mouses" class="navbar-item">Mouses</router-link>
          <router-link to="/other" class="navbar-item">Other equipments</router-link>

          <div class="navbar-item">
            <div class="buttons">
              <template v-if="$store.state.isAuthenticated">
                  <router-link to="/my-account" class="button is-light">My account</router-link>
              </template>
              <template v-else>
                  <router-link to="/log-in" class="button is-light">Log in</router-link>
              </template>
              <router-link to="/cart" class="button is-succes">
                <span class="icon"> <i class="fas fa-shopping-cart"></i></span>
                <span>Cart ({{cartTotalLength}})</span>
                

                
              </router-link>
            </div>
          </div>
        </div>
      </div>
    </nav>

    <div class="is-loading-bar has-text-centered" v-bind:class="{'is-loading': $store.state.isLoading}">
      <div class="lds-dual-ring"></div>
    </div>

    <section class="my-container">
      <router-view/>
    </section>

    <footer>
      <div class="my-footer">
        <div class="my-left">
          <div class="social">
            <a href="#"><i class="fab fa-facebook"></i></a>
            <a href="#"><i class="fab fa-instagram"></i></a>
            <a href="#"><i class="fab fa-telegram"></i></a>
          </div>

          <div class="contact">
            <h1 class="title">Biz bilan bog'lanish</h1>
            <div class="field">
              <label class="label">Email</label>
              <div class="control has-icons-left has-icons-right">
                <input class="input is-succes" type="email" placeholder="Emailingizni yozing...">
                <span class="icon is-small is-left">
                  <i class="fas fa-envelope"></i>
                </span>
              </div>
            </div>

            <div class="field">
              <label class="label">Habar matni</label>
              <div class="control">
                <textarea class="textarea" placeholder="Habaringizni yozing"></textarea>
              </div>
            </div>

            <div class="field">
              <div class="control">
                <button class="button is-link">Yuborish</button>
              </div>
            </div>
          </div>
        </div>

        <div class="my-right">
          <iframe src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3830.101679227346!2d71.77257440201002!3d40.36555243949973!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x38bb84ba1d76a659%3A0x7bc998c6a195e3d5!2z0KLQkNCi0KMg0KTQpA!5e0!3m2!1sru!2s!4v1656049787023!5m2!1sru!2s" style="border:0;" allowfullscreen="" loading="lazy" referrerpolicy="no-referrer-when-downgrade"></iframe>
        </div>
      </div>
    </footer>
  </div>  
</template>

<script>
import axios from 'axios'
export default{
  data(){
    return{
      showMobileMenu: false,
      cart:{
        items:[]
      }
    }
  },
  beforeCreate(){
    this.$store.commit('initializeStore')
    
    const token = this.$store.state.token

    if(token){
      axios.defaults.headers.common['Authorization'] = "Token " + token
    } else {
      axios.defaults.headers.common['Authorization'] = ""
    }
  },
  mounted() {
    this.cart = this.$store.state.cart
  },
  computed:{
    cartTotalLength(){
      let totalLength = 0
      for(let i = 0; i<this.cart.items.length; i++){
        totalLength += this.cart.items[i].quantity
      }
      return totalLength
    }
  }
}
</script>

<style lang="scss">
@import '../node_modules/bulma';
footer{
  padding-top: 50px;
}
#wrapper{
  margin: 0 0 0 0px;
  // width: 100vw;
  padding: 0;
  box-sizing: border-box;
}
.my-container{
  padding: 0;
}

.lds-dual-ring{
  display: inline-block;
  width: 80px;
  height: 80px;
  &:after{
    content: " ";
    display: block;
    width: 64px;
    height: 64px;
    margin: 8px;
    border-radius: 50%;
    border: 6px solid #ccc;
    border-color: #ccc transparent #ccc transparent;
    animation: lds-dual-ring 1.2s linear infinite;
  }
}

.is-loading-bar{
  height: 0;
  overflow: hidden;
  -webkit-transition: all 0.3s;
  transition: all 0.3s;
  &.is-loading{
    height: 80px;
  }
}

@keyframes lds-dual-ring{
  0%{
    transform: rotate(0deg)
  }
  100%{
    transform: rotate(360deg)
  }
}

.my-footer{
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  padding-bottom: 30px;
  padding-top: 30px;
  background-color: rgba($color: #000000, $alpha: 0.8);
  .my-left{
    width: 48vw;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    .social{
      width: 10%;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      a{
        width: 30px;
        height: 30px;
        margin: 5px 0;
        i{
          font-size: 30px;
          &:hover{
            color: blue;
            animation: social-hover 0.3s infinite;
          }
        }
      }
    }
    .contact{
      width: 88%;
      h1{
        color: white;
      }
      label{
        color: white;
      }
    }
  }
  .my-right{
    width: 48vw;
    iframe{
      width: 95%;
      height: 100%;
    }
  }
}

@keyframes social-hover{
  0%{
    transform: scale(1.2);
  }
  100%{
    transform: scale(1);
  }
}

</style>
