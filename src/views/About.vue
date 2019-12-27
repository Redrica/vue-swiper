<template>
  <div class="about">
    <h1>This swiper acts as nav</h1>

    <swiper :options="mainSwiperOptions" ref="mySwiper">
      <swiper-slide v-for="(item, index) in mainSliderData" >
        <div class="slide">
          {{ item.text + (index + 1)}}
        </div>
      </swiper-slide>

      <div class="swiper-button-next swiper-button-white" slot="button-next"></div>
      <div class="swiper-button-prev swiper-button-white" slot="button-prev"></div>
    </swiper>

    <h2>These swipers are dependant</h2>

    <swiper :options="mainSwiperOptions" ref="depSwiper" v-for="(item, index) in swipersArr">
      <swiper-slide v-for="(item, index) in mainSliderData" >
        <div class="slide">
          {{ item.text + 'dependant ' + (index + 1)}}
        </div>
      </swiper-slide>
    </swiper>

  </div>
</template>

<script>
  import { swiper, swiperSlide } from 'vue-awesome-swiper';

  export default {
    name: 'my-swiper',

    components: {
      swiper,
      swiperSlide,
    },

    data() {
      return {
        mainSwiperOptions: {
          speed: 100,
          slidesPerView: 4,
          spaceBetween: 10,
          navigation: {
            nextEl: '.swiper-button-next',
            prevEl: '.swiper-button-prev'
          },
          slideToClickedSlide: true,
        },

        mainSliderData: [
          {
            text: 'I\'m Slide ',
          },
          {
            text: 'I\'m Slide ',
          },
          {
            text: 'I\'m Slide ',
          },
          {
            text: 'I\'m Slide ',
          },
          {
            text: 'I\'m Slide ',
          },
          {
            text: 'I\'m Slide ',
          },
          {
            text: 'I\'m Slide ',
          },
          {
            text: 'I\'m Slide ',
          },
          {
            text: 'I\'m Slide ',
          }
        ],

        swipersArr: '1111',
      }
    },

    mounted() {
      this.$nextTick(() => {
        const swiperTop = this.$refs.mySwiper.swiper;
        const swiperThumbs = this.$refs.depSwiper;
        //const swiperThumbs = this.$refs.depSwiper[0].swiper;

        console.log(this.$refs.depSwiper, ' массив зависимых КОМПОНЕНТОВ со слайдерами'); // массив, который объект. Оо. Чушь. object - это не объект, а возвращенный тип операнда. НЕ примитив => скорей всего получим object (м.б. и Array и Object)
        console.log('он действительно массив? - ', Array.isArray(swiperThumbs));
        console.log(typeof this.$refs.depSwiper, this.$refs.depSwiper.length, ' тип данных объекта и его длина'); // тип выводится как объект, длина - соответствует
        //console.log(this.$refs.depSwiper[0].swiper); // Swiper { options }

        // если надо совместить два слайдера - то достаточно такой записи:
        //swiperTop.controller.control = swiperThumbs;
        //swiperThumbs.controller.control = swiperTop;

        // если надо связать > 2 слайдеров
        swiperTop.controller.control = swiperThumbs; // контроль главным слайдером зависимых

        // получаем массив самих слайдеров в компонентах
        const depSwipers = [];
        swiperThumbs.forEach(it => depSwipers.push(it.swiper));
        console.log(depSwipers, ' массив зависимых слайдеров (конечные функциональные структуры)');

        // каждому слайдеру передаем массив контролируемых слайдеров
        for (let i = 0; i < depSwipers.length; i++) {
          let array = depSwipers.slice();
          let currentSwipersArray = array.splice(i, 1);
          console.log(i + '-й виток цикла', currentSwipersArray, ' извлеченный слайдер');
          console.log(i + '-й виток цикла', array, ' массив слайдеров после извлечения текущего слайдера');
          console.log(i + '-й виток цикла', depSwipers, ' depSwipers');

          depSwipers[i].controller.control = swiperTop;
        }

        console.log(depSwipers, ' после цикла');


        // let currentIndex;
        // let newThumbsArr = [];
        //
        // for (let i = 0; i < Array.from(swiperThumbs).length; i++) {
        //   newThumbsArr = swiperThumbs.slice(i, 1).push(swiperTop);
        //   // swiperThumbs[i].swiper.controller.control = [, ];
        //   // swiperThumbs[i].swiper.controller.control = newThumbsArr;
        // }
      })
    },
  }
</script>

<style lang="scss">
  @import '../../node_modules/swiper/dist/css/swiper.css';

  .swiper-slide:nth-child(4n+1) .slide {
    background-color: #7551ff;
  }

  .swiper-slide:nth-child(4n+2) .slide {
    background-color: #ffee7b;
  }

  .swiper-slide:nth-child(4n+3) .slide {
    background-color: #ff8271;
  }

  .swiper-slide:nth-child(4n+4) .slide {
    background-color: #3cffbc;
  }

  .slide {
    height: 200px;
    opacity: 0.4;
  }

</style>
