<template lang="pug">
    include ../../tools/mixins.pug

    +b.index
        +e.ASIDE.sidebar.sidebar__(:class="{'active': status}")
            +e.BUTTON.button--hamburger.button--hamburger(type="button" :class="{'active': status}" @click="status= !status")
                span
                span
                span
            +e.wrapper
                +e.logo
                    h1 +MED summit 18.
                +e.list
                    +e.A.item(v-for="item in list")
                        span {{ item }}
                +e.BUTTON.button--contact.button(type="button")
                    span Contact us
        +e.content.content__(:class="{'active': status}" ref="content")
            +e.top(:style="{'height': height.top}")
                +e.background
                    img(src="public/media/bg.jpg")
                +e.H2.title--main #[span World largest medical summit] #[span Kuala-Lumpur] #[span Petronas Twin Towers]
                +e.event
                    +e.H2.date 19.10.2018 - 27.10.2018
                    +e.BUTTON.button--register.button--blue.--radius(type="button") Registration
                    +e.info
                        p Notax till 01.09.2018
                        a(href="#")
                            span Learn more
            +e.bottom(ref="bottom" :style="{'height': height.bottom}")
                +e.H3.title--bottom.--bold(ref="bottomTitle") Speakers:
                +e.slider.slider__.swiper-container(v-swiper:mySwiper="swiperOption" ref="slider")
                    .swiper-wrapper
                        +e.item.swiper-slide(v-for="item in 10")
                            +e.img
                                img(src="public/media/item.jpg")
                            +e.content
                                +e.P.name PhD John Hertz
                                +e.P.position Molecular surgery
                +e.invite
                    h3 Become a spaker
                    +e.BUTTON.button--apply.button--blue.--radius(type="button") Apply now
</template>

<script>

    export default {
        data() {
            return {
                list: ['Programm','Our speakers', 'Location', 'Taxes and fees','Become a speaker', 'About'],
                status: false,
                height: {
                    top: null,
                    bottom: null
                },
                swiperOption: {
                    freeMode: false,
                    init: true,
                    slidesPerView: 'auto',
                    spaceBetween: 10,
                    loop: true,
                    autoplay: {
                        delay: 5000
                    },
                    breakpoints: {
                        650: {
                            slidesPerView: 2,
                        },
                        400: {
                            slidesPerView: 1,
                        }
                    }
                },
            }
        },
        methods: {
            onResize() {
                if (window.innerWidth >= 650) {
                    console.log(this.$refs.bottomTitle.clientHeight,this.$refs.slider.offsetHeight,parseInt(this.getStyle(this.$refs.bottomTitle,'marginBottom')) ,parseInt(this.getStyle(this.$refs.bottom,'paddingTop')),parseInt(this.getStyle(this.$refs.bottom,'paddingBottom')))
                    this.height.bottom = `${this.$refs.bottomTitle.clientHeight + this.$refs.slider.clientHeight + parseInt(this.getStyle(this.$refs.bottomTitle,'marginBottom')) + parseInt(this.getStyle(this.$refs.bottom,'paddingTop')) + parseInt(this.getStyle(this.$refs.bottom,'paddingBottom'))}px`;
                    this.height.top = `${this.$refs.content.getBoundingClientRect().height - parseInt(this.height.bottom)}px`;
                } else {
                    this.height.bottom = null;
                    this.height.top = null;
                }
            },
            closeSidebar(e) {
                console.log(e)
                if(this.hasClass(e.target,'active')){
                    this.status = false;
                }
            },

            hasClass(element, className) {
                let rx = new RegExp('(?:^| )' + className + '(?: |$)');
                return rx.test(element.className);
            },
            getStyle(el, styleProp) {
                let value, defaultView = (el.ownerDocument || document).defaultView;
                // W3C standard way:
                if (defaultView && defaultView.getComputedStyle) {
                    // sanitize property name to css notation
                    // (hypen separated words eg. font-Size)
                    styleProp = styleProp.replace(/([A-Z])/g, "-$1").toLowerCase();
                    return defaultView.getComputedStyle(el, null).getPropertyValue(styleProp);
                } else if (el.currentStyle) { // IE
                    // sanitize property name to camelCase
                    styleProp = styleProp.replace(/\-(\w)/g, function(str, letter) {
                        return letter.toUpperCase();
                    });
                    value = el.currentStyle[styleProp];
                    // convert other units to pixels on IE
                    if (/^\d+(em|pt|%|ex)?$/i.test(value)) {
                        return (function(value) {
                            let oldLeft = el.style.left, oldRsLeft = el.runtimeStyle.left;
                            el.runtimeStyle.left = el.currentStyle.left;
                            el.style.left = value || 0;
                            value = el.style.pixelLeft + "px";
                            el.style.left = oldLeft;
                            el.runtimeStyle.left = oldRsLeft;
                            return value;
                        })(value);
                    }
                    return value;
                }
            }
        },
        mounted() {
            this.onResize();
            window.addEventListener('resize',this.onResize);

            if (window.innerWidth < 650) {
                window.addEventListener('click',this.closeSidebar);
            }
        },
        destroyed() {
            window.removeEventListener('resize', this.onResize);

            if (window.innerWidth < 650) {
                window.removeEventListener('click',this.closeSidebar);
            }
        }
    }
</script>